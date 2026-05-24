## Protocolo ICMP (Internet Control Message Protocol)
El protocolo **ICMP es un subprotocolo fundamental de la capa de red que actúa como el mecanismo de **control y notificación de errores** para el Protocolo de Internet (IP).

A diferencia de IP, que se encarga del transporte de datos con el "mejor esfuerzo", ICMP permite que los routers y hosts informen sobre problemas encontrados durante el procesamiento de los paquetes.
# Funcionamiento y Encapsulamiento
Aunque opera en la capa de red (Capa 3), el protocolo ICMP no envía sus mensajes directamente sobre la capa de enlace. En su lugar:

- Un mensaje ICMP se entrega a la capa **IP**, la cual lo **encapsula** dentro de un datagrama IP estándar como si fueran datos normales antes de transmitirlo.
- Cada mensaje contiene una **cabecera** con tres campos fijos de 4 bytes en total: **Tipo** (8 bits), **Código** (8 bits) y **Suma de comprobación** (16 bits), seguidos de datos que dependen del tipo de mensaje.
![[Pasted image 20260516191305.png|573]]
# Principales Tipos de Mensajes ICMP

Existen diversos tipos de mensajes definidos para diferentes diagnósticos y situaciones de error:
1. **Destino inalcanzable (Destination Unreachable):** Se genera cuando un paquete no puede ser entregado. Puede ser enviado por un router que no localiza el destino o por el propio host de destino si el protocolo de usuario no está disponible. También ocurre si un router necesita fragmentar un paquete pero el bit **DF (Don't Fragment)** está activado.
2. **Tiempo excedido (Time Exceeded):** Se envía cuando un paquete es descartado porque su campo **TTL (Time to Live)** ha llegado a cero. Esto suele indicar que el paquete está atrapado en un bucle de enrutamiento o que el valor inicial del TTL era demasiado bajo.
3. **Redirección (Redirect):** Un router utiliza este mensaje para informar a un host que existe una **ruta mejor** o más corta para llegar a un destino específico, enseñándole así sobre la "geografía" de la red.
4. **Eco y Respuesta a Eco (Echo Request/Reply):** Se utilizan para comprobar si una máquina está activa y es alcanzable en la red.

# Herramientas Basadas en ICMP

ICMP es la base técnica de dos de las herramientas de diagnóstico más utilizadas en redes:

- **Comando "ping":** Utiliza los mensajes **Echo Request** y **Echo Reply**. El emisor envía un "eco" y el receptor está obligado a devolverlo, confirmando que la comunicación entre ambos es posible.
- **Comando "traceroute" (o tracert):** Utiliza de forma ingeniosa el mensaje de **Tiempo excedido**. Envía una serie de paquetes con el TTL empezando en 1 y aumentando de uno en uno en cada intento. Cada router en el camino descarta el paquete cuando el TTL llega a cero y devuelve un mensaje ICMP de error, permitiendo al emisor identificar cada salto en la ruta hasta el destino final.
## Protocolo ARP (Address Resolution Protocol)
El protocolo **ARP (Address Resolution Protocol)** es un mecanismo fundamental de la capa de red que se encarga de **mapear(resolucion) direcciones lógicas (IP) a direcciones físicas (MAC)** dentro de una red de área local.
# ¿Por qué es necesario el ARP?

Aunque las aplicaciones y el software de red utilizan direcciones IP para identificar los destinos, las tarjetas de red (NIC) de la capa de enlace de datos, como las de Ethernet, **no entienden las direcciones IP**. Las NIC solo pueden enviar y recibir tramas basándose en direcciones físicas de 48 bits (direcciones MAC). Por lo tanto, se necesita un "traductor" que convierta la dirección IP en una dirección MAC para que el paquete pueda viajar por el cable físico.
# Funcionamiento básico (Solicitud y Respuesta)
Cuando un host necesita enviar un paquete a una dirección IP específica dentro de su misma red:

1. **Solicitud ARP (Request):** El host emisor envía un paquete de **difusión (broadcast)** a toda la red preguntando: "¿Quién tiene la dirección IP X? Por favor, dime tu dirección MAC". Este mensaje incluye la IP y la MAC del emisor para que el destinatario sepa a quién responder.
2. **Respuesta ARP (Reply):** Todas las máquinas de la red reciben la pregunta, pero **solo aquella que posee la dirección IP especificada** responde con un mensaje enviando su dirección MAC.
3. **Encapsulamiento:** Una vez que el emisor recibe la MAC, puede encapsular el paquete IP en una trama Ethernet y enviarlo directamente al destino.

# La Tabla ARP (Caché)

Para evitar saturar la red con mensajes de difusión cada vez que se quiere enviar un dato, cada host mantiene una **Tabla ARP** en su memoria.

- Esta tabla relaciona temporalmente las direcciones IP con sus correspondientes direcciones MAC.
- Las entradas en esta tabla tienen un **tiempo de vida (TTL)** limitado (usualmente unos minutos) y expiran si no hay comunicación, obligando a realizar una nueva búsqueda ARP si se requiere contactar al equipo de nuevo.

# Casos especiales y variantes

- **Envío fuera de la red local:** Si el host destino no está en la misma subred, el emisor no realiza ARP por la IP del destino final. En su lugar, realiza ARP para obtener la dirección MAC de su **puerta de enlace predeterminada** (el router) y le envía el paquete a él para que lo encamine.
- **Proxy ARP:** Es una configuración donde un router responde a solicitudes ARP en nombre de hosts que se encuentran en otras redes, haciendo que el solicitante crea que el destino está en su propia red local.
- **ARP gratuito:** Ocurre cuando un host difunde su propia dirección IP al conectarse a la red. Sirve para actualizar las tablas ARP de otros hosts y para detectar si hay **direcciones IP duplicadas** en la red (si alguien responde, hay un conflicto).
## Protocolo DHCP 
El **DHCP** (Dynamic Host Configuration Protocol o Protocolo de Configuración Dinámica de Host) es un protocolo de la capa de red diseñado para la **asignación automática y dinámica de direcciones IP** y otros parámetros de configuración a los dispositivos que se conectan a una red.
# Propósito y necesidad

Aunque es posible configurar manualmente la dirección IP de cada ordenador, este proceso es tedioso y propenso a errores en redes grandes. El DHCP soluciona esto permitiendo que una red tenga un servidor encargado de gestionar un **conjunto de direcciones IP** y asignarlas a los equipos a medida que estos se activan, recuperándolas cuando dejan de usarse.

# Proceso de funcionamiento (4 pasos clave)

Cuando un dispositivo (cliente) se conecta a la red y no tiene una dirección IP, inicia un intercambio de mensajes con el servidor DHCP:

1. **DHCP DISCOVER:** El cliente envía un mensaje de difusión (**broadcast**) a toda la red para localizar servidores DHCP disponibles. En este mensaje, el cliente incluye su dirección física (**MAC**) para que el servidor pueda identificarlo.
2. **DHCP OFFER:** El servidor DHCP recibe la solicitud, selecciona una dirección IP libre de su lista y la envía al cliente en un mensaje de oferta.
3. **DHCP REQUEST:** Como puede haber más de un servidor DHCP en la red, el cliente envía este mensaje para aceptar formalmente una de las ofertas recibidas.
4. **DHCP ACK:** El servidor confirma la asignación con un acuse de recibo, indicando que la configuración está lista para ser usada.

_Nota: Si el servidor DHCP no está en la misma red local, los routers pueden configurarse para recibir estas difusiones y retransmitirlas hacia donde se encuentre el servidor._


[[Fin]]
