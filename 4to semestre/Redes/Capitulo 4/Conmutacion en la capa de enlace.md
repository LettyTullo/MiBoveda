## Puentes

Los **puentes** (conocidos modernamente como **conmutadores o switches**) son dispositivos fundamentales de la **capa de enlace de datos** (Capa 2 del modelo OSI) se utilizan fundamentalmente para interconectar múltiples redes de área local (LAN), permitiendo que funcionen como una única red lógica más grande y rápida.

>[!example] Motivos para la utilización de puentes
>- **Interacción entre LANs:** Permiten que departamentos con LANs autónomas y establecidas previamente puedan comunicarse entre sí.
>- **Distribución geográfica:** Facilitan la conexión de redes situadas en distintos edificios mediante enlaces de larga distancia (como fibra óptica).
>- **División de carga:** Una LAN grande puede dividirse en varias LANs independientes interconectadas para manejar una carga mayor, ya que cada segmento puenteado mantiene su propia capacidad de ancho de banda.
>- **Aumento de la fiabilidad:** Al decidir qué tramas reenviar y cuáles no, los puentes actúan como "puertas cortafuegos", impidiendo que un nodo defectuoso que emita tráfico basura afecte a todo el sistema.

# Puentes de Aprendizaje 
La mayoría de los puentes son **transparentes**, lo que significa que son dispositivos "enchufar y listo" (plug-and-play) que no requieren configuración manual. Utilizan el algoritmo de **aprendizaje hacia atrás** para operar eficientemente:
- **Modo promiscuo:** El puente acepta todas las tramas que llegan a sus puertos para examinarlas.
- **Construcción de la tabla CAM:** Al recibir una trama, el puente lee la **dirección MAC de origen**. Si no está en su tabla hash (o tabla CAM), añade la dirección junto con el número de puerto y un temporizador.
- **Decisión de reenvío (Forwarding):** El puente consulta la **dirección MAC de destino** en su tabla.
    - Si el destino está en el mismo puerto por donde llegó, la trama se descarta.
    - Si el destino está en un puerto diferente, la trama se reenvía únicamente por ese puerto.
    - Si el destino es desconocido (**difusión o inundación**), el puente envía la trama por todos los puertos excepto por el de entrada.
- **Envejecimiento (Aging):** Periódicamente, se eliminan las entradas de la tabla que superan unos minutos de antigüedad para adaptarse a cambios en la topología (como mover un ordenador de sitio).

>[!danger] Ojo 
>La afirmación de que las LAN interconectadas con **puentes transparentes** constituyen un único **"dominio de broadcast"** se refiere a que, para mantener la transparencia y permitir que cualquier dispositivo se comunique con otro sin configuración manual, el puente debe asegurar que los mensajes de difusión (broadcast) lleguen a todas las estaciones de la red extendida.

# Protocolo de Árbol de Expansión (Spanning Tree Protocol - STP)

Cuando se instalan enlaces redundantes para aumentar la fiabilidad, se pueden generar **bucles infinitos** de tramas que colapsarían la red. Para evitarlo, los puentes utilizan el estándar **IEEE 802.1d (STP)**:

>[!info] Funcionamiento:
>- **Elección del Puente Raíz (Root Bridge):** Los puentes eligen al dispositivo con el identificador más bajo (basado en la dirección MAC) como la raíz del árbol.
>- **Cálculo de caminos cortos:** Se construye un árbol de rutas óptimas desde la raíz a cada puente.
>- **Bloqueo de puertos:** Cualquier enlace que no forme parte del camino más corto se coloca en estado de bloqueo, eliminando lógicamente el bucle pero manteniendo la redundancia física en caso de fallo

**Rapid Spanning Tree - IEEE 802.1w:** Una versión revisada para lograr una convergencia más rápida tras cambios en la red
# Dispositivos de networking en cada capa

