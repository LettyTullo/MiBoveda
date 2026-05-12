El protocolo **IPv4 (Internet Protocol versión 4)** es el "pegamento" que mantiene unida a la Internet, encargándose de transportar paquetes de datos desde un origen a un destino de la manera más eficiente posible (**"mejor esfuerzo"**), sin importar cuántas redes intermedias deba atravesar.
##  El Formato del Datagrama IPv4

![[Pasted image 20260512150703.png|605]]
La cabecera tiene una **parte fija de 20 bytes** y una parte opcional de longitud variable.

>[!info] Explicacion de cada campo 
>- **Versión (4 bits):** Indica la versión del protocolo (en este caso, 4).
>- **IHL (4 bits):** Indica la longitud de la cabecera en palabras de **32 bits**. El valor mínimo es 5 (20 bytes) y el máximo 15 (60 bytes).
>- **Servicios Diferenciados (8 bits):** Se utiliza para distinguir la clase de servicio de los paquetes (QoS) y para señalar indicaciones de congestión.
>- **Longitud Total (16 bits):** Incluye la cabecera y los datos, con un máximo de **65,535 bytes**.
>- **Identificación (16 bits):** Permite al host de destino identificar a qué datagrama pertenecen los fragmentos recibidos.
>- **Flags (DF y MF):**
>	- **DF (Don't Fragment):** Ordena a los routers no fragmentar el paquete.
>	- **MF (More Fragments):** Indica si hay más fragmentos después del actual.
>- **Desplazamiento de Fragmento (13 bits):** Indica en qué parte del datagrama original se sitúa el fragmento.
>- **Tiempo de Vida (TTL) (8 bits):** Contador que se reduce en cada salto (router) para evitar que los paquetes circulen infinitamente; si llega a cero, el paquete se descarta.
>- **Protocolo (8 bits):** Indica a qué proceso de la capa de transporte (como **TCP** o **UDP**) debe entregarse el paquete en el destino.
>- **Suma de Comprobación (Checksum) (16 bits):** Verifica la integridad de la cabecera.
>- **Direcciones de Origen y Destino (32 bits cada una):** Indican las direcciones IP de las interfaces de red del emisor y receptor.
>- **Opciones (Variable):** Diseñado para permitir pruebas, seguridad o enrutamiento específico, aunque hoy en día muchos routers las ignoran.

# Direccionamiento IPv4
Una dirección IPv4 es una etiqueta lógica de **32 bits** que define la conexión de un dispositivo a una red (un router, un servidor u host).

- **Notación:** Se escribe comúnmente en formato **decimal con puntos**, dividiendo los 32 bits en 4 octetos (ej. `128.208.2.151`).
- **Jerarquía:** Cada dirección consta de un **Número de RED** y un **Número de HOST**. Esto permite a los routers encaminar basándose solo en el prefijo de red, reduciendo el tamaño de las tablas de rutas.
- **Prefijos y Máscaras:**
    - **CIDR(Enrutamiento Entre Dominios sin Clases):** Usa una barra seguida de la longitud del prefijo (ej. `/24`).
    - **Máscara de subred:** Es una secuencia de bits donde los "1" indican la parte de red y los "0" la de host (ej. `255.255.255.0`).

>[!danger] ¿Qué es un Prefijo?
>Un **prefijo** es un bloque contiguo de espacio de direcciones IP donde todos los dispositivos comparten los mismos bits iniciales (la parte de red).
> **Notación CIDR:** Los bloques de direcciones se definen como `x.y.z.t /n`, donde `x.y.z.t` es una de las direcciones del bloque y **/n** indica la longitud del prefijo en bits. Por ejemplo, `/24` indica que los primeros 24 bits identifican a la red.

# 3. Tipos de Direcciones y Reglas Especiales

Dentro de un bloque de direcciones, existen valores con funciones específicas:

- **Dirección de Red:** Es la dirección más baja del bloque (todos los bits de host en "0") y sirve para identificar el segmento de red; **no se puede asignar** a ningún host.
- **Dirección de Broadcast (Difusión):** Es la dirección más alta del bloque (todos los bits de host en "1") y se usa para enviar un paquete a **todos** los dispositivos de esa red simultáneamente.
>[!amarillo] Cantidad de direcciones posibles a asignar
>$$Host = 2^k- 2$$
k = Cantidad de bits de host
- **Direcciones Especiales:**
    - **0.0.0.0:** Utilizada por los hosts durante el proceso de arranque.
    - **127.x.x.x (Loopback):** Reservada para pruebas internas del propio host; los paquetes enviados aquí nunca salen al cable físico.
    - **Direcciones Privadas:** Rangos (como `10.0.0.0/8`, `172.16.0.0/12` o `192.168.0.0/16`) reservados para redes locales que no son visibles directamente en Internet y requieren **NAT** para navegar.

### 4. División en Subredes (Subnetting)

Consiste en dividir un bloque de red grande en segmentos más pequeños e independientes. Su objetivo principal es **reducir el tráfico de broadcast** y mejorar la organización administrativa, permitiendo que cada subred sea vista externamente como una sola red unificada.

### 5. Protocolos Auxiliares Necesarios

Para que el direccionamiento IP sea efectivo en una red real, se apoya en otros protocolos:

- **ARP (Address Resolution Protocol):** Traduce una dirección IP lógica a una dirección **MAC física** para poder enviar la trama por el cable (ej. Ethernet).
- **DHCP (Dynamic Host Configuration Protocol):** Asigna automáticamente direcciones IP dinámicas y otros parámetros de red a los dispositivos cuando se conectan.
- **ICMP (Internet Control Message Protocol):** Envía mensajes de diagnóstico y error entre routers y hosts (ej. cuando un destino es inalcanzable).