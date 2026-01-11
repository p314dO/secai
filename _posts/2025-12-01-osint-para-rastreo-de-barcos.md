---
title: OSINT para Rastreo de Barcos
image:
    path: /assets/posts/2025-11-26-osint-para-rastreo-de-barcos/image1.png
date: 2025-11-26 20:09:45 +/-TTTT
categories: [OSINT]
tags: [OSINT]
---

# Introducción

El rastreo de embarcaciones mediante OSINT (Open Source Intelligence) y el análisis de vulnerabilidades en sistemas marítimos son habilidades críticas en ciberseguridad moderna. La industria marítima ha experimentado un aumento del 900% en ciberataques desde 2017, y las técnicas OSINT revelan exactamente por qué: sistemas obsoletos, credenciales por defecto, y dispositivos expuestos a internet sin protección alguna.

El OSINT marítimo es aplicable a múltiples disciplinas:

- Pentesting y hacking ético
- Investigaciones privadas (rastreo de fugitivos, activos)
- Análisis económico y comercial
- Periodismo de investigación
- Análisis de amenazas geopolíticas
- Auditorías de seguridad corporativa

## DISCLAIMER

> TODO EL CONTENIDO DE ESTE POST ES CON FINES EDUCATIVOS Y DE CONCIENCIACIÓN SOBRE SEGURIDAD. EL ACCESO NO AUTORIZADO A SISTEMAS INFORMÁTICOS ES ILEGAL. LAS TÉCNICAS DESCRITAS DEBEN USARSE EXCLUSIVAMENTE EN SISTEMAS PROPIOS O CON AUTORIZACIÓN EXPLÍCITA POR ESCRITO.

## El Contexto: Una Industria Bajo Ataque

Casos Reales de Ciberataques Marítimos 

