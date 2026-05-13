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

## Direccionamiento IPv4
Una dirección IPv4 es una etiqueta lógica de **32 bits** que define la conexión de un dispositivo a una red (un router, un servidor u host).

- **Notación:** Se escribe comúnmente en formato **decimal con puntos**, dividiendo los 32 bits en 4 octetos (ej. `128.208.2.151`).
- **Jerarquía:** Cada dirección consta de un **Número de RED** y un **Número de HOST**. Esto permite a los routers encaminar basándose solo en el prefijo de red, reduciendo el tamaño de las tablas de rutas.
- **Prefijos y Máscaras:**
    - **CIDR(Enrutamiento Entre Dominios sin Clases):** Es el sistema estándar actual para la asignación de direcciones IP y el enrutamiento de paquetes en Internet. Usa una barra seguida de la longitud del prefijo (ej. `/24`).

>[!danger] ¿Qué es un Prefijo?
>Un **prefijo** es un bloque contiguo de espacio de direcciones IP donde todos los dispositivos comparten los mismos bits iniciales (la parte de red).
> **Notación CIDR:** Los bloques de direcciones se definen como `x.y.z.t /n`, donde `x.y.z.t` es una de las direcciones del bloque y **/n** indica la longitud del prefijo en bits. Por ejemplo, `/24` indica que los primeros 24 bits identifican a la red.

>[!example] Que es una Mascara?
> Dado que la longitud de la parte de red no se puede adivinar solo mirando la dirección IP, se utiliza la **máscara de subred** para indicar dónde termina la red y dónde empieza el host.
>- **Estructura binaria:** Es una secuencia de 32 bits que contiene **"1"** en todas las posiciones correspondientes a la parte de red (prefijo) y **"0"** en las posiciones de la parte de host.
>- **Relación visual:** Si un prefijo tiene una longitud **L**, su máscara tendrá exactamente **L** unos seguidos de **32–L** ceros.
>- **Conversión a decimal:** Las máscaras también se expresan en formato decimal con puntos. Por ejemplo, un prefijo **/20** (20 unos seguidos de 12 ceros) equivale a la máscara **255.255.240.0**
# Tipos de Direcciones y Reglas Especiales

Dentro de un bloque de direcciones, existen valores con funciones específicas:

- **Dirección de Red:** Es la dirección más baja del bloque (todos los bits de host en "0") y sirve para identificar el segmento de red; **no se puede asignar** a ningún host.
- **Dirección de Broadcast (Difusión):** Es la dirección más alta del bloque (todos los bits de host en "1") y se usa para enviar un paquete a **todos** los dispositivos de esa red simultáneamente.
>[!amarillo] Maximo numero de hosts en un bloque (red o suubred)
>$$Host = 2^k- 2$$
k =( 32 - prefijo)
- **Direcciones Especiales:**
    - **0.0.0.0:** Utilizada por los hosts durante el proceso de arranque.
    - **127.x.x.x (Loopback):** Reservada para pruebas internas del propio host; los paquetes enviados aquí nunca salen al cable físico.
    - **Direcciones Privadas:** Rangos (como `10.0.0.0/8` hasta `10.255.255.255/8` , `172.16.0.0/12` hasta `172.31.255.255/12`o `192.168.0.0/16` hasta `192.168.255.255/16`) reservados para redes locales que no son visibles directamente en Internet y requieren **NAT** para navegar.


# Subnetting (Division en Subredes)
La **división en subredes** (_subnetting_) es una técnica que permite fragmentar un bloque de direcciones IP grande en varias redes más pequeñas e independientes para uso interno, mientras que para el resto de Internet siguen pareciendo una única red.

La implementación de subredes responde a varias necesidades técnicas y administrativas:
- **Reducción del tráfico de difusión (broadcast):** Este es el motivo principal; al dividir una LAN grande, se limita el alcance de los mensajes de difusión, mejorando el rendimiento general.
- **Organización jerárquica:** Permite que la red refleje la estructura de la organización (por ejemplo, dividiendo un bloque en subredes para los departamentos de Informática, Ingeniería y Arte).
- **Seguridad y gestión:** Facilita la administración interna y permite aislar el tráfico entre diferentes grupos de usuarios, dificultando ataques entre colectivos.
- **Eficiencia en el direccionamiento:** Evita el desperdicio de direcciones IP al adaptar el tamaño de cada subred a la cantidad real de hosts que necesita.
# Funcionamiento técnico

Para dividir una red, se toman bits que originalmente pertenecían a la **porción de host** y se utilizan para identificar la **subred**.

- **Transparencia externa:** Desde fuera de la organización, la red se ve como un único prefijo (por ejemplo, un `/16`). Los routers externos no necesitan conocer la estructura interna, lo que ayuda a que las tablas de enrutamiento globales no colapsen.
- **Uso de máscaras de subred:** Los routers internos utilizan la **máscara de subred** para procesar los paquetes. Cuando llega un paquete, el router realiza una operación lógica **AND** entre la dirección IP de destino y la máscara de la subred para determinar a qué segmento enviarlo.
- **Flexibilidad:** La división no tiene que ser uniforme. Un bloque se puede partir en trozos de diferentes tamaños (por ejemplo, una mitad como `/17`, un cuarto como `/18`, etc.) según las necesidades.


////mirar que es esto, se divide otra vez
**CIDR** son las siglas de **Classless Inter-Domain Routing** (Enrutamiento Entre Dominios sin Clases). Es el sistema estándar actual para la asignación de direcciones IP y el enrutamiento de paquetes en Internet.

A continuación se detallan sus características principales según las fuentes:

### 1. Propósito: Agregación de Rutas

El objetivo fundamental de CIDR es frenar el crecimiento explosivo de las **tablas de enrutamiento** en los routers troncales de Internet.

- Para lograrlo, utiliza la **agregación de rutas**, que permite juntar múltiples prefijos IP pequeños en uno solo más grande (a veces llamado **superred**).
- De esta forma, un router distante solo necesita una entrada en su tabla para representar a miles de hosts o redes pequeñas.

### 2. Funcionamiento y Notación

A diferencia del direccionamiento por clases antiguo (donde los bloques eran fijos como Clase A, B o C), CIDR permite que los prefijos tengan cualquier longitud.

- **Notación:** Se escribe como una dirección IP seguida de una barra y el número de bits de la red (ejemplo: `194.24.0.0/21`).
- **Flexibilidad:** Permite que un bloque de direcciones se divida o combine de forma mucho más eficiente, adaptándose a las necesidades reales de cada organización.

### 3. Regla del Prefijo más Largo Coincidente

Debido a que con CIDR un destino puede estar contenido en varios prefijos de diferentes tamaños dentro de la misma tabla de enrutamiento, los routers aplican la regla del **prefijo más largo coincidente** (_longest matching prefix_).

- Esto significa que si un paquete coincide con una entrada de máscara `/20` y otra de `/24`, el router elegirá la entrada `/24` por ser la más específica para ese destino.
.