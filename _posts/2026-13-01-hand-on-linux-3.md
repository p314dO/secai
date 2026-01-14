---
title: Linux - 3. Navegar por Linux
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-14 06:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

La navegación eficiente en Linux es fundamental en ciberseguridad porque la mayoría de servidores, herramientas de pentesting y entornos de análisis forense funcionan en Linux. Dominar la terminal no es opcional: es la diferencia entre un profesional efectivo y alguien perdido en la consola. Durante un test de penetración, necesitas moverte rápidamente por el sistema comprometido, localizar archivos de configuración, analizar logs y explorar directorios sin levantar alertas.Cada segundo cuenta.

## Contexto

Hoy vamos a hablar de algo que puede parecer básico pero que es absolutamente crítico en ciberseguridad: navegar por Linux desde la terminal.
Piénsalo así: en Windows usas el ratón para moverte entre carpetas, hacer clic aquí y allá. Es visual, es intuitivo. Pero en Linux, especialmente cuando estás haciendo pentesting o análisis forense, el ratón es casi inútil. Tu teclado es tu arma, y la terminal es tu campo de batalla.

¿Por qué es tan importante? Imagina que acabas de obtener acceso a un servidor durante una prueba de penetración. No hay interfaz gráfica. Solo una línea de comandos parpadeante esperando tus órdenes. Si no sabes moverte rápidamente, localizar archivos críticos o entender la estructura del sistema, estás perdido.


## ¿Donde estoy?
Cuando abres una terminal en Linux, no siempre es obvio en qué carpeta te encuentras. El comando que necesitas es pwd, que significa "print working directory". Es como preguntarle al sistema: "oye, ¿dónde estoy parado ahora mismo?"

```
pwd
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/18.png)

Este comando responde una sola pregunta:
¿en qué directorio estoy ahora mismo?

En ciberseguridad, ejecutar un comando en la ruta equivocada puede:
- borrar archivos críticos
- contaminar evidencia
- levantar alertas de seguridad

---

## Ver el contenido de un directorio
Perfecto. Estamos en nuestro directorio home. Ahora, ¿qué hay aquí? Para eso usamos ls, que ya lo vimos en la clase numero 1, que simplemente lista el contenido.

```
ls
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/19.png)

## Listado detallado

Bien, vemos carpetas. Pero esto es información muy básica. En ciberseguridad necesitamos más detalles: permisos, propietarios, tamaños. Aquí es donde ls -l se vuelve tu mejor amigo.

```
ls -l
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/20.png)

Ahora sí. Mira toda esta información. Vamos a desglosarla porque cada columna tiene un propósito.

- Primera columna: `drwxr-xr-x`. Estos son los permisos del archivo o directorio. La 'd' inicial significa que es un directorio. Las letras que siguen (rwx) indican quién puede leer, escribir o ejecutar. Esto es crítico en seguridad porque te dice quién tiene acceso a qué.
- Segunda columna: el número '2' indica `hardlinks`. No te preocupes mucho por esto ahora.
- Tercera y cuarta columnas: 'student student'. El primer 'student' es el propietario, el segundo es el grupo. Cuando estás auditando un sistema, estos datos te dicen quién controla cada archivo.
- Quinta columna: 4096. El tamaño en bytes. Para directorios, esto indica bloques usados, no el tamaño real del contenido.
- Sexta columna: la fecha y hora de última modificación. En análisis forense, estos timestamps son oro puro.

Y finalmente, el nombre del directorio o archivo.

Pero espera, aquí hay algo que no estás viendo todavía.

## Archivos Ocultos

En Linux, los archivos que comienzan con un punto son archivos ocultos. Son archivos de configuración, historiales, cosas que el sistema no muestra por defecto. Y justamente esos archivos son los que más nos interesan en ciberseguridad. Para verlos usamos

```
ls -la
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/21.png)

Ahí está. Mira .bash_history. Este archivo guarda todos los comandos que has ejecutado. Durante un test de penetración, si comprometes una cuenta, este archivo te dice exactamente qué ha estado haciendo ese usuario.

Fíjate también en los dos primeros elementos: el punto simple '.' y el punto doble '..'. El punto simple representa el directorio actual. El doble punto representa el directorio padre, es decir, el directorio que está un nivel arriba.

## Moverse entre directorios

Ahora que sabemos listar, aprendamos a movernos. El comando es cd, que significa "change directory".

```
cd
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/22.png)

Fíjate cómo cambió el prompt. Ahora muestra que estamos en ~/Desktop/Tools. La virgulilla '~' es un atajo que significa tu directorio home.

## Volver al directorio anterior

Algo muy útil: si quieres volver al directorio donde estabas antes, no necesitas escribir toda la ruta de nuevo. Simplemente usa cd -.

```
cd -
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/22.png)

Boom. Regresaste.

## Autocompletado

Ahora, un truco que te va a ahorrar muchísimo tiempo: el autocompletado con TAB.

![alt text](/assets/posts/2026-01-12-hands-on-linux/gif1.gif)

Cuando escribes cd Des y presionas TAB, el shell completa automáticamente a Desktop si es la única opción. Si hay múltiples opciones, presiona TAB dos veces y te mostrará todas las posibilidades.

