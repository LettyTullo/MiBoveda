[[Protocolo de Red]]
## Modelo OSI
- Tiene origen teorico. Una propuesta elaborada por ISO(Organizacion Internacional de Standars) en 1983. Fue revisado en 1995 y se denomina OSI (Interconexion de Sistemas Abiertos)
## Capas
# 1- Capa Fisica: 
Es la capa más baja y se encarga de la **transmisión de bits puros** a través de un canal de comunicación. Define las especificaciones mecánicas y eléctricas, como los niveles de voltaje, la duración de las señales, los tipos de conectores, cables y las bandas inalámbricas. Su unidad de información es el **bit**. 
# 2- Capa de Enlace de Datos:
Su función principal es transformar un medio de transmisión crudo en una **línea libre de errores** para la capa de red. Sus tareas incluyen:
- **Enmarcado:** Fragmentar el flujo de bits en unidades llamadas **tramas**.
- **Control de errores:** Detectar y corregir tramas dañadas o perdidas.
- **Control de flujo:** Evitar que un emisor rápido sature a un receptor lento.
- **Direccionamiento físico:** Identificar el origen y destino de las tramas en el enlace local.
# 3- Capa de Red:
Se ocupa de controlar las operaciones de la subred para que los **paquetes** lleguen desde el origen hasta el destino final, lo que implica realizar múltiples saltos en routers intermedios. Es la capa responsable del **enrutamiento** y del **direccionamiento lógico** (internetworking)
# 4- Capa de Transporte:
Su función es aceptar los datos de las capas superiores, dividirlos en unidades más pequeñas llamadas **segmentos** y asegurarse de que lleguen correctamente al destino. Actúa como una frontera que **vuelve transparente a los hosts** los detalles de la tecnología de red subyacente, proporcionando una comunicación de extremo a extremo fiable.

# 5- Capa de Sesion:
Permite a los usuarios de diferentes máquinas establecer y manejar **sesiones** entre ellos. Sus servicios incluyen el **control del diálogo** (quién transmite y cuándo), el agrupamiento de datos y la sincronización mediante puntos de recuperación ante fallas.
# 6- Capa de presentacion:
A diferencia de las capas inferiores que mueven bits, esta se ocupa de la **sintaxis y semántica** de la información transmitida. Realiza tareas de **codificación de caracteres** (como ASCII), **cifrado y descifrado** de datos para la seguridad, y algoritmos de **compresión** (como ZIP)
# 7- Capa de Aplicacion:
Es la capa que interactúa directamente con el **usuario o el programa de aplicación**. Contiene una variedad de protocolos de uso común como **HTTP** para la navegación web, **SMTP** para el correo electrónico y **DNS** para la resolución de nombres.

![[Pasted image 20260308132605.png]]

## Modelo TCP/IP
Tiene un origen practico (basado en protocolos reales como los de ARPANET) y su objetivo principal es permitir la interconexion de redes de manera fluida (1974 y 1988)
## Capas
# 1- Capa de enlace:
Es la capa más baja y describe qué deben realizar los enlaces físicos para satisfacer las necesidades de la capa de Internet sin conexión.
- **Naturaleza:** Más que una capa en el sentido estricto, funciona como una **interfaz entre el host y los enlaces de transmisión**.
- **Ejemplos:** Define protocolos para tecnologías como **Ethernet**, **WiFi (802.11)** o líneas serie
# 2- Capa de Internet:
Es el eje que mantiene unida toda la arquitectura
- **Objetivo:** Permitir que los hosts inyecten paquetes en cualquier red para que viajen de forma independiente hacia su destino (potencialmente a través de diferentes subredes).
- **Protocolo base:** **IP (Protocolo de Internet)**, que define el formato oficial de los paquetes y se encarga del enrutamiento. También incluye el protocolo **ICMP** para mensajes de control y error
# 3- Capa de transporte:
Su función es permitir que las entidades pares en los hosts de origen y destino mantengan una conversación.
- **Protocolos principales:**
    - **TCP (Protocolo de Control de Transmisión):** Es un protocolo **fiable y orientado a la conexión** que garantiza que el flujo de bytes llegue sin errores y en orden.
    - **UDP (Protocolo de Datagramas de Usuario):** Es un protocolo **no fiable y sin conexión** diseñado para aplicaciones que prefieren una entrega rápida sobre la precisión (como streaming de video o voz)
# 4- Capa de Aplicacion:
Es la capa superior y contiene todos los protocolos de alto nivel que las aplicaciones utilizan para comunicarse a través de la red.
- **Funciones:** A diferencia del modelo OSI, el TCP/IP no tiene capas separadas de Sesión o Presentación; si una aplicación necesita esas funciones, las incluye internamente.
- **Protocolos clave:** **HTTP** (web), **SMTP** (correo electrónico), **FTP** (transferencia de archivos), **DNS** (resolución de nombres) y **RTP** (multimedia en tiempo real)

![[Pasted image 20260308141751.png]]

