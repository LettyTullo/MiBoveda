 La subcapa MAC (Medium Access Control) es la parte inferior de la capa de enlace de datos y desempeña un papel fundamental, especialmente en las redes de área local (LAN), al gestionar cómo varios dispositivos comparten un mismo canal de comunicación.
A continuación, se explica detalladamente su funcionamiento, características y el protocolo ALOHA puro:
# 1. ¿Qué es y cómo funciona la subcapa MAC?
En una red con un canal de difusión (como WiFi o el Ethernet clásico), el problema central es decidir quién es el siguiente en transmitir cuando hay competencia por el medio. Si dos o más dispositivos transmiten al mismo tiempo, las señales interfieren y los datos se destruyen, lo que se conoce como colisión.
La subcapa MAC resuelve esto mediante protocolos de acceso múltiple que pueden ser de dos tipos:

> [!warning] Tipos de Asignación
> **Asignación Estática:** Divide el canal de forma fija (como FDM o TDM). Es ineficiente para el tráfico de datos moderno, que suele ser intermitente (en ráfagas), ya que el ancho de banda se desperdicia si un usuario no tiene nada que enviar.
> **Asignación Dinámica:** Asigna el canal bajo demanda mediante algoritmos. Puede ser centralizada (una entidad decide quién sigue) o descentralizada (cada máquina decide por sí misma siguiendo reglas establecidas).

# 2. Características y supuestos clave
El diseño de la subcapa MAC se basa en cinco supuestos fundamentales para la asignación dinámica:

> [!important] Supuestos Fundamentales
> - **Tráfico Independiente:** Las estaciones generan tramas de forma independiente siguiendo una distribución estadística (Poisson).
> - **Canal Único:** Solo hay un canal disponible para todas las comunicaciones y todas las estaciones pueden transmitir y recibir en él.
> - **Detección de Portadora (opcional):** Las estaciones pueden saber si el canal está ocupado antes de intentar transmitir.
> - **Colisiones Observables:** Las estaciones pueden detectar si su transmisión chocó con otra.
> - **Tiempo Continuo o Ranurado:** La transmisión puede empezar en cualquier momento (tiempo continuo) o solo al inicio de intervalos discretos (tiempo ranurado).

## Protocolos de acceso multiple 
# 1. Protocolo ALOHA Puro
Ideado en la Universidad de Hawái en los años 70, es el protocolo MAC más básico para sistemas de contención.

> [!info] Funcionamiento
> La regla es extremadamente simple: dejar que los usuarios transmitan siempre que tengan datos que enviar.
> Tras enviar una trama, el emisor escucha si hubo colisión (en el sistema original, esperaba una retransmisión del ordenador central).
> Si la trama se destruye por una colisión, el emisor espera un tiempo aleatorio antes de volver a intentarlo. Este tiempo debe ser aleatorio para evitar que las mismas tramas choquen una y otra vez indefinidamente.
# _Ejemplo:_
Imagina que estás en una cena a oscuras. No ves a nadie. Si quieres decir algo, simplemente gritas. Si escuchas que alguien más gritó al mismo tiempo que tú y no se entendió nada, te callas, esperas unos segundos y vuelves a gritar.

> [!danger] Periodo Vulnerable
> Es el intervalo de tiempo durante el cual una trama puede sufrir una colisión. Si $t$ es el tiempo necesario para transmitir una trama, el periodo vulnerable en ALOHA puro es de $2t$. Esto se debe a que cualquier trama que comience justo antes o durante la transmisión actual provocará un solapamiento y destruirá ambos mensajes.

> [!tip] Eficiencia
> Debido a la falta de coordinación y a que las tramas se envían en tiempos arbitrarios, su rendimiento es bajo:
> La fórmula de rendimiento es $S = G \cdot e^{-2G}$ (donde $G$ es la carga del canal).
> La utilización máxima del canal es de tan solo el 18.4% ($1/2e$). Esto significa que, en el mejor de los casos, el 82% del ancho de banda se pierde en colisiones o tiempo ocioso.
> **EL ALOHA ranurado:** Es **dos veces más eficiente**, alcanzando un rendimiento máximo de aproximadamente el **36.8%** (1/e). Su fórmula de rendimiento es: $S = G \cdot e^{-G}$ 

![[Pasted image 20260426214844.png]]

# 2. Protocolos CSMA (Carrier Sence Multiple Access)
Los protocolos CSMA son una evolución de ALOHA basados en la premisa: **"Si el canal está ocupado, la estación no debe transmitir"**.
#  Variaciones según la Persistencia

**CSMA 1-persistente**
 - **Funcionamiento:** La estación escucha el canal. Si está libre, transmite con probabilidad 1. Si está ocupado, espera escuchando continuamente hasta que el canal quede libre para transmitir de inmediato.     
 - **Debilidad:** Si varias estaciones están esperando, todas intentarán transmitir apenas se libere el canal, garantizando una colisión.
**CSMA No-persistente** 
- **Funcionamiento:** Si el canal está ocupado, la estación **no** espera continuamente. En su lugar, espera un tiempo aleatorio y vuelve a iniciar el algoritmo.
 - **Ventaja/Desventaja:** Reduce significativamente la probabilidad de colisiones, pero aumenta el retardo promedio de transmisión.

**CSMA p-persistente** 
- **Contexto:** Utilizado en canales ranurados.     
- **Funcionamiento:** Si el canal está libre, transmite con una probabilidad $p$. Con probabilidad $q = 1-p$, posterga la transmisión hasta la siguiente ranura.    
- **Proceso:** Se repite hasta que la trama es enviada o el medio es ocupado por otra estación.

