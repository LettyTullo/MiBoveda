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
5. **Problema de parámetros:** Indica que se ha detectado un valor ilegal o no válido en un campo de la cabecera IP.
6. **Fuente de enfriamiento (Source Quench):** Un mensaje antiguo (ya casi en desuso) que servía para pedirle a un emisor que redujera su velocidad de transmisión por congestión.
7. **Timestamp Request y Timestamp Reply (Solicitud y Respuesta de Marca de Tiempo)**: Verifica si una máquina está activa, estos mensajes registran la **hora exacta de llegada** del mensaje y la **hora de salida** de la respuesta. Se utilizan principalmente para **medir el rendimiento de la red**, permitiendo calcular con precisión los retardos de ida y vuelta entre dos puntos.
8. **Router Advertisement y Router Solicitation (Anuncio y Solicitud de Router)**
Estos mensajes son esenciales para que los dispositivos finales puedan comunicarse con el resto del mundo:
- **Router Solicitation (Solicitud):** Cuando un host arranca o se conecta a una red, envía este mensaje para **encontrar routers cercanos** de forma activa.
- **Router Advertisement (Anuncio):** Los routers envían estos mensajes periódicamente (o en respuesta a una solicitud) para anunciar su presencia y proporcionar su dirección IP.
- **Objetivo:** Un host necesita conocer la dirección IP de al menos un router (**puerta de enlace predeterminada**) para poder **enviar paquetes fuera de su red local**
# Herramientas Basadas en ICMP

ICMP es la base técnica de dos de las herramientas de diagnóstico más utilizadas en redes:

- **Comando "ping":** Utiliza los mensajes **Echo Request** y **Echo Reply**. El emisor envía un "eco" y el receptor está obligado a devolverlo, confirmando que la comunicación entre ambos es posible.
- **Comando "traceroute" (o tracert):** Utiliza de forma ingeniosa el mensaje de **Tiempo excedido**. Envía una serie de paquetes con el TTL empezando en 1 y aumentando de uno en uno en cada intento. Cada router en el camino descarta el paquete cuando el TTL llega a cero y devuelve un mensaje ICMP de error, permitiendo al emisor identificar cada salto en la ruta hasta el destino final.
## Protocolo ARP (Address Resolution Protocol)
