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

 ![[Pasted image 20260509173525.png|358]]

# Simbolos de dispositivos de red 
 ![[Pasted image 20260509173817.png|429]]