**[Maersk - NotPetya (2017)](https://www.lrqa.com/en/insights/articles/notpetya-ransomware-attack-on-maersk-key-learnings/)**
- Pérdidas: 300 millones de dólares
- Impacto: 76 terminales portuarias paralizadas mundialmente
- Efecto: Paralización total de buques portacontenedores

Otros incidentes:

- COSCO (2018): [Naviera china comprometida](https://www.cshub.com/attacks/news/incident-of-the-week-cosco-shipping-faces-ransomware-attack)
- Austal (2018): [Constructor australiano de ferries atacado](https://www.abc.net.au/news/2018-11-02/austal-ship-cyber-attack-and-extortion-attempt-national-security/10458982)
- Corea del Sur (2016): [280 buques forzados a regresar a puerto por interferencia GPS](https://www.cnbc.com/2017/02/01/shipping-industry-vulnerable-to-cyber-attacks-and-gps-jamming.html)
- China (2019): [Cientos de buques víctimas de GPS spoofing](https://safety4sea.com/vessels-navigating-in-china-report-gps-spoofing-incidents/)

La realidad: [Solo el 15% de los marineros recibe formación en ciberseguridad](https://cyberonboard.com/wp-content/uploads/Crew_Connectivity_2018_Survey_Report.pdf), y casi la mitad ha navegado en barcos afectados por ciberataques.

---

## Marine Traffic: La Herramienta Fundamental

¿Cómo funciona [Marine Traffic](https://www.marinetraffic.com/)?

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image2.png)

Marine Traffic es la plataforma más completa para rastreo de embarcaciones en tiempo real. Para entender su poder, primero necesitas conocer los tres sistemas de rastreo que utiliza simultáneamente.

### AIS - Automatic Identification System

El primero es el [AIS (Sistema Automático de Identificación)](https://en.wikipedia.org/wiki/Automatic_identification_system), que transmite la posición del barco en tiempo real. Aquí está el problema de seguridad que mencionamos antes: este sistema no está cifrado, lo que significa que cualquiera puede interceptar y manipular estas señales. [Desde 2014 se conoce que es vulnerable, y aún así sigue siendo el estándar de la industria.](https://www.muyseguridad.net/2014/03/26/trend-micro-vulnerabilidades-barcos/)

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image3.png)

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image4.webp)

### INMARSAT

El segundo sistema es el [rastreo satelital mediante INMARSAT](https://en.wikipedia.org/wiki/Inmarsat#:~:text=Inmarsat's%20network%20provides%20communications%20services,was%20completed%20in%20May%202023.), que permite seguir barcos a cualquier distancia en el mundo. Esto es especialmente útil cuando los barcos están en aguas internacionales, lejos del alcance del AIS terrestre.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image5.png)

### Redes Moviles

Finalmente, para aguas interiores y zonas costeras, Marine Traffic utiliza redes móviles. Las torres celulares cerca de puertos y ríos capturan señales de embarcaciones cercanas, complementando la cobertura del AIS.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image6.png)

---

## Casos de Uso Prácticos

### CASO 1: Análisis Económico y Comercial

Imagina que trabajas como analista económico y necesitas evaluar la salud de la economía de una región costera. Los datos de tráfico marítimo son uno de los indicadores más confiables que existen. Una economía próspera mueve mercancías, y esas mercancías viajan en barcos.

Las preguntas que puedes responder con Marine Traffic son fascinantes: 
- ¿Está una región en crecimiento o declive? 
- ¿Hay cuellos de botella logísticos que están afectando la cadena de suministro? 
- ¿Cuánto petróleo está importando un país (un excelente indicador de consumo energético)? 
- ¿De dónde vienen las mercancías y hacia dónde van?

**La metodología es simple pero poderosa**. Primero, abre Marine Traffic y navega hasta la región que quieres analizar. En el menú lateral derecho, encontrarás "Vessel Filters". Aquí es donde empieza la magia. Si quieres medir actividad comercial, selecciona únicamente "Cargo Vessels" (que aparecen en verde en el mapa) y "Tankers" (que aparecen en rojo).

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image7.png)


![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image8.png)

Tomemos un ejemplo real que demuestra perfectamente este concepto. En la imagen de abajo puedes ver el petrolero **"BLACKCOMB SPIRIT"** navegando cerca del [puerto de Arzew (Argelia)](https://www.marinetraffic.com/en/ais/details/ports/1332?name=ARZEW&country=Algeria).

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image9.png)

Lo más revelador está en el estado de navegación: "Underway Using Engine" con datos actualizados hace apenas 22 minutos. El barco tiene un calado de 8.4m y velocidad de 2.4kn/238°. Esa velocidad extremadamente baja y el hecho de que lleva desde las 23 de ayer dando vueltas cerca del puerto te dice exactamente lo que necesitas saber: está esperando su turno para atracar.

Si haces clic en "Past track" (el historial de ruta), verás exactamente por dónde ha navegado en las últimas 24 horas. Con una suscripción paga puedes ver períodos más largos. Esta función te permite rastrear rutas comerciales completas, identificar patrones, y hasta predecir comportamientos futuros.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image10.png)

Este patrón de comportamiento (velocidad reducida, navegación en círculos, cercanía al puerto pero sin atracar) es un indicador clásico de congestión portuaria o espera por ventana de atraque. Para un analista económico, ver múltiples barcos en este estado simultáneamente señalaría un cuello de botella logístico. Para un investigador, confirma que el barco está exactamente donde dice estar, sin discrepancias entre su posición AIS y su ubicación real.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image11.png)

Si haces click en "Vessel Details" inmediatamente obtienes una cantidad sorprendente de información: De donde partio y la fecha,a donde va y la fecha de arrivo(estimada).
Puedes ver su velocidad actual, su rumbo, y hasta el tipo de carga que transporta.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image12.png)

Para un analista económico, esto es oro puro. Puedes contar cuántos barcos llegan diariamente, de qué países vienen, qué están transportando, y cuánto tiempo tardan en descargar. Todo esto sin levantarte de tu escritorio.

---

### CASO 2: Investigaciones Privadas

Cambiemos de escenario. Ahora eres un investigador privado rastreando a un fugitivo que sabes que tiene una embarcación en una zona. No sabes exactamente dónde está, pero tienes algunas pistas.

Si conoces el nombre exacto del barco, el trabajo es trivial. Ve directamente a la barra de búsqueda "Search MarineTraffic" en la parte superior, ingresa el nombre, y Marine Traffic te mostrará su ubicación exacta en tiempo real. Así de simple.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image13.png)