## Criticas al modelo de protocolo OSI
# - Mala sincronización (El modelo de la "doble burbuja")
El éxito de un estándar depende de su aparición en el momento justo entre la fase de investigación y la de inversión masiva.
- **TCP/IP ya estaba establecido:** Cuando aparecieron los protocolos OSI, las universidades ya utilizaban ampliamente TCP/IP y muchos proveedores ya ofrecían productos para este modelo.
- **Retraso en el soporte:** Debido a que las empresas no quisieron soportar una segunda pila de protocolos hasta que fuera estrictamente necesario, no hubo ofertas iniciales de protocolos OSI, permitiendo que TCP/IP se consolidara
# - Mal diseño
Tanto el modelo como sus protocolos presentan defectos estructurales y conceptuales:
- **Capas vacías vs. sobrecargadas:** Las capas de **Sesión y Presentación** están casi vacías, mientras que las de **Enlace de datos y Red** están saturadas de funciones.
- **Funciones repetitivas:** Tareas como el direccionamiento, el control de flujo y el control de errores aparecen de forma redundante en casi todas las capas. Se argumenta que, para ser eficaz, el control de errores debería realizarse solo en la capa más alta.
- **Complejidad excesiva:** El modelo es extraordinariamente complejo, difícil de aplicar y de funcionamiento ineficaz. Se bromeaba con que un estándar internacional era como un mafioso que "te hace una oferta que no puedes entender".
# -Malas aplicaciones 
- **Lentitud y tamaño:** Los primeros productos eran enormes, lentos y difíciles de manejar, lo que generó una asociación inmediata de la marca "OSI" con la **baja calidad**.
- **Contraste con TCP/IP:** Mientras OSI fallaba en sus inicios, las implementaciones de TCP/IP (especialmente la de Berkeley UNIX) eran buenas y gratuitas, lo que generó una comunidad de usuarios que impulsó mejoras constantes
# - Mala politica
- **Origen burocrático:** Se percibía a OSI como una creación de burócratas gubernamentales y ministerios de telecomunicaciones europeos que intentaban imponer un estándar técnicamente inferior.
- **Aceptación de TCP/IP:** En cambio, TCP/IP se veía como parte de UNIX, un sistema con gran aceptación y estima en el mundo académico de los años 80

# Criticas del modelo TCP-IP
- **Falta de distinción entre conceptos clave:** A diferencia de OSI, el modelo TCP/IP **no distingue claramente entre servicios, interfaces y protocolos**
- **Modelo derivado de los protocolos:** Se considera un **modelo pobre** debido a que se definió después de que los protocolos ya existieran
- **Deficiencias en las capas inferiores:** El modelo **no distingue entre la capa física y la capa de enlace de datos**, las cuales realizan tareas completamente diferentes (transmisión de bits frente a delimitación de tramas y fiabilidad). Un modelo adecuado debería tratarlas como capas separadas.
- **Falta de generalidad:** El modelo no es adecuado para describir cualquier pila de protocolos que no sea TCP/IP. Por ejemplo, intentar describir tecnologías como **Bluetooth** utilizando el modelo TCP/IP resulta imposible
# Modelo utilizado en el libro
5- Aplicacion 
4- Transporte
3- Red
2- Enlace
1- Fisico
## Estandarizacion
# El objetivo: La interoperabilidad
Las normas no dictan todos los detalles de cómo se construye un producto, sino que definen estrictamente lo necesario para la **interoperabilidad** (la capacidad de que sistemas distintos trabajen juntos).
- Esto permite la **producción en masa** y economías de escala, lo que reduce los precios y aumenta la aceptación de la tecnología en el mercado.
- Las empresas pueden competir en la calidad de su implementación mientras respetan el estándar común (por ejemplo, el estándar 802.11 define velocidades, pero no cuándo usarlas exactamente
# Categorías de estándares
Existen dos formas principales en las que surgen los estándares:
- **De Facto (de hecho):** Son normas que surgen sin un plan formal pero que se vuelven universales por su éxito. Ejemplos son el protocolo **HTTP** y la tecnología **Bluetooth**.
- **De Jure (por ley):** Son normas adoptadas mediante las reglas de organismos oficiales de normalización
# - **Telecomunicaciones:**
- **ITU (Unión Internacional de Telecomunicaciones):** Agencia de la ONU que emite "recomendaciones" técnicas. Su sector **ITU-T** se ocupa de la telefonía y los sistemas de datos (como la norma H.264 para video).
- **Normas Internacionales:**
    - **ISO (Organización Internacional de Normalización):** Organización voluntaria que publica estándares sobre miles de temas, incluyendo el modelo OSI y las tecnologías de la información a través del comité **JTC1**.
- **Ingeniería y LAN:**
    - **IEEE (Instituto de Ingenieros Eléctricos y Electrónicos):** La organización profesional más grande del mundo. Su comité **802** ha estandarizado redes fundamentales como **Ethernet (802.3)** y **WiFi (802.11)**
- NIST (National Institute of Standards and Technology) Parte del Departamento de Comercio de USA
# **Internet:**
- I**AB (Internet Arquitecture Board)** supervisaba a la ARPANET. Emite RFC (Request for Comments) Subsidiarias a la IAB son:
	- **IRTF (Internet Research Task Force)** - investigación a largo plazo
	- **IETF (Internet Engineering Task Force):** Se encarga de la ingeniería a corto plazo y publica los documentos.
- **W3C (World Wide Web Consortium):** Desarrolla protocolos y guías (como HTML y CSS) para facilitar el crecimiento de la Web
- 802.1 - Visión general y arquitectura de las redes LAN
- 802.15 - Redes de área personal (Bluetooth, Zigbee)

![[Pasted image 20260308172107.png]]