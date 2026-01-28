---
title: Introduccion a la seguridad en AWS
image:
    path: /assets/posts/2026-28-01-aws-security/1.png
date: 2026-01-28 06:00:00 +/-TTTT
categories: [AWS]
tags: [AWS, Security]
---

Imagina la nube de AWS como un gran lienzo rectangular: un espacio privado y seguro donde desplegamos los recursos de nuestras propias cuentas. Es aquí, dentro de estos límites, donde la infraestructura cobra vida según nuestras necesidades.

![Image](./assets/posts/2026-28-01-aws-security/2.png)

El componente fundamental de la arquitectura en AWS es la Nube Privada Virtual (VPC). En nuestro diagrama, está representada por el rectángulo verde: un entorno de red aislado de forma lógica donde puedes desplegar tus recursos con total control sobre la seguridad y la conectividad.

![Image](./assets/posts/2026-28-01-aws-security/3.png)

Al crear e iniciar sesión en una cuenta de AWS, el sistema genera automáticamente una "VPC predeterminada" en su nombre. Dentro de este entorno, es posible desplegar una amplia variedad de servicios. Aunque no todos los recursos de AWS requieren una VPC, una gran parte de la infraestructura principal sí lo hace.

Por ejemplo, al lanzar una instancia EC2, esta debe alojarse dentro de los límites de su VPC. No obstante, para que este despliegue sea posible, es necesario definir primero subredes (subnets) que segmenten el espacio de red de su Nube Privada Virtual.

<!-- ![Image](./assets/posts/2026-28-01-aws-security/4.png)

![Image](./assets/posts/2026-28-01-aws-security/5.png) -->

![Image](./assets/posts/2026-28-01-aws-security/6.png)

Estas subredes cumplen múltiples funciones, pero su propósito principal es la segmentación lógica de la red, de manera similar a la creación de redes independientes mediante conmutadores físicos. Esta estructura es fundamental por las siguientes razones:

- **Seguridad avanzada**: Permiten aislar recursos críticos en subredes privadas, restringiendo el acceso desde el internet público y manteniendo un control total sobre el flujo de tráfico entrante y saliente.

- **Organización estratégica**: Facilitan la asignación de propósitos específicos a cada segmento, permitiendo, por ejemplo, dedicar subredes exclusivas para bases de datos, servidores de aplicaciones o servidores web.

Además, existen recursos que se despliegan dentro de la VPC pero operan en conjunto con las subredes para optimizar la arquitectura, tales como Amazon EFS para almacenamiento compartido, Amazon ElastiCache para la gestión de caché, o Elastic Load Balancers (ELB) para la distribución inteligente de carga entre instancias.

![Image](./assets/posts/2026-28-01-aws-security/7.png)

Fuera de los límites de la VPC, pero aún dentro del ecosistema global de AWS, existen servicios fundamentales que operan a nivel de región o de red perimetral:

**Almacenamiento de objetos**: Amazon S3 permite el almacenamiento seguro de archivos estáticos y copias de seguridad de forma escalable.

**Distribución de contenido (CDN)**: Amazon CloudFront optimiza la entrega de aplicaciones web al posicionarse frente a ellas para reducir la latencia global.

**Seguridad perimetral**: Para proteger la infraestructura y las instancias, implementamos AWS WAF (Web Application Firewall) contra exploits web comunes y AWS Shield para mitigar ataques DDoS.

Y para gestionar el flujo de tráfico hacia y desde el internet público, es fundamental contar con un sistema de resolución de nombres. Para ello, utilizamos Amazon Route 53, el servicio de DNS escalable de AWS que permite enrutar las solicitudes de los usuarios de manera eficiente hacia nuestra infraestructura.

![Image](./assets/posts/2026-28-01-aws-security/8.png)

## Consideraciones de seguridad en nuestra arquitectura

Esta estructura se define como una arquitectura multicapa, donde los servicios se organizan en niveles especializados para optimizar la escalabilidad y la seguridad. Al segmentar la infraestructura en un nivel de presentación, un nivel lógico y un nivel de datos, implementamos lo que se conoce específicamente como una arquitectura de tres niveles (3-Tier Architecture).

![Image](./assets/posts/2026-28-01-aws-security/9.png)

Al implementar una arquitectura de este tipo, la funcionalidad es solo el primer paso; la seguridad es el pilar que garantiza su sostenibilidad. Como profesionales de ciberseguridad, nuestra responsabilidad es identificar vectores de ataque y diseñar defensas proactivas bajo un enfoque de "Defensa en Profundidad".

Para asegurar esta implementación de manera integral, debemos priorizar estas cinco categorías críticas:

