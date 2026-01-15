---
title: Linux - 4. Trabajar con Archivos y Directorios
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-15 07:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

El manejo eficiente de archivos y directorios desde la terminal es fundamental. Durante un pentest, necesitarás navegar rápidamente por sistemas comprometidos, organizar evidencias, crear scripts de automatización, y manipular archivos de configuración sin levantar alertas con interfaces gráficas. La velocidad y precisión en la terminal puede marcar la diferencia entre el éxito y el fracaso de una evaluación de seguridad.

## Diferencia entre Windows y Linux

Hoy vamos a hablar de algo que puede parecer básico, pero que es absolutamente crucial: trabajar con archivos y directorios desde la terminal.
Déjame hacerte una pregunta: ¿Cómo sueles buscar archivos en tu computadora? Probablemente abres el explorador de archivos, haces clic en carpetas, buscas visualmente... ¿verdad? Bueno, en Linux las cosas funcionan diferente y créeme, una vez que domines la terminal, no querrás volver atrás.

La terminal no solo es más rápida, es infinitamente más potente. Puedes manipular cientos de archivos simultáneamente, usar expresiones regulares para editar contenido, automatizar tareas repetitivas, y todo esto con unos pocos comandos. Es como comparar conducir un auto automático con uno de carreras de fórmula 1.

## Creando archivos

Empecemos por lo básico: crear archivos y directorios.
Para crear un archivo vacío, usamos el comando touch. Sí, literalmente "tocar". Piensa en ello como tocar algo para que exista. La sintaxis es super simple: touch seguido del nombre del archivo.
Déjame mostrarte un ejemplo práctico:
```
touch test
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/30.png)

Listo. Acabamos de crear un archivo vacío llamado test.txt. En un escenario real de pentesting, podrías usar esto para crear archivos de log donde registrar tus hallazgos, o archivos temporales para almacenar output de comandos.

## Creando directorios

Ahora, para crear directorios usamos mkdir, que significa "make directory" o crear directorio.

```
mkdir Folder
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/31.png)

Perfecto. Pero aquí viene algo interesante.

## Creando estructuras completas

¿Qué pasa si necesitas crear una estructura completa de directorios? Por ejemplo, durante un pentest, podrías querer organizar tus hallazgos así: `Evidencias/Cliente1/WebApp/Vulnerabilidades`
¿Tendrías que crear cada carpeta una por una? No. Aquí entra la opción -p de "parents" o padres.
```
mkdir -p Evidencias/Cliente/WebApp/Vulnerabilidades
```
Podemos visualizar esta estructura con el comando tree:
```
tree
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/32.png)

Este comando crea toda la estructura de una sola vez. La opción -p le dice a Linux: "si las carpetas padre no existen, créalas también".

## Creando archivos en rutas especificas

Ahora, algo muy importante en ciberseguridad: especificar rutas. Puedes crear archivos directamente en ubicaciones específicas sin necesidad de moverte allí primero.

```
touch ./Evidencias/Cliente1/WebApp/Vulnerabilidades/xss.txt
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/33.png)

## Renombrando archivos

Ahora viene una de mis partes favoritas: el comando `mv`, que significa "move" o mover.
Lo interesante de el comando `mv` es que hace dos cosas: mover Y renombrar archivos. Es como un 2 en 1.
Para renombrar un archivo:
```
mv test.txt info.txt
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/34.png)

¿Ves? Acabamos de renombrar test.txt a info.txt. Simple, rápido, efectivo.

## Moviendo archivos

Pero `mv` también mueve archivos entre directorios, y lo mejor es que puedes mover varios a la vez:
```
touch readme.txt
mv info.txt readme.txt Evidencias/
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/35.png)

Acabamos de mover ambos archivos a la carpeta Storage de una sola vez. En un escenario de post-explotación, esta habilidad es imprescindible. Imagina que encontraste archivos sensibles en diferentes ubicaciones y necesitas consolidarlos rápidamente para exfiltrarlos.

## Copiando archivos

Para copiar archivos, usamos cp, de "copy".

> **DIFERENCIA ENTRE MV Y CP**  
La diferencia entre mv y cp es simple: 
- mv mueve (el original desaparece) 
- cp copia (el original permanece) 

```
cp Evidencias/info.txt Folder/
```

Fíjate: ahora tenemos `info.txt` en dos lugares. Esto es perfecto cuando necesitas hacer backups de evidencias antes de modificarlas, o cuando quieres mantener copias de archivos de configuración originales antes de alterarlos.
![alt text](/assets/posts/2026-01-12-hands-on-linux/36.png)
Mira toda la estructura que hemos construido. Cuatro directorios, cuatro archivos, todo organizado. Y lo hicimos en cuestión de segundos con comandos simples.

---

### Resumen

Déjame decirte algo importante: lo que acabamos de ver son los fundamentos, los cimientos. Pero la terminal de Linux va MUCHO más allá.

En los próximos videos vamos a explorar técnicas avanzadas como la redirección de entrada y salida, que te permite encadenar comandos y manipular archivos de formas que ni te imaginas. También veremos editores de texto como vim y nano, herramientas que todo pentester profesional debe dominar.

## Video del post

{% include embed/youtube.html id='YtXdTRM90mQ' %}

### **TU MISIÓN AHORA**

Por ahora, tu tarea es clara: practica estos comandos hasta que los ejecutes sin pensar. La diferencia entre un pentester junior y uno senior no está en las herramientas que usa, sino en lo fluido que es con los fundamentos.