|Capa|Dispositivo Principal|Unidad de Datos|
|---|---|---|
|**Aplicación**|Pasarela de Aplicaciones|Mensaje|
|**Transporte**|Pasarela de Transporte|Segmento|
|**Red**|Router|Paquete|
|**Enlace de datos**|Bridge, Switch|Trama|
|**Física**|Repetidor, Hub|Bit|

# Simbolos de dispositivos de red 

 ![[Pasted image 20260509173817.png|429]]


# Constitución del dominio de broadcast
Debido a que los puentes **no filtran el tráfico broadcast**, sino que lo retransmiten fielmente a todos sus segmentos conectados, el resultado es que una trama de difusión enviada por una estación en la LAN 1 será repetida por los puentes hasta alcanzar a todas las estaciones en la LAN 2, LAN 3, etc..

- Esto define un **dominio de broadcast**: el conjunto de todos los segmentos de red donde una trama de difusión es recibida por todos los hosts.
- Lógicamente, todas las LAN físicas unidas por puentes actúan como **una sola LAN lógica** en cuanto a difusiones se refiere.

# Problemas y soluciones asociados
Aunque este comportamiento permite que protocolos como **ARP** (Protocolo de resolución de direcciones) o **DHCP** funcionen correctamente a través de los puentes, plantea desafíos a medida que la red crece:

- **Consumo de CPU:** A diferencia del tráfico unicast (que las tarjetas de red filtran), las tramas broadcast siempre llegan a la **CPU** de cada host para ser procesadas, lo que puede degradar el rendimiento de todo el sistema si hay demasiado tráfico de este tipo.
- **Tormentas de broadcast:** Un fallo en una interfaz o un bucle en la red puede generar un flujo infinito de difusiones que paralice todas las máquinas del dominio.
- **Aislamiento:** Mientras que los puentes extienden el dominio de broadcast, los **routers** son los dispositivos encargados de aislarlo, ya que no reenvían tráfico broadcast de una red a otra. Por esta razón, se utilizan técnicas como las **VLAN** para "partir" lógicamente el dominio de broadcast dentro de los mismos conmutadores y mejorar así el rendimiento y la seguridad
# Envio de broadcasts a una LAN

>[!rosado] Caracteristicas 
>- Muchos protocolos envían regularmente mensajes broadcast (ARP, DHCP, ICMP y otros).
>-  La cantidad de tráfico broadcast es proporcional al número de equipos.
>- Cuando la LAN crece, el tráfico broadcast aumenta y degrada apreciablemente el rendimiento de hosts
>- El crecimiento de tráfico broadcast puede deberse a: 
>  - Hay un número excesivo de hosts,
>  - Se está utilizando algún protocolo inadecuado, o
>  - Hay algún problema en la red (por ejemplo, virus)

# Division de una LAN en varias LAN fisicas
Para implementar esta división de forma física, se requiere:

- **Múltiples switches:** Es necesario instalar varios conmutadores por cada edificio, gabinete o bastidor ("rack").
- **Cableado extensivo:** Se genera una gran cantidad de cableado para cada LAN individual, así como enlaces troncales entre edificios y racks.
- **Puentes transparentes:** Se utilizan estos dispositivos (normalmente switches modernos) para unir los segmentos físicos y que funcionen como una única red lógica

## Redes de area local virtuales VLANs

Las VLAN  son una tecnología que permite dividir lógicamente una red física grande en varias redes lógicas más pequeñas e independientes. Básicamente, equivale a **"partir" un conmutador (switch) físico en otros más pequeños y virtuales**, permitiendo que la disposición lógica de la red no dependa de la geografía del edificio.
# 1. Objetivos de la implementación de VLANs

