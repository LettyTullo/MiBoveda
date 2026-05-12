Esta guía de estudio sobre **IP (Internet Protocol)** y la **Capa de Red** se basa en el material de referencia (Tanenbaum) y los conceptos clave relacionados con el direccionamiento y funcionamiento del protocolo en el nivel 3 del modelo OSI.
# Guía de Estudio: IP y la Capa de Red (Diapositiva 11)

### **1. Introducción a la Capa de Red**

La **Capa de Red** (Capa 3) es la encargada de enviar paquetes de longitud finita entre computadoras, incluso a través de múltiples saltos o redes intermedias.

- **Función principal:** A diferencia de la capa de enlace, que maneja la comunicación directa entre dos nodos, la capa de red decide cuál de las líneas de salida utilizar para encaminar el paquete hacia su destino final.
- **Unidad de datos:** En este nivel, la unidad de información se denomina **Paquete**.
- **Subred:** Originalmente, el término se refería a la colección de routers y líneas de comunicación que mueven los paquetes del origen al destino.
### **2. Formato del Paquete IP (Cabecera)**

El encabezado IP contiene información de control crucial para que los routers puedan procesar y reenviar los datos correctamente. Los campos más importantes son:

- **TTL (Time to Live):** Un contador que se reduce en cada salto; evita que los paquetes circulen infinitamente en caso de bucles en la red.
- **Protocolo:** Indica qué protocolo de la capa superior (como TCP o UDP) debe recibir los datos del paquete.
- **Suma de comprobación (Checksum):** Se utiliza para verificar la integridad de la cabecera IP.
- **Direcciones de Origen y Destino:** Identifican de manera única a los hosts en la internetwork.
### **3. Direccionamiento IP y Subnetting**
El direccionamiento permite organizar la red de forma jerárquica para facilitar el enrutamiento.

- **Prefijos IP:** Se utiliza la notación **CIDR** (como `/18`) para indicar cuántos bits corresponden a la parte de la red y cuántos a los hosts.
- **Subnetting (Subredes):** Consiste en dividir un prefijo IP grande en redes más pequeñas e independientes. Por ejemplo, un bloque `128.208.0.0/18` puede dividirse para diferentes departamentos (Informática, Ingeniería, Arte), utilizando una "barra vertical" lógica para separar el número de subred del número de host.
- **Routers y Subredes:** Cuando un paquete llega al router principal, este consulta sus tablas de rutas para decidir a qué subred específica debe asignar el tráfico basándose en los bits del prefijo.
### **4. Protocolos de Control y Auxiliares**
Para que IP funcione, requiere de protocolos adicionales que gestionen errores e información de direcciones físicas:

- **ICMP (Internet Control Message Protocol):** Se utiliza para enviar mensajes de error e información de control entre routers y hosts.
- **ARP (Address Resolution Protocol):** Es el protocolo encargado de traducir una dirección IP (Capa 3) a una dirección MAC física (Capa 2) dentro de una LAN, como Ethernet 802.3.
- **DHCP:** Permite la configuración automática de parámetros de red, como la asignación de direcciones IP dinámicas.
### **5. Enrutamiento (Routing)**
El enrutamiento es el proceso de elegir el camino óptimo a través de la red.

- **Algoritmo de Dijkstra:** Se utiliza para calcular el **camino más corto** entre dos nodos basándose en métricas como el número de saltos, el retardo o el ancho de banda.
- **Inundación (Flooding):** Una técnica simple donde cada router reenvía el paquete por todas sus interfaces. Para evitar saturación, se usan números de secuencia para descartar paquetes duplicados u obsoletos.
- **Enrutamiento Jerárquico:** A medida que la red crece, se dividen los routers en regiones para que cada uno solo necesite conocer la topología de su propia zona, reduciendo el tamaño de las tablas de rutas.