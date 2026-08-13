Los algoritmos de enrutamiento son la parte del software de la capa de red encargada de decidir por qué línea de salida debe transmitirse un paquete que llega a un enrutador. 
# Propiedades 
Independientemente de si las rutas se eligen para cada paquete por separado o solo al establecer una conexión, un algoritmo de enrutamiento debe poseer las siguientes propiedades:

>[!example]
>- **Corrección y Simplicidad:** El algoritmo debe encontrar rutas válidas de forma sencilla.
>- **Robustez:** Debe ser capaz de lidiar con cambios en la topología (como fallos en enrutadores o líneas) y variaciones en el tráfico sin necesidad de reiniciar toda la red.
>- **Estabilidad:** El algoritmo debe alcanzar un estado de equilibrio (convergencia) y mantenerse en él.
>- **Equidad:** Debe buscar un balance para que las conexiones individuales reciban un trato justo en la red.
>- **Eficiencia:** Debe optimizar el uso de los recursos globales de la red.
# Objetivos de los algoritmos

El objetivo principal es **descubrir caminos** en la red para transportar paquetes desde el origen al destino. De manera más específica, los algoritmos suelen buscar:

- **Minimizar la distancia** que debe recorrer el paquete o el **número de saltos** necesarios para llegar al destino.
- **Mejorar el retardo** y reducir el **ancho de banda consumido** por cada paquete, lo que a su vez mejora la velocidad de transferencia y el rendimiento global de la red.
- Evitar la sobrecarga de ciertas líneas o routers mientras otros permanecen sin utilizar
## El principio de la optimizacion
El **principio de optimización** (o principio de optimalidad) es una afirmación general sobre las rutas óptimas que sirve como base para casi todos los algoritmos de enrutamiento, independientemente de la topología o el tráfico de la red.
# 1. La lógica principal
El principio establece que si un enrutador **J** se encuentra en el camino óptimo entre el enrutador **I** y el enrutador **K**, entonces el camino óptimo desde **J** hasta **K** también sigue esa misma ruta.

>[!info] ¿Por qué es así?
>Imagina que la ruta de **I** a **K** pasa por **J**. Si existiera un camino mejor (más corto o rápido) para ir de **J** a **K**, podrías "pegar" ese nuevo camino a la primera parte (de **I** a **J**) y tendrías una ruta mejor para ir de **I** a **K**. Esto contradice la idea original de que el primer camino era el "óptimo". Por lo tanto, cualquier parte de un camino óptimo debe ser, por definición, óptima también.
# 2. El Árbol Sumidero (Sink Tree)
Como consecuencia directa de este principio, si juntas todos los mejores caminos desde todos los orígenes hacia un destino específico, el resultado es una estructura llamada **árbol sumidero**.

- **Forma de árbol:** Se llama así porque todos los caminos convergen en el enrutador de destino (la raíz del árbol).
- **Sin bucles:** Una propiedad fundamental de los árboles es que **no contienen ciclos o bucles**. Esto garantiza que un paquete no se quede dando vueltas infinitamente y llegue a su destino en un número finito de saltos.
- **Métricas:** El concepto de "mejor" camino depende de lo que el algoritmo quiera optimizar: puede ser el menor número de saltos, la distancia geográfica, el menor retardo o el mayor ancho de banda.
# 3. ¿Por qué es importante?
El objetivo final de cualquier algoritmo de enrutamiento (como Dijkstra o Bellman-Ford) es **descubrir y utilizar estos árboles sumideros** para cada enrutador de la red.
Aunque en la práctica los fallos de routers o enlaces pueden hacer que diferentes nodos tengan ideas distintas sobre la topología, el principio de optimización proporciona el estándar o la "referencia" ideal para medir qué tan bien está funcionando un algoritmo de enrutamiento