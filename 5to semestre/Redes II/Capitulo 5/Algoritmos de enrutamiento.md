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
## Algoritmo de Dijkstra 
Para aplicar el algoritmo, la red se representa como un grafo donde cada nodo es un enrutador y cada arista es un enlace de comunicación. A cada enlace se le asigna un **peso o costo no negativo**, que puede representar la distancia geográfica, el retardo medio, el ancho de banda o el tráfico promedio.
# Tipos de etiquetas
El algoritmo utiliza etiquetas en los nodos para rastrear el progreso. Estas etiquetas contienen la distancia desde el origen y el nodo predecesor (para poder reconstruir la ruta al final). Existen dos estados para estas etiquetas:

- **Provisionales**: Se asignan inicialmente a los nodos cuando se descubre un camino, pero aún es posible encontrar uno mejor.
- **Permanentes**: Una vez que el algoritmo confirma que una etiqueta representa el camino más corto posible desde el origen hasta ese nodo, la marca como permanente y su valor ya nunca cambia.

>[!example] Funcionamiento
>1. **Inicialización**: Se marca el nodo de origen como **permanente** con una distancia de 0. Todos los demás nodos se marcan como **provisionales** con una distancia de **infinito** y sin predecesor.
>2. **Examen de vecinos (Relajación)**: El algoritmo toma el nodo que acaba de hacerse permanente (llamado "nodo de trabajo") y examina a todos sus vecinos que aún sean provisionales.
>3. **Actualización de costos**: Para cada vecino, se calcula la distancia desde el origen sumando la etiqueta del nodo de trabajo y el costo del enlace hacia ese vecino. Si este nuevo valor es **menor** que la etiqueta actual del vecino, se actualiza su etiqueta con el nuevo valor y se marca al nodo de trabajo como su nuevo predecesor.
>4. **Selección del mínimo**: Tras examinar a los vecinos, el algoritmo busca entre **todos** los nodos provisionales del grafo aquel que tenga la **etiqueta de distancia más pequeña**.
>5. **Cierre de ciclo**: Ese nodo con la distancia mínima se convierte en **permanente** y pasa a ser el nuevo nodo de trabajo para la siguiente ronda.
>6. **Finalización**: Los pasos 2 a 5 se repiten hasta que el nodo de destino se vuelve permanente o todos los nodos han sido procesados.
La **inundación** (o _flooding_) es un algoritmo de enrutamiento estático y sencillo cuyo objetivo es hacer llegar un paquete a **todos los nodos** de la red.

## Inundacion
Bajo esta técnica, cada vez que un enrutador recibe un paquete entrante, lo retransmite por **todas sus líneas de salida**, excepto por aquella por la que el paquete llegó originalmente.

Su principal problema es que genera una **gran cantidad de paquetes duplicados**, lo que puede saturar la red o incluso crear bucles infinitos. Para evitar esto, se utilizan dos métodos de control:

1. **Límite de saltos (TTL):** Se incluye un contador en la cabecera de cada paquete que disminuye en uno en cada enrutador. Cuando el contador llega a cero, el paquete se descarta.
2. **Números de secuencia:** El enrutador de origen asigna un número de secuencia a cada paquete. Los enrutadores intermedios mantienen una lista de los paquetes que ya han procesado (identificados por origen y secuencia); si llega un paquete que ya está en la lista, no se vuelve a inundar.
# Ventajas y aplicaciones
A pesar de su ineficiencia en el uso de ancho de banda, tiene propiedades valiosas:
- **Extrema robustez:** Es ideal para aplicaciones militares o redes donde muchos nodos pueden fallar simultáneamente, ya que si existe un camino al destino, la inundación lo encontrará.
- **Métrica de camino más corto:** Como explora todos los caminos posibles en paralelo, garantiza que la primera copia en llegar al destino lo haga por la ruta más corta (menor retardo).
- **Simplicidad:** Los routers solo necesitan conocer a sus vecinos directos, no requieren una imagen completa de la topología.
- **Redes inalámbricas:** En estas redes, un mensaje transmitido por una estación es recibido naturalmente por todas las demás en su alcance, lo que facilita este proceso.
## Enrutamiento por vector de distancia
