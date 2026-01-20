---
title: Linux - 5. Editar archivos en Linux
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-19 07:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

Saber editar archivos en Linux no es solo una habilidad técnica más: es una herramienta fundamental para cualquier persona que quiera dedicarse a la ciberseguridad.

En este sistema operativo, prácticamente todo se maneja a través de archivos de texto:
las configuraciones de red, los usuarios del sistema, los servicios activos, los logs de actividad, los scripts de automatización, y por supuesto los exploits.

Si no sabes cómo abrir, modificar y analizar estos archivos desde la terminal, estás trabajando con una mano atada.

## Introduccion

Si quieres convertirte en pentester, analista de seguridad o simplemente dominar Linux como un profesional, hay una habilidad básica que no puedes ignorar: **editar archivos de texto desde la terminal**.

Hoy vamos a aprender a usar Nano, un editor simple y amigable para empezar, y Vim, una herramienta potente que utilizan profesionales de la seguridad en entornos reales, incluso cuando solo tienen acceso a una terminal remota.

Dominar estos editores no solo te hará más rápido, sino también más eficiente, más preciso y mucho más profesional en tus auditorías de seguridad.

## Editar archivos con NANO

Imagina que Nano es como el bloc de notas de Linux: simple, directo y fácil de usar.

Para crear o editar un archivo con Nano, solo necesitas escribir:
```
nano learn_nano.txt
```
Esto crea el archivo si no existe, o lo abre si ya está creado.

![alt text](/assets/posts/2026-01-12-hands-on-linux/37.png)

Ahora estás dentro del editor.
Aquí puedes escribir lo que quieras, como si fuera un editor normal.

![alt text](/assets/posts/2026-01-12-hands-on-linux/38.png)

En la parte inferior verás comandos como:
- `^O` para guardar
- `^X` para salir
- `^W` para buscar  
El símbolo ^ significa Ctrl.

Por ejemplo, si presionas:
`Ctrl + W`
Aparece una barra de búsqueda.

![alt text](/assets/posts/2026-01-12-hands-on-linux/39.png)

Escribes una palabra, presionas Enter, y Nano te lleva directamente a esa parte del texto.
Esto es muy útil cuando analizas archivos largos como logs o configuraciones de servidores.

---

### Guardar y salir

Cuando termines de escribir:
- Presiona `Ctrl + O` para guardar
- Enter para confirmar
- `Ctrl + X` para salir

![alt text](/assets/posts/2026-01-12-hands-on-linux/40.png)

De vuelta en la terminal, puedes ver el contenido con:

```
cat learn_nano.txt
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/41.png)

---

## Archivos criticos para pentesting

Ahora viene lo interesante para ciberseguridad.

En Linux hay archivos que pueden revelar información sensible si están mal configurados.
Uno de los más importantes es:
```
/etc/passwd
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/42.png)

Este archivo contiene:
- Usuarios del sistema
- IDs de usuario
- Grupos
- Directorios personales  
Antes también guardaba contraseñas cifradas, pero ahora eso está en:
```
/etc/shadow
```
Si un atacante puede leer estos archivos, puede:
- Enumerar usuarios
- Identificar cuentas débiles
- Planear ataques de escalada de privilegios
Por eso, como pentesters, siempre revisamos permisos de archivos críticos.

![alt text](/assets/posts/2026-01-12-hands-on-linux/43.png)

## Introduccion a VIM
Ahora pasamos al editor profesional: **Vim**.

Vim no es solo un editor, es una herramienta de productividad extrema.

Se basa en una idea simple:
**No todo lo que escribes es texto. A veces son comandos.**

Por eso Vim tiene modos.

![alt text](/assets/posts/2026-01-12-hands-on-linux/44.png)

Empiezas en modo normal.
![alt text](/assets/posts/2026-01-12-hands-on-linux/45.png)

En este modo las teclas no escriben texto, escriben modos.  
Para escribir texto necesitas entrar en:
**Modo Insert** → presionando `i`.  
Para salir y volver al modo normal → `Esc`.

![alt text](/assets/posts/2026-01-12-hands-on-linux/5.gif)

---

### Modos principales de VIM

- **Normal** – Comandos
- **Insert** – Escribir texto
- **Visual** – Seleccionar texto
- **Command** – Ejecutar órdenes
- **Replace** – Sobrescribir
- **Ex** – Comandos encadenados

Para guardar y salir de Vim:
```
:wq
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/46.png)

**¿POR QUÉ VIM ES IMPORTANTE PARA ETHICAL HACKERS?**

Porque Vim:
- Funciona en cualquier sistema
- No depende de interfaz gráfica
- Es rápido
- Se integra con herramientas como grep, awk, sed

Si estás dentro de un servidor comprometido por SSH, **Vim** puede ser tu único editor disponible.

### Practicar con VIMTUTOR
Para aprender Vim de verdad, usa:
```
vimtutor
```

Es un tutorial interactivo oficial que te enseña paso a paso.

![alt text](/assets/posts/2026-01-12-hands-on-linux/47.png)

En 30 minutos puedes dominar lo básico.

## Video del post

{% include embed/youtube.html id='244OimnYq2c' %}

