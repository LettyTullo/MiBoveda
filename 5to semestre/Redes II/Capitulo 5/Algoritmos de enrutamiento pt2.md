## Enrutamiento por estado de enlace
El **enrutamiento por estado de enlace** es un algoritmo de enrutamiento dinámico que reemplazó al enrutamiento por vector de distancia en redes grandes debido a su convergencia más rápida y dinámica más simple. En este modelo, cada enrutador inunda la red con información sobre sus enlaces directos para que todos los nodos construyan un mapa completo y coincidente de la topología de la red.
Para que este algoritmo funcione, cada enrutador debe ejecutar los siguientes **cinco pasos**:

1. **Conocer a los vecinos:** Al arrancar, el router envía un paquete especial **HELLO** por sus enlaces punto a punto; los routers en el otro extremo responden con su identidad para establecer quiénes son sus vecinos directos.
2. **Fijar los costes de enlace:** Se establece una métrica de distancia o costo para cada vecino. Comúnmente, este costo es inversamente proporcional al ancho de banda (por ejemplo, Ethernet de 1 Gbps tiene costo 1 y 100 Mbps tiene costo 10) o se basa en el retardo medido con paquetes ECHO.
3. **Crear paquetes de estado de enlace (LSP):** El router construye un paquete que contiene su identidad, un número de secuencia, una "edad" y una lista de todos sus vecinos con sus respectivos costos.
![[Pasted image 20260815165750.png|408]]
>[!info] Notas
>- **Identidad (Letra superior):** Cada columna es el paquete generado por ese nodo (A, B, C, etc.).
 >- **Secuencia (`Sec.`):** Es el número de versión de la ficha. Si a un router le llega el paquete de A con secuencia 20 y luego recibe el 21, descarta el 20 por ser viejo.  
>- **Edad (`Edad`):** Un contador hacia atrás. Si llega a 0, la información se borra para no mantener datos viejos si un router se apaga.
>- **Lista de vecinos:** El paquete del **nodo B** dice únicamente: _"Mis vecinos directos son A (costo 4), C (costo 2) y F (costo 6)"_. El nodo B **no** opina sobre D ni E porque no son sus vecinos directos.

4. **Distribuir los paquetes LSP:** Los LSP se envían a todos los demás enrutadores mediante un proceso de **inundación confiable**. Para evitar saturar la red, se usan los números de secuencia (para no procesar duplicados) y el campo de "edad" (que disminuye con el tiempo para descartar información obsoleta).

![[Pasted image 20260815165831.png|368]]
>[!info] Notas 
>Esta es la tabla interna que guarda el **Router B** para administrar el reparto de las fichas sin saturar las líneas ni generar bucles. Como el Router B está conectado físicamente con **A, C y F**, usa banderas (0 o 1) para cada línea.
>Analicemos la **primera fila (Origen A)**:
>1. El Router B recibe por el cable **A** la ficha número 21 del router **A**.  
>2. **ACK flags (A=1, C=0, F=0):** Como la ficha vino por el cable **A**, B debe responder por la línea A diciendo _"Recibido"_ (ACK = 1). No envía confirmación a C ni a F porque ellos no le mandaron ese paquete.  
>3. **Send flags (A=0, C=1, F=1):** B debe avisarle al resto de la red. Pone **C=1** y **F=1** para reenviar la ficha de A hacia C y F. Pone **A=0** para no cometer la tontería de reexpedirle a A la misma ficha que A le acaba de entregar.
>

5. **Calcular las nuevas rutas:** Una vez que un enrutador ha acumulado un conjunto completo de paquetes LSP de toda la red, construye un grafo de la topología completa y ejecuta localmente el **algoritmo de Dijkstra** para hallar el camino más corto hacia cada destino posible.
# Características Principales

- **Uso en la actualidad:** Es la base de los protocolos de enrutamiento intradominio (IGP) más utilizados en Internet, como **OSPF** (Open Shortest Path First) e **IS-IS** (Intermediate System-to-Intermediate System).
- **Requisitos:** A diferencia del vector de distancia, el estado de enlace requiere **más memoria y potencia de cálculo**, ya que cada router debe almacenar la base de datos de toda la topología y procesar el grafo completo.
- **Ventajas:** No sufre del problema del "conteo al infinito" y converge rápidamente ante cambios en la red, lo que lo hace mucho más robusto para infraestructuras de gran escala.
## Enrutamiento Jerarquico
El **enrutamiento jerárquico** es una técnica utilizada para gestionar el crecimiento de las redes, permitiendo que las tablas de enrutamiento se mantengan en un tamaño manejable a medida que aumenta el número de enrutadores.

En este modelo, los enrutadores se dividen en unidades de agregación llamadas **regiones** o **áreas**:

- **Conocimiento local:** Cada enrutador conoce todos los detalles sobre cómo enrutar paquetes a destinos dentro de su propia región.
- **Conocimiento remoto:** El enrutador no sabe nada sobre la estructura interna de otras regiones. Para enviar tráfico a una región distinta, simplemente sabe qué línea de salida utilizar para alcanzar dicha región de manera general.
# Niveles de jerarquía

En redes extremadamente grandes, dos niveles (red local y regiones) pueden ser insuficientes. En esos casos, se pueden agrupar las regiones en **clústeres**, los clústeres en **zonas**, y así sucesivamente. Según las investigaciones citadas en los textos, para una red de \(N\) enrutadores, el número óptimo de niveles es \(\ln N\), lo que requiere un total de \(e \ln N\) entradas en la tabla de cada enrutador.
# Ventajas y Desventajas
El uso de la jerarquía implica un intercambio de beneficios técnicos:
- **Ventajas:**
    - **Reducción del tamaño de las tablas:** Ahorra memoria en los enrutadores y ancho de banda al enviar informes de estado.
    - **Aislamiento:** Evita que los cambios en la topología de una región obliguen a todos los enrutadores de la red a recalcular sus rutas.
    - **Eficiencia de CPU:** Requiere menos tiempo de procesamiento para escanear las tablas.
- **Desventajas:**
    - **Caminos subóptimos:** Puede resultar en rutas ligeramente más largas en comparación con el "enrutamiento plano" (donde cada router conoce toda la topología), aunque esta penalización suele ser pequeña y aceptable.

**Ejemplo comparativo:** En una red de **720 nodos** sin jerarquía, cada enrutador necesita **720 entradas**. Si se divide en 24 regiones de 30 enrutadores, cada uno solo necesita **53 entradas** (30 locales + 23 remotas). Con tres niveles (clústeres, regiones y nodos), el número de entradas podría reducirse a tan solo **25**.