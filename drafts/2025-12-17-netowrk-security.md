---
title: Network Security
image:
    path: /assets/posts/2025-12-17-network-security/1.jpg
date: 2025-12-17 05:09:45 +/-TTTT
categories: [DetectionEngineering]
tags: [DetectionEngineering, Blue Team, SOC, LetsDefend]
---

## Firewall

Un firewall es un dispositivo de seguridad de red que protege una red informatica de posibles amenazas maliciosas.
Controla el trafico etrante y saliente de la red para garantizar la seguridad de una red especifica creando una barrera entre dos redes.

Los firewalls se utilizan a menudo para separar la red privada de una empresa (normalmente una LAN(Local Area Network)) de una red publica como internet.

![alt text](/assets/posts/2025-12-17-network-security/HardwareFirewall.gif)

Un firewall permite o bloquea el paso de cierto trafico, generalmente examinando todo el trafico que entra y sale de la red.
Para esto los firewall suelen utilizar una politica de seguridad o un conjunto de reglas. Estas reglas determinan que tipos de trafico se permitiran y cuales se bloquearan.

Estos pueden ofrecer diversas funciones de seguridad:
- Identificar senales maliciosas (ataques o malware) mediante analisis de trafico de red.
- Permitir conexiones seguras (como VPN)

## Clasificaciones

### Packet Filtering

Los firewalls de capa de red operan en la capa 3 del modelo OSI y controlan el trafico de red segun las direcciones IP. Estos firewalls filtran el trafico examinando informacion a nivel de IP, como la direccion IP de origen, la direccion IP de destino, el tipo de protocolo y los numeros de puerto mediante metodos de filtrado de paquetes.

![alt text](./assets/posts/2025-12-17-network-security/layer3.jpeg)

Suelen ubicarse en la capa de red, responsable del enrutamiento y reenvio de paquetes de datos en una red. Normalmente se utilizan para filtrar el trafico entrante y saliente.

### Stateful Inspection

Estos firewalls operan en la capa 4 del modelo OSI y se basan en protocolos de capa de transporte como TCP y UDP. Analizan el trafico de red segun los numeros de puerto de origen y destino, el estado del enlace y el protocolo de transporte.

![alt text](./assets/posts/2025-12-17-network-security/layer4.png)

Estos tipos de firewalls también se conocen como firewalls con estado porque pueden rastrear el estado de una sesión o conexión. Es decir, pueden monitorear todas las etapas del recorrido de un paquete por la red y tomar decisiones en consecuencia. Esto proporciona una seguridad más sofisticada, ya que estos firewalls pueden tener en cuenta la actividad pasada de una sesión o conexión en particular.

### Application Level

Los firewalls de capa de aplicación operan en la séptima capa del modelo OSI y analizan los protocolos de nivel de aplicación y las comunicaciones de datos. Estos firewalls suelen realizar una inspección de datos mucho más profunda. Analizan el tráfico entrante a través de protocolos de aplicación como HTTP, HTTPS, FTP y DNS.

![alt text](./assets/posts/2025-12-17-network-security/layer7.png)

Los firewalls de capa 7 pueden determinar qué tipo de datos puede enviar y recibir una aplicación o servicio en particular. Por ejemplo, pueden examinar y controlar las solicitudes HTTP o HTTPS entrantes a una aplicación o servicio web. De esta manera, solo permiten ciertos tipos de solicitudes y bloquean otros.

## IDS - Intrusion Detection Systems

Los IDS generalmente monitorean y examinan el tráfico de red continuamente. Para ello, suelen ubicarse en el núcleo de la red, donde monitorean todos los paquetes de datos que entran y salen de internet u otras partes de la red. Actúan como observadores para garantizar que todo funcione correctamente y que nadie dañe la red.

Exiten tres tipos:
- Anomaly-based IDS
- Signature-based IDS
- Behavior-based IDS

### Basado en anomalias
Este tipo de IDS aprende los patrones típicos de tráfico de datos dentro de su red y le notifica en caso de anomalías. Por ejemplo, si un usuario observa que sus descargas de datos habituales son mínimas y de repente detecta un aumento significativo en la actividad de descarga, podría sospechar un ataque y activar una alerta

### Basado en firmas
Estos identificadores utilizan las "firmas" o patrones de ataques conocidos. Es decir, describen comportamientos dañinos y técnicas de ataque previamente conocidos. Esto es similar a cómo el software antivirus busca una firma específica de un virus. 

### Basado en el comportamiento
El IDS basado en comportamiento determina patrones de comportamiento normales y anormales mediante el análisis de datos como el tráfico de red, las actividades del sistema y el comportamiento del usuario. El sistema cuenta con un conjunto predefinido de reglas o algoritmos de comportamiento que se utilizan para detectar comportamientos inusuales o potencialmente peligrosos.

![alt text](./assets/posts/2025-12-17-network-security/ids.png)

Los productos IDS suelen funcionar analizando una copia reflejada del tráfico de red, ya que solo observan sin realizar ninguna acción directa. 

## IPS - Intrusion Prevention Systems
A diferencia de los IDS, el IPS se ubica en línea, lo que significa que el tráfico ingresa primero al dispositivo IPS. Tras analizar el tráfico, si lo considera malicioso, lo detiene; de ​​lo contrario, lo permite. La principal desventaja de este enfoque es el riesgo de retrasos en la red, lo que resulta en un conjunto de reglas más restringido en comparación con los IDS.

![alt text](./assets/posts/2025-12-17-network-security/ips.png)

Cuando un IPS detecta un patrón o firma de ataque específico, puede activar automáticamente una secuencia de respuestas. Estas suelen incluir acciones como bloquear paquetes maliciosos, redirigir el tráfico dañino o incluso ralentizar intencionalmente la red para impedir la actividad maliciosa.

## WAF - Web Application Firewall

Los firewalls de aplicaciones web están diseñados para contrarrestar las amenazas contra las aplicaciones web. Normalmente se ubican delante de una aplicación web y regulan todo el tráfico HTTP/HTTPS entrante. Estos sistemas analizan las solicitudes entrantes y las evalúan con un conjunto de reglas predefinidas para determinar si representan una amenaza o son benignas.

![alt text](./assets/posts/2025-12-17-network-security/waf.png)

## NAC - Network Access Control

Funciona como una solución de seguridad que regula el acceso a la red y define las actividades permitidas en ella.
Gestiona el tráfico de red entrante y saliente según directrices específicas. Estas directrices suelen tener en cuenta las credenciales del usuario, los atributos del dispositivo, los horarios y, en ocasiones, incluso la ubicación física del usuario.

Un sistema NAC generalmente consta de varios componentes:

**Autenticación**
NAC utiliza métodos de autenticación para verificar la identidad del usuario y su derecho a acceder a la red. Esto suele implicar el uso de datos como nombre de usuario y contraseña, o en ocasiones, un ID de hardware.

**Control de dispositivos**
NAC puede realizar una auditoría de dispositivos para garantizar que los dispositivos conectados a la red sean seguros y cuenten con parches de seguridad actualizados. Si la seguridad del dispositivo no es suficiente, NAC puede limitar o bloquear el acceso.

**Gestión de políticas**
NAC se utiliza para administrar e implementar políticas de red, siendo la gestión de políticas un pilar fundamental de su funcionalidad. Estas políticas determinan los privilegios de acceso a la red, los permisos de los dispositivos y las actividades autorizadas. Por ejemplo, pueden definir que una categoría específica de usuario pueda acceder a recursos de red designados o que un tipo de dispositivo específico tenga permiso para conectarse a la red.