Esto no es solo comodidad. Durante un engagement de pentesting, cuando estás bajo presión y el tiempo corre, la velocidad importa. El autocompletado también evita errores de tipeo que podrían ejecutar comandos equivocados.

## Listar contenido sin navegar a la carpeta

Un detalle más sobre navegación: no necesitas estar físicamente en un directorio para ver su contenido.

```
ls -l Desktop/Tools/
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/24.png)

![alt text](/assets/posts/2026-01-12-hands-on-linux/25.png)

Puedes especificar la ruta completa. El directorio /var es especialmente interesante en seguridad porque contiene logs, archivos temporales, datos de aplicaciones. Es un tesoro de información.

## Navegacion rapida

Hablemos de navegación rápida. Si estás varios directorios adentro y quieres ir directamente a otro lugar, puedes usar rutas completas:

```
cd Desktop/Tools/Directory1/Directory2/Directory3/
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/26.png)

O puedes usar el doble punto para subir niveles:

```
cd ..
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/27.png)

Esto te sube un nivel. Si quieres subir dos niveles, usas 
```
cd ../..
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/28.png)

## Limpiar la terminal

Algo que vas a hacer constantemente es limpiar tu terminal. Después de ejecutar varios comandos, la pantalla se llena y se vuelve confusa.

![alt text](/assets/posts/2026-01-12-hands-on-linux/29.png)

Puedes usar el comando `clear` o el atajo de teclado `Ctrl+L`.

![alt text](/assets/posts/2026-01-12-hands-on-linux/gif2.gif)

# Usar el historial

Finalmente, dos herramientas más para navegar.

1) Las flechas arriba y abajo te permiten moverte por tu historial de comandos. No necesitas reescribir comandos largos que ya ejecutaste.

![alt text](/assets/posts/2026-01-12-hands-on-linux/gif3.gif)

Y si necesitas buscar un comando específico que ejecutaste hace rato, usa Ctrl+R.

![alt text](/assets/posts/2026-01-12-hands-on-linux/gif4.gif)

Escribes parte del comando y el shell busca en tu historial. Es increíblemente útil cuando estás repitiendo procesos similares durante una auditoría.

---

## Resumen

Bien, recapitulemos rápidamente. Hoy aprendiste:
- pwd para saber dónde estás
- ls con sus variantes para listar contenido
- cd para moverte entre directorios
- Atajos como TAB, Ctrl+L, Ctrl+R
- La importancia de los archivos ocultos

Estos comandos son tus fundamentos. Son las bases sobre las que construirás todo lo demás en tu carrera de ciberseguridad. Practica hasta que se vuelvan automáticos, hasta que tus dedos los escriban sin pensar.

---

## Video del post

{% include embed/youtube.html id='hyNEd6CUV7s' %}

---

## Ejercicios practicos
1. EJERCICIO 1 - BÁSICO: Exploración Inicial
**Objetivo**: Familiarizarte con la navegación básica
- Abre tu terminal y determina en qué directorio estás usando pwd
- Lista todo el contenido visible del directorio actual
- Lista TODO el contenido incluyendo archivos ocultos
- Identifica al menos 3 archivos ocultos y anota sus nombres
- Navega al directorio /etc y lista su contenido con detalles

**Pista**: Presta atención a los permisos de los archivos ocultos. ¿Por qué crees que `.bash_history` tiene permisos `-rw-------`?

2. EJERCICIO 2 - INTERMEDIO: Navegación Eficiente
**Objetivo**: Dominar el movimiento rápido por el sistema
- Desde tu directorio home, navega a /var/log en un solo comando
- Lista los archivos y ordena mentalmente cuáles podrían contener información de seguridad
- Regresa a tu directorio home usando el atajo más rápido
- Crea la siguiente estructura de directorios en tu Desktop: Practice/Security/Logs
- Navega a Practice/Security/Logs usando autocompletado TAB
- Vuelve al directorio Practice usando cd .. dos veces
- Regresa a Logs usando cd - y observa qué sucede
**Pista**: El comando cd - solo te lleva al directorio previo inmediato, no funciona como un historial múltiple.

3. EJERCICIO 3 - DESAFÍO: Reconocimiento de Sistema (Escenario de Pentesting)
**Objetivo**: Simular las primeras acciones después de obtener acceso a un sistema
Escenario: Has obtenido acceso SSH a un servidor durante un test de penetración. Tu objetivo es realizar reconocimiento inicial sin levantar alertas.
Tareas:
- Determina tu directorio actual y tu nombre de usuario
- Lista todos los usuarios con directorio home en /home/
- Explora /etc/ y localiza el archivo passwd (contiene información de usuarios)
- Navega a /var/log/ e identifica cuáles archivos de log podrían revelar actividad reciente
- Busca en tu directorio home archivos que contengan historial de comandos (pista: comienzan con punto)
- Usa Ctrl+R para buscar en tu historial si has ejecutado el comando sudo anteriormente
- Documenta 5 directorios que consideres críticos para un pentester y por qué

Criterios de éxito:
- No ejecutes ningún comando que modifique archivos
- Usa solo comandos de navegación y listado
- Anota todos los comandos que ejecutaste (simula un engagement real)

**Pregunta de reflexión**: ¿Qué información valiosa encontraste que podría ayudarte a elevar privilegios o moverse lateralmente en la red?