> [!info] Las organizaciones utilizan VLANs por tres razones fundamentales:
>- **Rendimiento:** Reducen el **tráfico de difusión (broadcast)**. En una LAN única, las tramas broadcast llegan a la CPU de todos los hosts, lo que degrada el rendimiento; las VLANs limitan estas tormentas a un grupo específico de puertos.
>- **Seguridad:** Permiten aislar colectivos (como Finanzas vs. Ingeniería). Los paquetes de una VLAN no son recibidos por los miembros de otra, impidiendo que usuarios no autorizados "escuchen" tráfico sensible.
>- **Flexibilidad:** Permiten la configuración de la red por **software**. Se puede mover a un usuario de departamento sin necesidad de cambiar cables físicos, simplemente reasignando su puerto en el switch.

# 2. Funcionamiento y el Estándar IEEE 802.1Q

Para que las VLANs funcionen, los switches deben ser **"VLAN-aware" (gestionables)**.

![[Pasted image 20260523110924.png|523]]

>[!amarillo] El mecanismo clave es el etiquetado de tramas:
>- **Standard 802.1Q:** Fue introducido para permitir que tramas de diferentes VLANs viajen por el mismo cable.
>- **Etiqueta (Tag):** Se inserta un campo de **4 bytes** en la cabecera Ethernet original.
>- **Identificador de VLAN (VID):** Dentro de la etiqueta, hay **12 bits** destinados a identificar la VLAN, lo que permite hasta **4096 redes distintas** en un mismo sistema.
>- **Prioridad:** Incluye 3 bits para gestionar la Calidad de Servicio (QoS), distinguiendo, por ejemplo, el tráfico de voz en tiempo real del tráfico de datos normal.
>- CFI: Solo se utiliza en Token Ring 

# 3. Interconexión y Tipos de Enlaces

>[!warning] Existen dos formas principales de conectar dispositivos en un entorno VLAN:
>- **Puertos de Acceso:** Conectan a los hosts finales (PCs, impresoras). Para el host, la VLAN es transparente; no recibe tramas etiquetadas, ya que el switch añade/quita la etiqueta al entrar/salir del puerto.
>- **Puertos Troncales (Trunk):** Conectan switches entre sí o con un router. Estos enlaces transportan tramas de **múltiples VLANs simultáneamente**, por lo que es obligatorio que las tramas viajen etiquetadas según el estándar **802.1Q** para que el receptor pueda separarlas.
>- **Interconexión Inter-VLAN:** Los puertos asignados a una VLAN se comportan como un switch independiente; por lo tanto, para que una estación de la VLAN "Roja" hable con una de la VLAN "Azul", **se requiere obligatoriamente un router**.

# 4. Asignación de puertos

Existen tres métodos principales para decidir a qué VLAN pertenece un dispositivo:

1. **Estático (por configuración):** Es el método más común. El administrador establece una tabla fija de **Puerto físico vs. Número de VLAN**.
2. **Dinámico por dirección MAC:** El switch asigna la VLAN basándose en la identidad del dispositivo, permitiendo que el usuario se mueva a cualquier puerto del edificio y mantenga su red.
3. **Dinámico por autentificación (802.1x):** La VLAN se asigna tras validar el usuario y contraseña del cliente.

# 5. Spanning Tree con VLANs

Cuando hay enlaces redundantes en la red para evitar caídas, se utiliza el protocolo **STP (Spanning Tree Protocol)**. 

>[!example] Con VLANs, el comportamiento es el siguiente:
>- **Independencia:** Cada VLAN construye su propio árbol de expansión de forma **independiente**.
>- **Bloqueo de puertos:** Esto permite que un puerto que está bloqueado para una VLAN (para evitar un bucle) pueda estar activo para otra, optimizando el uso de los enlaces físicos.
>- **Configuración:** Los puentes eligen un **Puente Raíz** por cada VLAN. A igual costo de camino, se bloquea el puerto con el identificador más alto. Se puede modificar la prioridad de un puerto (por defecto 128) para forzar qué camino debe ser el principal y cuál el de reserva.

[[Capa de Red parte 1]]