#  CSMA/CD (Collision Detect)
Es la base de las redes Ethernet no conmutadas y establece condiciones específicas para garantizar que las colisiones sean detectadas correctamente por todas las estaciones involucradas

> [!danger] **Funcionamiento y Características**
> - **Detección:** La estación escucha el medio mientras transmite. Si la señal recibida difiere de la enviada, detecta una colisión e interrumpe la transmisión inmediatamente para ahorrar ancho de banda.
>     
> - **Tiempo de detección ($2\tau$):** En el peor escenario, el emisor tarda dos veces el tiempo máximo de propagación en detectar una colisión.
>     
> - **Trama mínima:** Se requiere una longitud mínima de trama para garantizar que el emisor siga transmitiendo mientras la ráfaga de colisión regresa.
>  $$L_{trama\_mínima} = V_t \cdot 2\tau$$
> 
>     _Donde $V_t$ es la velocidad de transmisión y $2\tau$ el retardo de ida y vuelta._
>     
> - **Backoff Exponencial Binario:** Tras una colisión, se espera un tiempo aleatorio definido por el intervalo $2^i - 1$, donde $i$ es el número de colisiones consecutivas. Tras 16 intentos fallidos, el sistema "dice que ocurrio un error".
>  $$\text{Espera} = \text{Número aleatorio en el rango } [0, 2^i - 1]$$
> 
> 	_Donde $i$ es el número de colisiones consecutivas experimentadas por la trama._ 

# 3. Protocolos Libres de Colisiones

Estos protocolos eliminan la competencia por el canal, evitando colisiones incluso bajo cargas de tráfico muy elevadas. Se basan en un sistema de **N estaciones con direcciones únicas (0 a N-1)**.

> [!abstract] **1. Protocolo de Mapa de Bits (Bitmap)**
> 
> - **Funcionamiento:** El periodo de contención se divide en **N ranuras**. Si una estación tiene datos, marca su ranura correspondiente con un "1".
>     
> - **Proceso:** Tras las N ranuras, todas las estaciones conocen quién debe transmitir y lo hacen en orden numérico ascendente.
>     
> - **Comportamiento:** Es muy eficiente cuando hay muchos datos para enviar, ya que el costo de la contención se diluye.
>     

> [!info] **2. Paso de Testigo (Token Ring)**
> 
> - **Funcionamiento:** Se utiliza un mensaje especial llamado **testigo (token)** que actúa como un permiso exclusivo.
>     
> - **Acción:**
>     
>     - Si la estación tiene datos, retiene el testigo, transmite su trama y luego libera el testigo hacia la siguiente estación.
>         
>     - Si no tiene datos, simplemente pasa el testigo inmediatamente.
>         
> - **Versatilidad:** Puede implementarse tanto en configuraciones de anillo físico como en buses lógicos.
>     

> [!warning] **3. Cuenta Regresiva Binaria (Binary Countdown)**
> 
> - **Funcionamiento:** Las estaciones que desean transmitir envían su dirección en binario bit a bit (comenzando por el más significativo).
>     
> - **Arbitraje:** El medio actúa como una **función OR booleana**. Si una estación envía un "0" pero detecta un "1" en el canal, significa que otra estación con una dirección mayor está compitiendo; por lo tanto, la primera se retira.
>     
> - **Resultado:** Al finalizar el proceso, la estación con la **dirección más alta** es la que gana el acceso al canal.
>     

# 4. Protocolos de contencion limitada
Su objetivo principal es proporcionar un **bajo retardo** cuando la carga de la red es baja (característica de la contención) y una **alta eficiencia** del canal cuando la carga es elevada (característica de los protocolos libres de colisiones).
# Protocolo de Árbol Adaptativo (Adaptive Tree-Walk)

Este es el modelo principal dentro de los protocolos de **contención limitada**. Su objetivo es identificar qué estaciones quieren transmitir mediante una búsqueda jerárquica, evitando que todo el conjunto de estaciones colisione simultáneamente.

> [!example] **Estructura Jerárquica**
> 
> - Las estaciones se organizan como las **hojas de un árbol binario**.
>     
> - El acceso se gestiona mediante una búsqueda en profundidad (_depth-first search_) o por niveles.
>     

> [!abstract] **Funcionamiento paso a paso**
> 
> - **Inicio:** Se parte de la raíz (nodo 1). Si solo una estación transmite, tiene éxito. Si hay silencio, la ranura se marca como ociosa.
>     
> - **Gestión de Colisiones:** Si ocurre una colisión, el protocolo desciende al sub-árbol izquierdo (nodo 2).
>     
> - **Búsqueda en profundidad:** Si hay otra colisión, se desciende un nivel más (nodo 4).
>     
>     - Si un nodo tiene éxito o queda inactivo, el algoritmo **asciende y pasa al hijo derecho** (nodo 3) para continuar la búsqueda.
>         

> [!tip] **Adaptabilidad a la Carga**
> 
> El protocolo ajusta su punto de partida según el tráfico para maximizar la eficiencia:
> 
> - **Baja carga:** Se comienza siempre desde la **raíz (nodo 1)**, permitiendo que una estación transmita con el menor retardo posible.
>     
> - **Alta carga:** Para evitar colisiones masivas, el protocolo inicia la búsqueda directamente en **niveles inferiores** del árbol, aislando grupos más pequeños de estaciones desde el inicio y reduciendo la contención.
>     