**Network Security**: Control de tráfico mediante Grupos de Seguridad (Security Groups), Listas de Control de Acceso (NACLs) y el uso de AWS WAF para filtrar peticiones maliciosas.

**Data Protection**: Implementación de cifrado tanto en reposo (at rest) como en tránsito (in transit) utilizando servicios como AWS KMS.

**IAM (Identity and Access Management)**: Aplicación del principio de privilegio mínimo para gestionar identidades, roles y permisos de acceso a los recursos.

**Logging & Monitoring**: Auditoría continua mediante AWS CloudTrail y Amazon CloudWatch para detectar anomalías y comportamientos sospechosos en tiempo real.

**Business Continuity**: Estrategias de respaldo y recuperación ante desastres para garantizar que la infraestructura sea resiliente frente a fallos o ataques disruptivos.

## Network Security

Al tratarse de una arquitectura diseñada para una aplicación web, la principal exposición radica en el flujo de peticiones externas. Las solicitudes de los usuarios atraviesan múltiples capas: ingresan a la VPC, recorren las subredes hasta alcanzar los servidores web y de aplicaciones, y finalmente interactúan con la base de datos. Cada punto de contacto en este trayecto representa un vector de ataque potencial que debe ser blindado.

![Image](./assets/posts/2026-28-01-aws-security/10.png)

Esta exposición inherente genera diversos vectores de ataque que pueden comprometer la integridad de la plataforma. Entre las amenazas más críticas se encuentran:

- **Ataques de inyección**: Tales como XSS, inyecciones SQL y ejecución de comandos del sistema operativo.
- **Ataques de Denegación de Servicio (DoS/DDoS)**: Diseñados para interrumpir la disponibilidad del servicio.
- **Fallas en el control de acceso**: Que facilitan la escalada de privilegios y el acceso no autorizado.
- **Fugas de información**: Exposición de datos sensibles y confidenciales.

Para mitigar estos riesgos, es imperativo fortalecer la seguridad de la red mediante la implementación de controles robustos:
- **Protección contra DDoS**: Uso de servicios perimetrales para absorber y filtrar tráfico malicioso.
- **Segmentación de red**: Configuración estratégica de subredes para aislar componentes críticos.
- **Filtrado de tráfico**: Implementación de Listas de Control de Acceso a la Red (NACL) a nivel de subred y Grupos de Seguridad (Security Groups) a nivel de instancia para permitir únicamente el tráfico legítimo.

![Image](./assets/posts/2026-28-01-aws-security/11.png)

Con esta base establecida, el siguiente paso es integrar capas de defensa perimetral, como AWS WAF (Web Application Firewall) y una CDN (Amazon CloudFront).

Este enfoque de defensa en profundidad permite filtrar y reducir drásticamente el volumen de tráfico malicioso antes de que alcance la VPC. Incluso si un ataque logra superar el perímetro inicial, la estructura multicapa garantiza que el impacto sea contenido, dificultando cualquier movimiento lateral o daño a los activos críticos.

## Estrategias de protección de datos en la nube

Más allá de la seguridad perimetral, la protección de datos es un pilar crítico que abarca toda la información almacenada en Amazon S3, instancias EC2 y bases de datos.

En el caso de las instancias EC2, la persistencia se gestiona a través de Elastic Block Store (EBS). Estos volúmenes, ya sean basados en SSD de alto rendimiento o HDD, almacenan los datos operativos que los servidores web y de aplicaciones consumen directamente, exigiendo estrategias de cifrado y respaldo para garantizar su integridad.

![Image](./assets/posts/2026-28-01-aws-security/12.png)

Esto implica que los volúmenes pueden alojar información sensible que debe estar cifrada en reposo. Lo mismo aplica para los datos en Amazon S3; aunque AWS gestiona la seguridad física bajo el modelo de Responsabilidad Compartida, el cumplimiento normativo suele exigir que cifremos los datos a nivel de aplicación.

Además, es imperativo asegurar los datos en tránsito —aquellos que se mueven a través de las redes— y los datos en uso. Toda esta infraestructura de cifrado depende de una gestión de claves robusta, por lo que se vuelve esencial contar con una solución segura para la creación, rotación y administración de claves privadas.

## IAM

Aun con una seguridad de red y de datos de primer nivel, la gestión deficiente de identidades puede permitir que un actor malicioso tome el control total de las cuentas y recursos de AWS.

Si no se aplica estrictamente el principio de privilegio mínimo, el compromiso de una sola cuenta de usuario —incluso una de bajo nivel— puede otorgar a un atacante la capacidad de causar daños masivos. Este riesgo se extiende a los recursos: si una instancia EC2 posee permisos excesivos y es comprometida, facilita la escalada de privilegios y el movimiento lateral a través de la VPC y las subredes.

