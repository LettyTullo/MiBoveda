## Piggybacking 
El **piggybacking** (o superposición) es una técnica utilizada en la transmisión bidireccional de datos que consiste en **adjuntar el acuse de recibo (ACK) de una trama recibida a la siguiente trama de datos que se envíe** en sentido contrario.
En lugar de enviar una trama de control dedicada exclusivamente para confirmar la recepción, el receptor espera un breve periodo de tiempo hasta que su propia capa de red le entregue un paquete para transmitir. En ese momento, inserta la confirmación en un campo específico del encabezado de la trama de datos saliente.
- **Funcionamiento:** El receptor demora temporalmente el envío del ACK con la esperanza de poder "engancharlo" a una trama de datos que viaje hacia el emisor original. Si no hay datos para enviar tras un periodo determinado, se termina enviando una trama de control ACK independiente para evitar que el emisor original agote su tiempo de espera y retransmita innecesariamente.
- **Ventajas:**
    - Mejor uso del ancho de banda
    - Menor carga de procesamiento
- **Desventajas y desafíos:**
    - **Gestión del tiempo:** Es difícil determinar exactamente cuánto tiempo debe esperar la capa de enlace por un paquete antes de enviar un ACK por separado. Si la espera es superior al temporizador del emisor, este retransmitirá la trama, invalidando el propósito de la técnica.
    - **Complejidad:** Requiere un esquema _ad hoc_ (como esperar un número fijo de milisegundos) para equilibrar la eficiencia y la prontitud de las confirmaciones.
# EJEMPLO: 
- **Piggybacking (Montarse a cuestas):** Enviar un paquete que _solamente_ diga "ACK" desperdicia el espacio de tu internet. Lo que hacen las computadoras es **retrasar** el ACK un poquito de tiempo, y lo "pegan" o "montan" dentro del próximo paquete de datos normal que tu amigo te iba a enviar de todas formas.
- Es como si recibes una carta y, en lugar de gastar en un sello postal solo para decir "me llegó", esperas a escribirle otra carta a esa persona y le agregas una nota al pie diciendo "por cierto, me llegó tu carta anterior". Esto mejora el uso de tu internet, aunque es difícil calcular cuánto tiempo exacto hay que esperar.
## ARQ (Automatic Repeat reQuest)
Es un mecanismo de control de errores utilizado para garantizar la entrega fiable de datos a través de canales de comunicación, especialmente en canales ruidosos donde las tramas pueden dañarse o perderse.
Consiste fundamentalmente en que el receptor confirma la llegada de los datos y el emisor retransmite la información si no recibe dicha confirmación en un tiempo determinado.
**Elementos fundamentales del ARQ**
Para que este sistema funcione correctamente, se basa en tres componentes clave:
1. **Acuses de recibo (ACK):** El receptor debe enviar una trama de control especial al emisor confirmando que ha recibido una trama correctamente.
2. **Temporizadores (Timers):** El emisor inicia un temporizador tras enviar cada trama. Si el temporizador expira antes de recibir el ACK, el emisor asume que la trama (o su confirmación) se perdió y procede a retransmitirla.
3. **Numeración de secuencias:** Tanto las tramas como los ACKs deben estar numerados. Esto permite al receptor distinguir si una trama entrante es una información nueva o una retransmisión de una trama que ya había aceptado previamente (lo cual ocurre si el ACK original se perdió en el camino)
# EJEMPLO
- **Concepto desde cero:** Los cables no son perfectos; hay interferencias y los paquetes a veces se pierden en el camino (se "caen").
- Para solucionar esto, existe el **ARQ (Reenvío Automático)**.
- _Ejemplo:_ Yo te envío el paquete número 1. Empiezo a contar en mi reloj (uso un "Timer"). Si el tiempo se agota y tú no me has enviado el "ACK" confirmando que llegó, yo asumo que se perdió y **te lo vuelvo a enviar**.
- Por esto es obligatorio **numerar los paquetes**, porque de lo contrario, si me retraso respondiendo, podrías enviarme el mismo paquete dos veces y yo pensaría que es un paquete nuevo.
## Ventanas Corredizas 
Las **ventanas corredizas** (o deslizantes) son un mecanismo de **control de flujo** basado en retroalimentación que permite que un emisor rápido no sature a un receptor lento, optimizando al mismo tiempo el uso del canal de comunicación.
Su funcionamiento se basa en:
# 1. Numeración de tramas
Para que el sistema funcione, cada trama transmitida debe incluir un **número de secuencia** en su encabezado. El rango de estos números suele ir de $0$ hasta un máximo de $2^n - 1$ (donde $n$ es el número de bits del campo de secuencia).
# 2. Definición de las ventanas
Tanto el emisor como el receptor mantienen una "ventana" que representa el conjunto de números de secuencia que tienen permitido procesar en un momento dado.
- **Ventana de emisión:** Es el grupo de números de secuencia que el emisor tiene permitido enviar sin esperar una confirmación.
- **Ventana de recepción:** Es el conjunto de números de trama que el receptor está preparado para aceptar.
# 3. El proceso de envío (Paso a paso)
1. **Carga de paquetes:** El emisor toma un paquete de la capa de red y lo encapsula en una trama, asignándole el número de secuencia disponible inmediatamente superior.
2. **Avance del extremo superior:** Al asignar un nuevo número, el **extremo superior** de la ventana del emisor avanza una posición.
3. **Almacenamiento en buffer:** El emisor debe guardar una copia de todas las tramas enviadas pero no confirmadas en un **buffer** por si fuera necesario retransmitirlas.
4. **Transmisión continua (Pipelining):** A diferencia de protocolos más simples, el emisor puede seguir enviando tramas mientras su ventana no esté llena, permitiendo que haya varias tramas "en vuelo" simultáneamente.
# 4. Recepción y Confirmación (ACK)
1. **Validación en el receptor:** Cuando llega una trama, el receptor verifica si su número de secuencia cae dentro de su ventana de recepción.
2. **Entrega a la capa superior:** Si la trama es la que se esperaba (el borde inferior de la ventana), se entrega a la capa de red y la ventana del receptor **gira** o avanza una posición.
3. **Envío del ACK:** El receptor envía un acuse de recibo (**ACK**) al emisor. Este puede enviarse como una trama de control separada o mediante **piggybacking** (superposición), insertando la confirmación dentro de una trama de datos que viaje en sentido contrario.
4. **Avance del extremo inferior (Emisor):** Cuando el emisor recibe el ACK, el **extremo inferior** de su ventana avanza, lo que libera espacio en su buffer y le permite enviar nuevas tramas.
## Fórmulas de Eficiencia de Utilización del Canal ($Ef$)

