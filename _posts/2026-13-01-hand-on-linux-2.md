---
title: Linux - 2. Reconocimiento Inicial
image:
    path: /assets/posts/2026-01-12-hands-on-linux/1.png
date: 2026-01-12 06:10:05 +/-TTTT
categories: [Linux]
tags: [Linux, Tutorial]
---

## Introduccion

El reconocimiento local es uno de los primeros pasos en pentesting, análisis forense y hardening. Saber quién sos en el sistema, qué kernel corre, qué grupos tenes y cómo está configurado el host permite identificar vectores de escalada de privilegios, configuraciones inseguras y superficies de ataque locales.

Cuando trabajamos con Linux —especialmente en ciberseguridad— no basta con “saber usar la terminal”.
Necesitamos **entender exactamente dónde estamos parados dentro del sistema**.

Imagina que acabas de conseguir acceso a un sistema Linux.
Puede ser tu propio servidor, una máquina de laboratorio… o un shell reverso durante un pentest.

Antes de tocar nada, antes de ejecutar exploits o herramientas avanzadas, hay una pregunta clave:

**¿Qué sistema es este… y quién soy yo dentro de él?**

Eso es lo que vamos a aprender hoy.

## Nombre del host

Linux nos ofrece una serie de comandos muy simples, casi aburridos a primera vista,
pero que en seguridad son oro puro.

Vamos a recorrer los más importantes y, en cada uno, voy a explicarte qué significa,
qué mirar, y por qué esto importa en ciberseguridad.

Empezamos con algo básico: el nombre del host.

```
hostname
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/9.png)

Este comando simplemente nos dice cómo se llama el sistema.

Ahora, esto parece trivial… pero no lo es.

En entornos corporativos, el hostname suele seguir un patrón:
producción, desarrollo, servidores críticos, bases de datos, etc.

En un pentest, el nombre del host puede darte pistas del rol del sistema dentro de la red. Como el siguiente ejemplo.

![alt text](/assets/posts/2026-01-12-hands-on-linux/10.png)

## ¿Quién soy?

Ahora vamos a una de las primeras cosas que debes ejecutar siempre.

```
whoami
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/11.png)

Este comando responde una pregunta crítica: **¿con qué usuario estoy ejecutando comandos?**

En ciberseguridad, esto define todo:
- Qué archivos puedes leer
- Qué procesos puedes matar
- Si puedes usar sudo
- Si eres un usuario sin privilegios… o no

Cada explotación, cada técnica de escalada, parte de aquí.

## Identidad y grupos

Ahora vamos un paso más allá.

```
id
```
![alt text](/assets/posts/2026-01-12-hands-on-linux/12.png)

Este comando amplía lo que vimos con whoami.

Vamos a descomponerlo:
- **uid=1000(student)** → ID del usuario
- **gid=1000(student)** → Grupo principal
- **groups=** → Todos los grupos secundarios

Y aquí está la parte interesante para seguridad.

![alt text](/assets/posts/2026-01-12-hands-on-linux/13.png)

El grupo adm permite leer archivos de /var/log.
Eso puede exponer:
- Logs de autenticación
- Logs de servicios
- Información sensible.  
El grupo sudo es aún más crítico.
Significa que este usuario puede ejecutar comandos como root, dependiendo de la configuración

> Para un pentester, esto puede ser una vía directa de escalada. Para un administrador, es una alerta: ¿este usuario realmente necesita estos permisos?

## Información del sistema

Ahora vamos a identificar el sistema operativo y el kernel.
Antes de usarlo, algo importante: leer las páginas de manual.
```
man uname
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/14.png)

Aquí es donde Linux deja de ser magia y pasa a ser una herramienta controlable.
El parametro mas usado es `-a`.

```
uname -a
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/15.png)

Vamos a interpretar esto correctamente.
- Linux → Nombre del kernel
- student-VirtualBox → Nombre del host
- 6.14.0-37-generic → Versión del kernel
- #37~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC … → Información de compilación
- x86_64 → Arquitectura
- GNU/Linux → Sistema operativo
¿Por qué esto es crítico en ciberseguridad?
Porque muchas vulnerabilidades dependen del kernel.

## Obtener solo la versión del kernel

```
uname -r
```

![alt text](/assets/posts/2026-01-12-hands-on-linux/16.png)

Con esto, un atacante —o un auditor— puede buscar:
- CVEs específicos
- Exploits locales
- Problemas de configuración conocidos

![alt text](/assets/posts/2026-01-12-hands-on-linux/17.png)

Esto no es hacking avanzado. Es reconocimiento inteligente.

---

Quiero que te quedes con esta idea:
> `Linux no se ataca a ciegas. Se entiende primero.`

Estos comandos no son relleno.
Son la base de:
- Pentesting
- Hardening
- Respuesta a incidentes
- Análisis forense.  
Dominar esto ahora te evitara errores graves más adelante.

## Video del post

{% include embed/youtube.html id='iSAbShLeYg4' %}

## Ejercicios Practicos

1.Ejecuta en tu máquina Linux:
- whoami
- id
- hostname
- uname -a 
Anota:
- Usuario
- Grupos interesantes
- Versión del kernel

2.Análisis de riesgo
Identifica:
- Qué grupos podrían representar riesgo (sudo, adm, docker, etc.)
- Qué información sensible podrías obtener solo con esos permisos

3.Enfoque pentesting
En una máquina de laboratorio (TryHackMe o HackTheBox):
- Obtén acceso inicial
- Ejecuta los comandos vistos
- Determina si existe una posible vía de escalada solo a partir de esta información
- Documenta tus hallazgos como si fuera un informe profesional.