- Para proteger íntegramente a los usuarios y la infraestructura, es fundamental dominar el uso de:
- Políticas: Definición precisa de permisos mediante documentos JSON.
- Roles: Asignación de permisos temporales a usuarios y servicios.
- Mejores prácticas de credenciales: Uso de MFA y rotación de llaves de acceso.
- Gestión de usuarios finales: Control centralizado de identidades y accesos.

![Image](./assets/posts/2026-28-01-aws-security/13.png)

## Registro, monitoreo y auditoría: La visibilidad como defensa

Una vez implementados los controles de seguridad, es imperativo validar su efectividad y detectar posibles intentos de intrusión o brechas en nuestras defensas. La visibilidad total de la infraestructura se alcanza únicamente a través de una estrategia sólida de registro (logging), supervisión (monitoring) y auditoría.

![Image](./assets/posts/2026-28-01-aws-security/14.png)

Lograr esta visibilidad requiere dominar el registro de eventos desde diversas fuentes, adaptándose a cada servicio en uso. No basta con acumular registros; es fundamental procesarlos para transformarlos en datos accionables, monitorear métricas críticas y configurar alertas automáticas que nos notifiquen ante cualquier anomalía.

Para implementar una estrategia de monitoreo efectiva, debemos enfocarnos en:
- **Ingesta de datos**: Configurar VPC Flow Logs, CloudTrail y logs de aplicaciones.
- **Procesamiento**: Utilizar herramientas para filtrar y analizar patrones de tráfico o errores.
- **Alertas proactivas**: Establecer umbrales en CloudWatch que disparen acciones inmediatas ante incidentes.

## Continuidad del negocio

Finalmente, debemos abordar la continuidad del negocio. Este pilar se centra en la implementación de medidas proactivas para asegurar que las operaciones críticas permanezcan funcionales durante un ataque o cualquier otra interrupción técnica.

Ciertos vectores de amenaza, como los ataques de Denegación de Servicio Distribuido (DDoS), están diseñados específicamente para paralizar las operaciones y causar perjuicios económicos. Por ello, la arquitectura debe ser lo suficientemente resiliente para absorber estos impactos y mantener la disponibilidad del servicio.

![Image](./assets/posts/2026-28-01-aws-security/15.png)

Incluso fuera de ataques maliciosos, la estabilidad de la plataforma puede verse afectada por interrupciones técnicas ocasionales. La resiliencia de su entorno depende directamente de la arquitectura configurada.

Por ello, la continuidad del negocio es un pilar estratégico de seguridad. Para maximizar la disponibilidad y la capacidad de recuperación, debemos implementar:

- **Load Balancers**: Para distribuir el tráfico y evitar puntos únicos de fallo.
- **Auto Scaling**: Para ajustar la capacidad automáticamente ante picos de demanda o fallos de instancias.
- **Availability Zones (AZs)**: Para desplegar recursos de forma redundante en centros de datos físicamente separados.
- **Backups y Versioning**: Para garantizar la integridad de los datos y permitir la recuperación ante borrados accidentales o corrupción de archivos.

## Puntos de referencia y marcos de trabajo útiles

Existen diversos marcos de referencia diseñados para estandarizar la protección de entornos en la nube. Uno de los más destacados es el desarrollado por el Centro para la Seguridad de Internet (CIS). Estos documentos, accesibles de forma gratuita, ofrecen guías específicas como la de [AWS Three-Tier Web](https://d1.awsstatic.com/whitepapers/compliance/CIS_Amazon_Web_Services_Three-tier_Web_Architecture_Benchmark.pdf), que proporciona recomendaciones técnicas detalladas para cada nivel de la arquitectura.
> Tenable tambien tiene documentacion al respecto en base al [benchmark](https://www.tenable.com/audits/CIS_Amazon_Web_Services_Three-tier_Web_Architecture_L1_v1.0.0)

Como verás, las directrices del CIS se alinean directamente con los pilares que hemos analizado:

- **Data Protection**: Cifrado y gestión de integridad.
- **Identity and Access Management**: Control de identidades y accesos.
- **Business Continuity**: Alta disponibilidad y recuperación.
- **Event Monitoring and Response**: Detección de incidentes.
- **Audit and Logging**: Trazabilidad de acciones.
- **Networking**: Seguridad perimetral y segmentación.

Cada recomendación incluye una descripción técnica y el procedimiento exacto para auditar el entorno, permitiéndote verificar si tu infraestructura cumple con los estándares de seguridad internacionales.

