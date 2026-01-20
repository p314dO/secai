---
title: Linux - 6. Buscar archivos y directorios
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-19 07:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

Cuando comprometemos un sistema Linux durante una auditoría de seguridad o pentesting, necesitamos localizar rápidamente archivos críticos: configuraciones con credenciales, scripts personalizados, logs con información sensible, backups de bases de datos, claves SSH, etc. Saber buscar eficientemente es la diferencia entre comprometer un sistema en minutos o en horas. Estas herramientas son fundamentales en las fases de post-explotación y escalada de privilegios.

##  Introduccion

Hoy vamos a hablar de algo absolutamente crítico para cualquier profesional de seguridad: cómo encontrar archivos y directorios en Linux de forma eficiente.

Imagínate esta situación: acabas de ganar acceso a un servidor Linux durante una auditoría de penetración. Tienes acceso limitado, tal vez solo como usuario estándar. ¿Qué haces ahora? Necesitas encontrar archivos de configuración que podrían contener contraseñas, scripts que podrían revelar rutas de escalación de privilegios, logs que contengan información sensible. Pero el sistema tiene miles, quizás millones de archivos.

¿Te vas a poner a navegar carpeta por carpeta manualmente? Por supuesto que no. Para eso tenemos herramientas especializadas que nos van a permitir encontrar exactamente lo que buscamos en cuestión de segundos.
Hoy vamos a dominar tres comandos esenciales: which, find y locate. Y de regalo, vamos a ver cómo combinarlos con grep para buscar no solo archivos, sino contenido dentro de ellos.

## Which

Empecemos con el más sencillo: **which**.
Este comando tiene una función muy específica pero súper útil: te dice dónde está ubicado un programa ejecutable. Piensa en which como un **"¿dónde vives?"** para comandos.
¿Por qué es importante esto en ciberseguridad? Porque cuando comprometes un sistema, una de las primeras cosas que necesitas saber es qué herramientas están disponibles. 

- ¿Tiene Python instalado?   
- ¿Está wget disponible?  
- ¿Y netcat?  
- ¿Puedo compilar código con gcc?

Mira qué simple es. Si quiero saber si Python está instalado y dónde se encuentra, simplemente escribo:
```
which python
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/48.png)

Y el sistema me responde con la ruta completa: `/usr/bin/python`. Perfecto, ahora sé que puedo usar Python para mis scripts de post-explotación.

![alt text](/assets/posts/2026-01-12-hands-on-linux/49.png)
> Buscamos mas herramientas de utilidad

**¿Y si el programa no existe?**: Simplemente no obtienes ninguna respuesta. Eso también es información valiosa: sabes que esa herramienta NO está disponible y tendrás que buscar alternativas.

> Un truco profesional: durante un pentesting, generalmente creas una lista de verificación de herramientas disponibles. Puedes automatizar esto con un simple script que verifique múltiples comandos a la vez.

## Find

Ahora vamos con la herramienta más poderosa y versátil: `find`.

Si `which` es un buscador simple, `find` es como tener un detective privado trabajando para ti. Puede buscar archivos:
- por nombre
- por tamaño
- por fecha de modificación
- por dueño
- por permisos...   
y puede ejecutar acciones automáticas sobre los resultados.
La sintaxis básica es super simple:
```
find [DÓNDE] [CRITERIOS] [ACCIÓN]
```

Déjame mostrarte un ejemplo real que usarías en pentesting. Imagina que quieres encontrar todos los archivos de configuración (que normalmente terminan en .conf) que:
- pertenecen al usuario root
- que sean mayores a 20 kilobytes
- y que hayan sido modificados después del 3 de marzo de 2020. 
Además, quieres ver los detalles completos de cada archivo.
Aquí está el comando completo:

```
find / -type f -name "*.conf" -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null 
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/50.png)

Sé que parece intimidante, pero vamos a descomponerlo parte por parte porque entender esto te va a hacer mucho más efectivo.

Empecemos: 
- La barra `/` le dice a find que busque desde la raíz del sistema, es decir, en TODO el sistema. 
- `-type` f especifica que solo queremos archivos (files), no directorios. Si quisiéramos solo directorios, usaríamos -type d.
- `-name "*.conf"` es nuestro filtro de nombre. El asterisco significa "cualquier cosa".
- `-user root` filtra solo archivos cuyo dueño es root. Crucial en pentesting porque estos archivos suelen contener información más sensible.
- `-size +20k` busca archivos mayores a 20 kilobytes. El signo más significa "mayor que".
- `-newermt 2020-03-03` encuentra archivos modificados después de esa fecha.
- `-exec ls -al {} \;` ejecuta un comando sobre cada resultado. Las llaves `{}` son un marcador de posición que se reemplaza con cada archivo encontrado.
- `2>/dev/null` redirige los errores al "agujero negro" del sistema. Esto evita que tu pantalla se llene de mensajes de "permiso denegado".


Déjame mostrarte ejemplos más prácticos para pentesting:

Buscar archivos que contengan "password" en su nombre:
```bash
find / -name "*password*" 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/51.png)

Buscar archivos con permisos SUID (oro para escalación de privilegios):
```bash
find / -perm -4000 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/52.png)

