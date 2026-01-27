---
title: Linux - 6. Buscar archivos y directorios
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-22 07:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

En pentesting, la información es poder, pero el exceso de información es caos. Cuando ejecutas herramientas como ffuf, dirb o nikto, pueden generar gigabytes de logs en minutos. Un pentester profesional sabe extraer lo valioso, silenciar el ruido y documentar todo sin perderse en el proceso. Las redirecciones son tu sistema de filtrado personal que separa vulnerabilidades reales de falsos positivos.

## Introduccion

Hoy vamos a hablar de algo que puede sonar técnico pero que te va a cambiar la vida cuando hagas pentesting: los descriptores de archivo y las redirecciones en Linux. Y sé lo que estás pensando: "¿Descriptores de archivo? Eso suena aburrido". Pero déjame decirte algo: dominar esto es como tener superpoderes cuando estás auditando sistemas.

Imagina que estás haciendo un escaneo con nmap a 1000 hosts. Tu terminal se llena de miles de líneas, algunas útiles, otras solo errores de conexión. ¿Cómo separas el oro de la basura? Exacto, con lo que vamos a aprender hoy.

## ¿Qué es un descriptor de archivo?





