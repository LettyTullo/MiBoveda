El protocolo **IPv4 (Internet Protocol versión 4)** es el "pegamento" que mantiene unida a la Internet, encargándose de transportar paquetes de datos desde un origen a un destino de la manera más eficiente posible (**"mejor esfuerzo"**), sin importar cuántas redes intermedias deba atravesar.
##  El Formato del Datagrama IPv4

![[Pasted image 20260512150703.png|605]]
Un paquete IPv4 consta de una **cabecera** (mínimo 20 bytes) y un **cuerpo** con la carga útil. Los campos más relevantes de la cabecera son:

- **IHL (Internet Header Length):** Indica la longitud de la cabecera.
- **TTL (Time to Live):** Un contador que se reduce en cada router; evita que los paquetes circulen infinitamente si hay un bucle.
- **Protocolo:** Indica a qué protocolo de transporte (como **TCP** o **UDP**) debe entregarse el paquete en el destino.
- **Checksum:** Verifica la integridad únicamente de la cabecera IP.
- **Direcciones de Origen y Destino:** Campos de 32 bits que identifican las interfaces de red de los dispositivos involucrados.

### 2. Direccionamiento IPv4

Una dirección IPv4 es una etiqueta lógica de 32 bits que define la conexión de un dispositivo (host, router o servidor) a una red.

- **Estructura Jerárquica:** Cada dirección se divide en dos partes principales: un **Número de RED** y un **Número de HOST**. Esto permite que los routers solo necesiten conocer las rutas hacia las redes y no hacia cada dispositivo individual, lo que hace que las tablas de enrutamiento sean manejables.
- **Notación:** Se expresan comúnmente en **formato decimal con puntos**, agrupando los 32 bits en 4 octetos (8 bits cada uno) separados por puntos (ej. `128.11.3.31`).
- **Prefijos y Máscaras:** La longitud de la parte de red se define mediante un **prefijo** o **máscara de subred**.
    - **Notación CIDR:** Se usa una barra seguida del número de bits de red (ej. `/24`).
    - **Máscara de subred:** Es una secuencia de bits donde los "1" indican la parte de red y los "0" la parte de host (ej. `255.255.255.0` corresponde a un `/24`).

### 3. Tipos de Direcciones y Reglas Especiales

Dentro de un bloque de direcciones, existen valores con funciones específicas:

- **Dirección de Red:** Es la dirección más baja del bloque (todos los bits de host en "0") y sirve para identificar el segmento de red; **no se puede asignar** a ningún host.
- **Dirección de Broadcast (Difusión):** Es la dirección más alta del bloque (todos los bits de host en "1") y se usa para enviar un paquete a **todos** los dispositivos de esa red simultáneamente.
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