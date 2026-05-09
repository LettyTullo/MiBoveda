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