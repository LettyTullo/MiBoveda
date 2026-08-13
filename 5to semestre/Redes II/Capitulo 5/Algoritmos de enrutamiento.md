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