Buscar archivos modificados en las últimas 24 horas:
```bash
find / -type f -mtime -1 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/53.png)

## Operadores lógicos en find

`find` se vuelve más potente con operadores lógicos. Por defecto usa AND (todos los criterios deben cumplirse).

Para encontrar archivos .jpg O .png, usas `-o` (OR):

```bash
find . -name "*.jpg" -o -name "*.png"
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/54.png)


Para encontrar archivos que NO pertenezcan a root, usas `-not`:

```bash
find / -not -user root 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/55.png)

Esto es útil cuando buscas archivos de usuarios regulares que podrían haber dejado información sensible.

## El comando locate: velocidad sobre precisión


Esta herramienta es como el hermano rápido pero menos preciso de `find`. No busca en tiempo real, usa una base de datos pre-construida del sistema de archivos.

Primero actualizas la base de datos:

[VISUAL: Terminal ejecutando `sudo updatedb` con barra de progreso]

```bash
sudo updatedb
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/56.png)
Luego buscas súper rápido:

```bash
locate "*.conf"
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/57.png)

Resultados instantáneos. Muchísimo más rápido que `find`.

Pero tiene limitaciones: no tiene tantas opciones de filtrado como `find`, y solo muestra archivos que existían cuando se actualizó la base de datos. Si alguien creó un archivo hace 5 minutos, `locate` no lo encontrará hasta ejecutar `updatedb` de nuevo.

**¿Cuándo usar cada uno? **  
- `locate` para búsquedas rápidas cuando sabes más o menos qué buscas.   
- `find` cuando necesitas criterios específicos o buscas archivos muy recientes.

## Grep: buscando contenido dentro de archivos

Hasta ahora buscamos archivos por nombre, ubicación o propiedades. ¿Y si necesitas buscar dentro del contenido?

Aquí entra `grep`, herramienta esencial para pentesting.

```bash
grep "ERROR" server.log
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/58.png)

Muestra solo las líneas relevantes. Pero grep es mucho más poderoso con expresiones regulares.

El símbolo `^` significa "inicio de línea":

```bash
grep "^ERROR" *
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/59.png)

Solo encuentra líneas que EMPIEZAN con "Error".

El símbolo `$` significa "fin de línea":

```bash
grep "ok$" *
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/60.png)

Encuentra líneas que TERMINAN en "ok".

Pero lo realmente poderoso para pentesting son las búsquedas recursivas en directorios completos:

```bash
grep -r -i "password" /etc/ 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/61.png)

Esto busca "password" (ignorando mayúsculas con `-i`) en TODOS los archivos dentro de /etc/.

Buscar claves API:
```bash
grep -r "api_key" /var/www/ 2>/dev/null
```

Buscar credenciales de base de datos:
```bash
grep -r "mysql.*password" /home/ 2>/dev/null
```

## Combinando herramientas: el verdadero poder

Aquí es donde nos volvemos ninjas de Linux. Podemos combinar estas herramientas usando pipes.

Buscar todos los archivos .conf y luego buscar dentro de ellos la palabra "admin":

```bash
find /etc -name "*.conf" -exec grep -l "admin" {} \; 2>/dev/null
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/62.png)

El `-l` en grep hace que solo muestre nombres de archivos que contienen el patrón, no las líneas completas.

Usar `xargs` para mayor eficiencia cuando hay muchos archivos:

```bash
find /var/log -name "*.log" | xargs grep "Failed password"
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/63.png)
Esto te encuentra todos los intentos fallidos de login en los logs del sistema. Información valiosa para un análisis de seguridad.

## Resumen

Perfecto, hemos cubierto las herramientas fundamentales para búsqueda en Linux. Recapitulemos rápidamente:
- `which` para encontrar la ubicación de comandos ejecutables.
- `find` para búsquedas potentes y criterios específicos.
- `locate` para búsquedas rápidas por nombre.
- `grep` para buscar contenido dentro de archivos.

Estas herramientas son absolutamente fundamentales en pentesting. En la fase de post-explotación, saber buscar eficientemente puede ser la diferencia entre encontrar el vector de escalación de privilegios en 5 minutos o en 5 horas.
Mi recomendación: practica estos comandos hasta que se vuelvan segunda naturaleza. Crea tus propios "cheat sheets" con los comandos que más uses. Y lo más importante: experimenta en laboratorios seguros como los que te voy a dejar en los recursos.

## Video del post

{% include embed/youtube.html id='TU_VIDEO_ID_AQUI' %}

## Ejercicios Practicos

1. Básico
    - Usa `which` para verificar si están instalados: python3, nmap, netcat, wget, curl
    - Usa `locate` para encontrar todos los archivos .conf de SSH
    - Actualiza la base de datos con `sudo updatedb` y repite la búsqueda

2. Intermedio
    - Encuentra todos los archivos .log en /var/log mayores a 1MB modificados en los últimos 7 días
    - Busca en tu directorio home todos los archivos que NO pertenezcan a tu usuario
    - Encuentra archivos con permisos de escritura para "others" en /tmp: `find /tmp -type f -perm -o+w 2>/dev/null`

3. Desafío (orientado a ciberseguridad)
    - Encuentra todos los scripts bash (.sh) en el sistema y busca dentro de ellos la palabra "root"
    - Busca archivos de configuración (.conf) que contengan credenciales potenciales (password, key, secret, token)
    - Lista archivos modificados en las últimas 48 horas en /var/www que contengan "upload"







## Video del post

{% include embed/youtube.html id='' %}