Pero supongamos que la situación es más compleja: solo sospechas que tu objetivo está en la zona del caribe , supongamos cerca de las islas Antigua y Barbuda, y no conoces el nombre de su embarcación. Solo sabes que es una embarcación recreativa o de pasajeros pequeña.

Aquí es donde los filtros se vuelven tus mejores aliados. Regresa al menú de "Vessel Filters" y esta vez selecciona solo "Passenger Vessels" y "Pleasure Craft". Cuando apliques el filtro, la transformación del mapa es dramática. Todos esos enormes cargueros y petroleros desaparecen, y solo quedan las embarcaciones pequeñas.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image14.png)

Ahora empieza la investigación real. Pasa el cursor lentamente sobre cada embarcación. Marine Traffic te mostrará su nombre, y si hay foto disponible, también la imagen del barco. Esto es crucial porque puedes verificar visualmente si es la embarcación que buscas.

Encontremos un ejemplo concreto (ACLARACION: NO ES UN YATE FUGITIVO): el "PERSEPHONE" posicionado cerca de la Isla Antigua. Al pasar el cursor, ves el nombre y una foto del yate. Los datos de posición son inquietantemente recientes: actualizados hace apenas 2 minutos. Estás viendo su ubicación prácticamente en tiempo real.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image15.png)

Haz clic en "Vessel Details" justo debajo de la imagen. Aquí la información se expande dramáticamente. Obtienes la posición GPS exacta (en este caso si tienes una cuenta que lo permita), la velocidad actual en nudos, el rumbo, la fuente de los datos (AIS/Satélite/Terrestre)..

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image16.png)

Si necesitas un análisis más profundo, usa la función de "Past Track". La versión gratuita te permite ver las últimas 24 horas de movimiento trazadas en el mapa como una línea continua. Con esto puedes identificar patrones: ¿navegó directamente o hizo paradas? ¿A qué velocidad se movió? ¿Cambió de rumbo repentinamente?

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image17.png)

Para investigadores privados, esta herramienta es invaluable. Lo que antes requería vigilancia física, contactos en puertos, y recursos considerables, ahora lo obtienes gratis desde tu computadora.

---

### CASO 3: Auditoría de Seguridad (Pentesting)

Ahora ponte en los zapatos de un pentester contratado para auditar la seguridad de una naviera. Tu objetivo es evaluar la superficie de ataque de toda su flota. Marine Traffic es tu punto de partida.

**Fase 1: Identificación de activos**  

Busca todos los barcos que pertenecen a la empresa. Puedes buscar por nombre de la naviera o, si conoces algunos barcos, usar esa información para encontrar otros de la misma flota. Para cada barco, anota su número IMO (el identificador único que nunca cambia) y su ubicación actual. Esto te da un inventario completo de activos flotantes.

**Fase 2: Correlación con SHODAN**  

Aquí es donde la auditoría se pone realmente interesante. Supongamos que durante la Fase 1 identificaste que la flota de la empresa que estás auditando utiliza sistemas de comunicación satelital SAILOR 900 VSAT para sus operaciones. Este es un dato crucial porque ahora sabes exactamente qué buscar en SHODAN.

Toma el query básico: title:"sailor 900" y ejecútalo en SHODAN. Lo primero que verás es cuántos sistemas están expuestos globalmente. En este caso, SHODAN encuentra 4 resultados totales de sistemas VSAT Sailor 900 accesibles públicamente desde internet. Fíjate en el mapa de países: estos sistemas están distribuidos en Irán, Luxemburgo, Malasia y Estados Unidos.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image19.png)

Ahora viene lo inquietante. Cuando haces clic en uno de estos resultados, obtienes acceso a información técnica detallada. Mira este ejemplo de un sistema VSAT:

- La IP exacta del dispositivo (tapada)
- Los puertos abiertos: 8000 y 8001
- El hostname (tapado)
- Tecnologías detectadas: jQuery 1.12.4, lighttpd 1.4.48
- Y lo más crítico: una sección completa de vulnerabilidades conocidas

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image20.png)

