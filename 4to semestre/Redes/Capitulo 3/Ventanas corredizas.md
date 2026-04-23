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

Elementos fundamentales del ARQ

Para que este sistema funcione correctamente, se basa en tres componentes clave:

1. **Acuses de recibo (ACK):** El receptor debe enviar una trama de control especial al emisor confirmando que ha recibido una trama correctamente.
2. **Temporizadores (Timers):** El emisor inicia un temporizador tras enviar cada trama. Si el temporizador expira antes de recibir el ACK, el emisor asume que la trama (o su confirmación) se perdió y procede a retransmitirla.
3. **Numeración de secuencias:** Tanto las tramas como los ACKs deben estar numerados. Esto permite al receptor distinguir si una trama entrante es una información nueva o una retransmisión de una trama que ya había aceptado previamente (lo cual ocurre si el ACK original se perdió en el camino)

### Página 5: Canales Ruidosos y Temporizadores (ARQ)

- **Concepto desde cero:** Los cables no son perfectos; hay interferencias y los paquetes a veces se pierden en el camino (se "caen").
    
- Para solucionar esto, existe el **ARQ (Reenvío Automático)**.
    
- _Ejemplo:_ Yo te envío el paquete número 1. Empiezo a contar en mi reloj (uso un "Timer"). Si el tiempo se agota y tú no me has enviado el "ACK" confirmando que llegó, yo asumo que se perdió y **te lo vuelvo a enviar**.
    
- Por esto es obligatorio **numerar los paquetes**, porque de lo contrario, si me retraso respondiendo, podrías enviarme el mismo paquete dos veces y yo pensaría que es un paquete nuevo.
    

---

### Página 6 y 7: Ventanas Corredizas (Explicación de las imágenes)

- **Imagen de la puerta de vidrio:** Es una metáfora literal de una "ventana corrediza".
    
- **Imagen de los cuadritos numéricos:** Aquí está la magia de la clase. Verás una fila de números del 1 al 10 y un recuadro azul que envuelve algunos números.
    
- **El Emisor:** Imagina que tienes 100 paquetes por enviar. En lugar de enviar el #1 y sentarte a esperar su ACK para enviar el #2 (lo cual sería lentísimo), usas una **ventana de emisión**.
    
    - Si tu ventana es de tamaño 8 (el recuadro envuelve del paquete 1 al 8), tú envías los paquetes del 1 al 8 de un solo golpe, sin esperar.
        
    - **El deslizamiento:** Cuando tu computadora recibe el "ACK" diciendo que el paquete #1 llegó bien, el recuadro _se desliza_ hacia la derecha. Ahora envuelve del paquete 2 al 9. Como el 9 acaba de entrar a la ventana, tu computadora lo envía inmediatamente. ¡Así la tubería nunca se queda vacía!
        

---

### Página 8: La Ventana del Receptor

- **El Receptor:** Quien recibe los archivos también tiene una "ventana" (una memoria o buffer). Solo va a aceptar y guardar paquetes que caigan dentro de esa ventana numérica. Si llega el paquete #20 pero el receptor estaba esperando el #2, simplemente lo tira a la basura porque llegó en desorden.
    

---

### Páginas 9 y 10: Eficiencia y Fórmulas

- **Concepto de "Canalización" (Pipelining):** Gracias a las ventanas, tu cable de internet actúa como una manguera de agua que siempre está llena a presión, usando eficientemente el 100% de la capacidad del cable.
    
- Aquí la diapositiva da una fórmula matemática que dice que el tamaño de la ventana ($W$) debe ser grande si tu cable es muy rápido pero muy largo geográficamente (como un cable submarino), para que los datos no se detengan nunca mientras esperas que la respuesta atraviese el océano.
    

---

### Resto del documento: Los 3 tipos de Ventanas y Protocolos reales

A partir de la página 11, la diapositiva explica tres estrategias de ventanas y luego cómo usamos esto hoy en día:

**Las tres formas de manejar las pérdidas de paquetes:**

1. **Stop and Wait (Parada y espera):** Tamaño de ventana de 1. Envío 1, espero. Envío 2, espero. Muy lento.
    
2. **Go-Back-N (Retroceder N):** Si enviaste los paquetes 1, 2, 3, 4 y 5 de golpe. El #3 se pierde. El receptor elimina el 4 y el 5 porque están desordenados. El emisor se da cuenta y tiene que re-enviar TODO empezando desde el 3 (o sea: 3, 4 y 5).
    
3. **Selective Repeat (Repetición Selectiva):** Es más inteligente. Si envías del 1 al 5 y el #3 se pierde, el receptor guarda el 4 y 5 temporalmente, y le dice al emisor: "Oye, _sólo_ te faltó el 3". El emisor reenvía únicamente el 3. Ahorra ancho de banda, pero es un proceso más complejo para la computadora.
    

**¿En qué protocolos reales se usa esto?**

- **Packet over SONET y PPP:** Son los protocolos que usan los cables gigantes de fibra óptica del internet mundial. Cuentan con un método especial donde insertan un símbolo (llamado _bandera_) para saber dónde empieza y dónde termina un archivo.
    
- **ADSL (Internet por línea de teléfono):** Rompe la información en cajitas súper pequeñas (celdas) de 53 bytes cada una.
    
- **DOCSIS (Internet por cable de TV):** Es el que proveen empresas de cable coaxial. El módem de tu casa se comunica con un aparato gigante del proveedor (llamado CMTS), y este último es como el director de tránsito, indicando qué canales usar y controlando la calidad.