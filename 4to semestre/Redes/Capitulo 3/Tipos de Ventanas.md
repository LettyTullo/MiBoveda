
### 5. Tipos principales de protocolos
Existen tres variantes comunes según cómo manejan los errores y el tamaño de las ventanas:

- **Stop-and-Wait (Parada y Espera):** El tamaño de la ventana es **1**. El emisor envía una trama y no puede enviar la siguiente hasta recibir el ACK.
- **Go-Back-N (Retroceso-N):** El receptor solo acepta tramas en orden estricto (su ventana de recepción es 1). Si una trama se pierde, el receptor descarta todas las posteriores y el emisor debe **retransmitir todo el grupo** de tramas no confirmadas a partir de la fallida.
- **Selective Repeat (Repetición Selectiva):** El receptor tiene una ventana mayor a 1 y puede aceptar tramas desordenadas, almacenándolas en un buffer hasta completar la secuencia. Si hay un error, solo se retransmite la trama perdida tras una confirmación negativa (**NAK**).