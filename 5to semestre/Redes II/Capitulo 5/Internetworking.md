Es la práctica de conectar dos o más redes físicas distintas e independientes para formar una sola red unificada y más grande, comúnmente llamada _internetwork_ o simplemente _internet_ (con "i" minúscula).

La necesidad de unir redes es fundamentalmente económica. Según la **Ley de Metcalfe**, el valor de una red es proporcional al cuadrado de su número de nodos (\(N^2\)). Esto significa que combinar varias redes pequeñas para formar una inter-red incrementa exponencialmente su valor y su utilidad práctica al permitir la comunicación universal entre ellas.
# Overview de Internetworking (Interconexión de redes)
Esta sección ofrece una perspectiva de alto nivel sobre cómo conectar múltiples redes físicas e independientes para formar una sola inter-red unificada:

- Introduce el desafío de la **heterogeneidad**, explicando cómo las redes difieren en aspectos críticos como el direccionamiento, los tamaños máximos de paquete (MTU), la seguridad y los niveles de fiabilidad.
- Explica de manera general cómo una **capa común de indirección** (el protocolo IP) actúa como un formato universal que permite que los paquetes atraviesen de manera fluida redes tecnológicamente distintas (como 802.11, MPLS y Ethernet)
# ¿Cómo difieren las redes?

Lograr que múltiples redes trabajen juntas es extremadamente complejo porque la **heterogeneidad** es una característica permanente en la tecnología. Las redes individuales difieren drásticamente en la capa de red en aspectos como:

- **Servicios ofrecidos:** Algunas redes son sin conexión (como IP orientada a datagramas) y otras son orientadas a la conexión (como los Circuitos Virtuales, ATM o MPLS).
- **Direccionamiento:** Utilizan diferentes tamaños de dirección, formatos planos o jerarquías.
- **Capacidad de difusión:** Algunas redes soportan difusión (_broadcast_) o multidifusión (_multicast_) y otras no.
- **Tamaño del paquete (MTU):** Cada red tiene un tamaño máximo de paquete permitido para la transmisión.
- **Seguridad, Fiabilidad y QoS:** Poseen diferentes niveles de pérdida de paquetes, métodos de cifrado o mecanismos de calidad de servicio.
## Opciones de diseño para conectar redes heterogéneas
Para solucionar estas diferencias y permitir que los paquetes viajen a través de redes extranjeras, existen dos filosofías de diseño básicas:
# Opción A: Traducción mediante Pasarelas (_Gateways_)

Consiste en colocar dispositivos en los límites de las redes que traduzcan y conviertan directamente los paquetes de un formato de red al de la otra red.

- **Inconvenientes:** La conversión directa suele ser incompleta y estar condenada al fracaso si los protocolos son muy diferentes. Por ejemplo, las direcciones IPv6 de 128 bits no caben físicamente en un campo de dirección IPv4 de 32 bits, por mucho que se intente. De igual forma, traducir entre una red sin conexión y una orientada a la conexión es sumamente complejo y rara vez se intenta con éxito debido a estas dificultades.
# Opción B: Añadir una Capa Común (La Solución IP)
Propuesta por Vint Cerf y Bob Kahn en 1974, esta opción añade una **capa común de indirección** por encima de todas las redes heterogéneas. El protocolo **IP (Internet Protocol)** sirve como este formato universal de paquetes que todos los enrutadores reconocen.
- En este modelo, existe una diferencia crítica entre la **conmutación** y el **enrutamiento**:
    - **Switches y Puentes (Capa 2):** Conectan el mismo tipo de red transportando la trama completa basándose únicamente en la dirección física MAC.
    - **Routers (Capa 3):** Conectan redes diferentes; extraen el paquete IP de la trama física en cada límite, examinan la dirección IP común del paquete y eligen de forma dinámica la mejor ruta hacia el destino.
## Técnicas clave en Internetworking

Para que el tráfico fluya eficazmente entre las diferentes redes de una inter-red se requieren tres mecanismos esenciales:
# A. Fragmentación de paquetes
	Dado que cada red intermedia puede tener un tamaño máximo de paquete (MTU - **Unidad de Transmisión Máxima**) diferente (por ejemplo, 2272 bytes en 802.11 frente a 1500 bytes en Ethernet), un router puede verse obligado a dividir un paquete grande en partes llamadas **fragmentos**. Cada fragmento se envía de forma independiente como un paquete de red separado y se vuelve a ensamblar en el destino final para evitar sobrecargar a los routers intermedios.
# B. Tunelización (_Tunneling_)
Se aplica cuando el origen y el destino de la transmisión se encuentran en el mismo tipo de red (por ejemplo, IPv6 en París y Londres), pero la red intermedia es diferente (por ejemplo, una red de tránsito IPv4). En lugar de intentar una traducción de protocolos compleja, el router de origen coloca el paquete original (IPv6) dentro del cuerpo de un paquete compatible con la red intermedia (IPv4). El paquete exterior viaja a través del "túnel" IPv4 como carga útil y, en el extremo opuesto, el router receptor remueve la cabecera externa para entregar el paquete original intacto.
# C. Enrutamiento en Dos Niveles

Debido a que Internet está compuesta por miles de redes independientes llamadas **Sistemas Autónomos (AS)** gestionados por múltiples operadores, es imposible utilizar un solo algoritmo global que escale. Por ello, se divide el enrutamiento en dos jerarquías:

1. **Protocolos Intradominio (IGP):** Protocolos internos dentro de cada red individual (ej. OSPF, IS-IS), configurados de forma independiente por el operador de dicha red.
2. **Protocolo Interdominio (EGP):** Un estándar común obligatorio para comunicar los distintos dominios entre sí; en Internet, el estándar exclusivo es **BGP** (_Border Gateway Protocol_), el cual decide rutas basándose en acuerdos comerciales y políticas de enrutamiento.