Desplázate hacia abajo y encontrarás algo aún más preocupante: el panel muestra vulnerabilidades específicas organizadas por año. Este sistema en particular tiene CVEs desde 2019 hasta 2022. Cada una con su nivel de severidad indicado por color (rojo para crítico, naranja para alto, amarillo para medio).

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image21.png)

Pero espera, hay más. Si encuentras la IP correcta y la visitas directamente en tu navegador (algo que NUNCA debes hacer sin autorización explícita del cliente), podrías encontrarte con el panel de administración completo del sistema VSAT. Mira esta captura: un dashboard de Cobham SAILOR 900 VSAT completamente accesible.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image22.png)

>Imagen tomada de [hackernoon.com](https://hackernoon.com/using-osint-for-maritime-intelligence-3lm3uht)

- Las coordenadas GNSS exactas
- Los números de serie (ACU serial number, Antenna serial number)
- El nombre del perfil del sistema ("Sandro - SAILOR 900 VSAT Ku" en la esquina superior derecha tapada)
- Cualquier IP que aparezca en la URL del navegador (no incluida en la imagen)

Este blog tiene un excelente post al respecto: [Satellite Communications Equipement Security](https://www.pentestpartners.com/security-blog/satellite-communications-equipment-security/).

Toda esta información está disponible sin autenticación, simplemente visitando la IP. Para un atacante, esto es suficiente para planear un ataque dirigido. Para un auditor, esto es un hallazgo crítico que debe reportarse inmediatamente.

Ahora observa lo irónico de la situación. El fabricante describe el sistema SAILOR 900 VSAT Ka como "fácil de implementar" con "beneficios significativos de instalación" y "confiabilidad incomparable". El problema es que esa facilidad de implementación a menudo viene a costa de la seguridad. Muchos de estos sistemas se despliegan con configuraciones por defecto y nunca se endurecen adecuadamente.

![image](/assets/posts/2025-12-01-osint-para-rastreo-de-barcos/image18.png)

Tu documentación como auditor debe incluir:

- Lista de IPs encontradas (censuradas en el reporte público)
- Puertos abiertos por sistema
- CVEs específicos y su criticidad
- Capturas de pantalla de paneles accesibles (con datos sensibles censurados)
- Modelo exacto del hardware vulnerable
- Recomendaciones de remediación inmediata

Volviendo a nuestro ejemplo: si la flota que auditas usa estos mismos sistemas SAILOR 900 VSAT, ahora tienes evidencia concreta de que dispositivos idénticos están expuestos globalmente con múltiples vulnerabilidades críticas. Esto fortalece tu caso para exigir cambios inmediatos en la configuración de seguridad.

---

## Conclusión: Un Océano de Vulnerabilidades por Explorar

Hemos navegado por las aguas del OSINT marítimo y la ciberseguridad naval, desde el rastreo básico de embarcaciones hasta la identificación de sistemas críticos expuestos. Lo que comenzó como una simple búsqueda en Marine Traffic terminó revelando una realidad preocupante: la industria marítima, responsable de mover el 90% del comercio mundial, está peligrosamente expuesta.

Vimos cómo con herramientas gratuitas y técnicas accesibles podemos mapear flotas completas, identificar sistemas VSAT vulnerables, rastrear tripulaciones, y hasta acceder a paneles de administración sin autenticación. Esto no es ciencia ficción ni hacking avanzado: es la realidad actual de miles de embarcaciones navegando en este momento.

No soy un experto absoluto en seguridad marítima ni en todos los sistemas OT que operan en barcos. Este tema es vastísimo. Cada sistema que mencionamos (AIS, ECDIS, GMDSS, VSAT, sensores industriales, redes de tripulación) merece su propia serie de artículos dedicados. La intersección entre OSINT, ciberseguridad, y operaciones marítimas es un campo en constante evolución, con nuevas amenazas emergiendo constantemente.

Este post es apenas la superficie. Como un submarinista que acaba de sumergirse, hemos visto lo suficiente para entender la magnitud del océano que tenemos debajo, pero queda muchísimo por explorar.

### ¿Por qué esto importa tanto?

**Desde la perspectiva geopolítica**, el control de rutas marítimas puede definir conflictos internacionales. La capacidad de rastrear, interferir o comprometer flotas militares y comerciales es una herramienta de poder estratégico. Los incidentes de GPS spoofing en Corea del Sur y China no fueron accidentes: fueron demostraciones de capacidad.

**Desde la perspectiva económica**, un solo ciberataque como el de Maersk puede costar 300 millones de dólares y paralizar el comercio global durante días. Multiplicado por las vulnerabilidades que vimos, el riesgo sistémico es enorme.

**Desde la perspectiva de seguridad**, cada sistema VSAT expuesto, cada panel sin autenticación, cada credencial por defecto sin cambiar, es una puerta abierta. No solo para atacantes sofisticados: para cualquiera con conocimientos básicos y una conexión a internet.

**Desde la perspectiva humanitaria**, estamos hablando de sistemas que mantienen vivas a las tripulaciones. Comprometer las comunicaciones de socorro, la navegación en zonas peligrosas, o los sistemas de estabilidad del barco puede tener consecuencias fatales.

### El mensaje para los defensores

Si trabajas en la industria marítima, en seguridad corporativa de navieras, o eres responsable de IT/OT en embarcaciones: las técnicas que describí aquí son las mismas que usan los atacantes para reconocimiento y hay muchisimas mas, que ni siquiera conozco. La diferencia es que ellos las usan en silencio, sin reportar lo que encuentran.
Usa estas herramientas proactivamente:
- Busca tu propia flota en SHODAN mensualmente
- Verifica que tus barcos no tengan paneles expuestos
- Cambia TODAS las credenciales por defecto
- Implementa segmentación de redes
- Capacita a tus tripulaciones (nada de [compartir paneles en TikTok](https://www.tiktok.com/@merchant_seaman/video/7065323186308517122) o Instagram)

La defensa marítima empieza con visibilidad. Si no sabes qué está expuesto, no puedes protegerlo.

### El mensaje para los investigadores 

Esta es un área fértil para investigación en ciberseguridad. Hay casos de uso fascinantes en OSINT marítimo: desde rastreo de tráfico ilegal hasta análisis de cadenas de suministro, desde investigaciones de sanciones hasta periodismo de datos.
Solo recuerda: con gran conocimiento viene gran responsabilidad. La línea entre investigación legítima y acceso no autorizado es clara. NO LA CRUCES.

---

## Recursos utilizados

**Guías y Tutoriales OSINT Marítimo**
- [Hackers Arise - OSINT: Tracking Marine Traffic Around the World](https://hackers-arise.com/open-source-intelligence-osint-tracking-marine-traffic-around-the-world/)
- [Using OSINT for Maritime Intelligence](https://hackernoon.com/using-osint-for-maritime-intelligence-3lm3uht)
- [Rae Baker - 5 Methods for Tracking Planes and Ships That Aren't Twitter](https://wondersmithrae.medium.com/5-methods-for-tracking-planes-and-ships-that-arent-twitter-rae-baker-deep-dive-1e6702eb5afa)
- [Rae Baker: Deep Dive Blog](https://www.raebaker.net/blog)
- [Deep Dive OSINT (Hacking, Shodan and more!) - YouTube](https://www.youtube.com/watch?v=dxiNByvkvU8)

**Herramientas de Rastreo Marítimo**
- [MarineTraffic](https://www.marinetraffic.com)
- [Live Map Filters for Advanced Live Map](https://support.marinetraffic.com/en/articles/9552715-live-map-filters-for-advanced-live-map)
- [MyShipTracking](https://www.myshiptracking.com/)
- [ShipXplorer](https://www.shipxplorer.com/@40.09537,18.56895,z5?rightSidebar=filters)
- [SHIPSPOTTING](https://www.shipspotting.com/)

**Ciberseguridad Marítima y Vulnerabilidades**

**Análisis de Seguridad en Sistemas Marítimos:**
- [Satellite Communications Equipment Security - Pen Test Partners](https://www.pentestpartners.com/security-blog/satellite-communications-equipment-security/)
- [Naval Dome: Cyberattacks on OT Systems on the Rise](https://maritime-executive.com/article/naval-dome-cyberattacks-on-ot-systems-on-the-rise?ref=hackernoon.com)
- [ENISA - European Union Agency for Cybersecurity](https://www.enisa.europa.eu/)

**GPS Spoofing y Vulnerabilidades AIS:**
- [Understanding GPS Spoofing in Shipping: How to Stay Protected](https://safety4sea.com/cm-understanding-gps-spoofing-in-shipping-how-to-stay-protected/?ref=hackernoon.com)
- [Vessels Navigating in China Report GPS Spoofing Incidents](https://safety4sea.com/vessels-navigating-in-china-report-gps-spoofing-incidents/)
- [UK MAIB Investigation: Ships' Collision Linked to Inappropriate Use of VHF and AIS Data](https://safety4sea.com/uk-maib-investigation-ships-collision-linked-to-inappropriate-use-of-vhf-and-ais-data/?ref=hackernoon.com)
- [CNBC: Shipping Industry Vulnerable to Cyber Attacks and GPS Jamming](https://www.cnbc.com/2017/02/01/shipping-industry-vulnerable-to-cyber-attacks-and-gps-jamming.html)
- [Could the Shipping Industry Be Susceptible to Cyber-Attacks?](https://fpa.org/shipping-industry-susceptible-cyber-attacks/#:~:text=South%20Korea%20reported%20that%20280,can%20evaluate%20what%20you%20identify.)

**Casos Reales de Ciberataques Marítimos**

**Maersk - NotPetya (2017):**
- [NotPetya Ransomware Attack on Maersk - Key Learnings](https://www.lrqa.com/en/insights/articles/notpetya-ransomware-attack-on-maersk-key-learnings/)
- [Case Study: Maersk NotPetya Ransomware Attack - YouTube](https://www.youtube.com/watch?v=CLYjs37LT74)

**COSCO (2018):**
- [Incident Of The Week: COSCO Shipping Faces Ransomware Attack](https://www.cshub.com/attacks/news/incident-of-the-week-cosco-shipping-faces-ransomware-attack)
- [COSCO Shipping Lines Falls Victim to Cyber Attack](https://psabdp.com/news/cosco-hit-by-cyber-attack)

**Austal (2018):**
- [Shipbuilder Austal in Australia Hit by Hacking/Ransomware Attack](https://maritimecybersecurity.nl/incident/4DG6J55oZA)
- [Explainer: Here's What You Need to Know About the Austal Cyber Attack](https://www.abc.net.au/news/2018-11-02/austal-ship-cyber-attack-and-extortion-attempt-national-security/10458982)

**Sistemas y Tecnología Marítima**

**AIS (Automatic Identification System):**
- [AIS Overview - Shine Micro](https://www.shinemicro.com/ais-overview/)
- [Sistemas AIS: ¿Qué Tipo es el Adecuado para mi Embarcación?](https://www.raymarine.com/es-es/aprendizaje/guias-en-linea/sistemas-ais-que-tipo-es-el-adecuado-para-mi-embarcacion)
- [Explicación de los Sistemas de Identificación Automática (SIA)](https://www.rubicon3adventure.com/es/sistema-de-identificacion-automatica-es-explicado/#:~:text=En%20resumen%2C%20aunque%20el%20alcance,estaciones%20base%2C%20repetidores%20y%20sat%C3%A9lites.)
- [Normativa sobre el Sistema AIS](https://tituladosnauticopesqueros.wordpress.com/2017/02/27/normativa-sobre-el-sistema-ais-automatic-identification-system/)

**Otros Sistemas:**
- [Global Maritime Distress and Safety System (GMDSS)](https://gmdsstesters.com/radio-survey/general/global-maritime-distress-and-safety-system.html)
- [Inmarsat - Wikipedia](https://en.wikipedia.org/wiki/Inmarsat#:~:text=Inmarsat's%20network%20provides%20communications%20services,was%20completed%20in%20May%202023.)

**Estado de la Ciberseguridad en Tripulaciones (viejazo)**
- [Crew Connectivity 2018 Survey Report - PDF](https://cyberonboard.com/wp-content/uploads/Crew_Connectivity_2018_Survey_Report.pdf)