Las fórmulas para calcular la eficiencia de utilización del canal ($Ef$) varían dependiendo del protocolo de acceso al medio y del tipo de mecanismo de confirmación (ACK) utilizado.

> [!abstract] Concepto General
> La eficiencia ($Ef$) es la relación entre el tiempo que el canal está enviando datos útiles y el tiempo total del ciclo (transmisión + espera de confirmación).

# 1. Protocolos de Ventana Corredizas

> [!important] Fórmula General
> $$Ef = \frac{\text{Tiempo de transmisión de } W \text{ tramas}}{\text{Tiempo de llegada del primer ACK}}$$

> [!todo] Caso: ACK en Tramas de Control (Pequeñas)
> Se usa cuando la confirmación es un paquete mínimo dedicado.
> $$Ef = \frac{W \cdot T_t}{2T_p + T_t}$$


> [!todo] Caso: ACK en Tramas de Datos (Piggybacking) o Superposicion
> Se usa cuando el ACK viaja "montado" en un paquete de datos que viene de regreso.
> $$Ef = \frac{W \cdot T_t}{2T_p + 2T_t}$$


> [!info] Diccionario de Variables
> - **$W$**: Tamaño de la ventana.
> - **$T_t$**: Tiempo de transmisión ($L_t / V_t$). (Longitud de la trama (bits)/ tasa de bits(bits/seg))
> - **$T_p$**: Retardo de propagación. (Distancia(m)/ Velocidad de propagacion (m/s))
> - **$B$**: Tramas por segundo ($V_t / L_t$).
>

> [!tip] Condición para Eficiencia del 100%
> Para que el emisor nunca se detenga, la ventana debe ser:
> $$W \ge 2BT_p + 1$$

[[Tipos de Ventanas]]

---- 
# EJEMPLO (EXPLICACION DE IMAGENES)
![[Pasted image 20260422223545.png]]
- **Imagen de los cuadritos numéricos:** Aquí está la magia de la clase. Verás una fila de números del 1 al 10 y un recuadro azul que envuelve algunos números.
- **El Emisor:** Imagina que tienes 100 paquetes por enviar. En lugar de enviar el #1 y sentarte a esperar su ACK para enviar el #2 (lo cual sería lentísimo), usas una **ventana de emisión**.
    
    - Si tu ventana es de tamaño 8 (el recuadro envuelve del paquete 1 al 8), tú envías los paquetes del 1 al 8 de un solo golpe, sin esperar.
        
    - **El deslizamiento:** Cuando tu computadora recibe el "ACK" diciendo que el paquete #1 llegó bien, el recuadro _se desliza_ hacia la derecha. Ahora envuelve del paquete 2 al 9. Como el 9 acaba de entrar a la ventana, tu computadora lo envía inmediatamente. ¡Así la tubería nunca se queda vacía!
    

--- 

    



