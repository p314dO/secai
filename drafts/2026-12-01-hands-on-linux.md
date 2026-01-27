---
title: Linux - 1. Primeros Comandos
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-12 06:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

Cuando uno empieza a usar Linux, especialmente si viene de otros sistemas operativos, hay algo que genera mucha frustración muy rápido. Abrís la terminal, ejecutás un comando, y de repente aparecen un montón de opciones raras: guiones, letras sueltas, palabras largas con doble guion… y la sensación es siempre la misma: “¿cómo se supone que memorice todo esto?”.

La respuesta es simple y, al mismo tiempo, liberadora: no tenés que memorizar nada. En Linux, y más aún en ciberseguridad, lo importante no es saber todo de memoria, sino saber cómo pedir ayuda en el momento correcto.

## El comando ls: nuestro punto de partida

Vamos a empezar con un comando muy básico, pero que usamos todo el tiempo

```
ls
```

Sirve para listar archivos y directorios, es decir, para pedirle al sistema que nos muestre qué hay dentro de una carpeta.

![alt text](/assets/posts/2026-01-12-hands-on-linux/image.png)

Acá vemos carpetas como Documents, Downloads, Pictures. Hasta ahora, nada extraño. Pero ls tiene muchas más capacidades. Puede:
- mostrar archivos ocultos
- ordenar resultados
- cambiar el formato de salida
y mucho más. 

Y nadie espera que recuerdes todas esas opciones de memoria. Por eso, lo primero que tenemos que aprender no es un comando nuevo, sino cómo entender cualquier comando.

## Las páginas del manual: man

La forma más completa de obtener ayuda en Linux es usando el comando man.
```
man <tool>
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/2.png)

man viene de “manual”, y es exactamente eso: el manual oficial del comando, escrito por quienes lo desarrollaron.

Cuando abrís una página del manual, no es para leerla como una novela. Es un documento de consulta. Hay tres partes que siempre te van a dar contexto rápido.

![alt text](/assets/posts/2026-01-12-hands-on-linux/3.png)

1 - `Name`: Es una descripción corta que te dice exactamente qué hace el comando.  
2 - `Synopsis`: La forma general de uso.  
3 - `Description`: La lista de parámetros disponibles  

En ciberseguridad esto es clave. Antes de ejecutar un comando en un sistema real, productivo o comprometido, leer el manual te permite evitar errores graves, como borrar archivos que no deberías o ejecutar acciones irreversibles.

Si quieres salir del manual presiona la tecla `q`.

## Ayuda rápida: --help

Ahora bien, seamos realistas. Muchas veces no queremos leer todo el manual. Solo necesitamos una referencia rápida.

Para eso existe la opción --help.
```
--help
```
Esto muestra un resumen con las opciones más comunes, pensado para consultas rápidas.
![alt text](/assets/posts/2026-01-12-hands-on-linux/4.png)

Cuando trabajes con herramientas de pentesting como nmap, ffuf, sqlmap o nikto, esta ayuda rápida suele ser suficiente para empezar a trabajar sin perder tiempo.

## La opción -h: ayuda corta en algunas herramientas

Algunas herramientas usan una variante más corta, la opción -h. 

![alt text](/assets/posts/2026-01-12-hands-on-linux/5.png)

El resultado es similar: una ayuda compacta con las opciones más importantes. No todos los comandos funcionan igual, pero ahora ya sabés qué probar cuando te enfrentes a una herramienta nueva.

## Buscar comandos por palabra clave: apropos

Pero, ¿qué pasa si ni siquiera sabés qué comando necesitás?
Ahí entra en juego apropos.

```
apropos
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/6.png)

apropos busca palabras clave dentro de las descripciones de los manuales y te muestra todos los comandos relacionados.

Es como un buscador interno del sistema, basado en documentación oficial. Esto es especialmente útil cuando estás analizando un sistema desconocido durante un pentest o trabajando en una distribución que no conocés bien.

## Herramienta externa: ExplainShell

Y si alguna vez te encontrás con un comando largo, lleno de símbolos, copiado de un write-up o de un informe técnico, existe una herramienta excelente llamada [ExplainShell](https://explainshell.com/).

![alt text](/assets/posts/2026-01-12-hands-on-linux/7.png)

Pegás el comando y la página te explica cada argumento en lenguaje humano. 

![alt text](/assets/posts/2026-01-12-hands-on-linux/8.png)

Es perfecta para aprender, para revisar comandos antes de ejecutarlos, y para entender exactamente qué hace algo que encontraste en internet.

A partir de ahora vamos a usar muchos comandos nuevos. No espero que los memorices. Lo que sí espero es que desarrolles el hábito de consultar la ayuda, leer documentación y experimentar de forma consciente. En ciberseguridad, la curiosidad y la capacidad de aprender rápido valen mucho más que la memoria.
Si dominás cómo pedir ayuda en Linux, ya estás un paso adelante como profesional.

## Video del post

{% include embed/youtube.html id='IdVj-YxiKaU' %}

## Ejercicios Practicos

1. Básico 
    - Investiga el comando `pwd` usando man.
    - Identifica qué hace y qué información muestra.

2. Intermedio
    - Usa `apropos network` y elige un comando relacionado.
    - Analiza su propósito usando `man`.

3. Desafío (orientado a ciberseguridad)
    - Investiga `nmap` usando `man` y `--help`.
    - Identifica:
        - Una opción de escaneo
        - Una opción de detección de servicios
        - Una opción relacionada con scripts