 ## Tipos principales de protocolos

# 1. Parada y Espera (Stop-and-Wait)

> [!success] Característica Principal
> Es el más sencillo: el emisor envía una trama y se bloquea (no hace nada más) hasta que recibe la confirmación.

> [!info] Configuración Técnica
> - **Ventana de emisión ($W$):** $1$
> - **Ventana de recepción:** $1$
> - **Números de secuencia:** Basta con **1 bit** (0 y 1).
> - **Mecanismo:** Si el temporizador expira antes del ACK, se retransmite.

> [!tip] ¿Por qué se usa?
>  Es sencillo de implementar. Se utiliza en enlaces donde el producto ancho de banda-retardo es muy pequeño, como en **WiFi (802.11)**, donde tener una ventana mayor no mejoraría el rendimiento y añadiría complejidad innecesaria
# 2. Retroceso-N (Go-Back-N)

> [!warning] Característica Principal
> El emisor puede enviar varias tramas, pero el receptor es "estricto": **solo acepta tramas en orden**. Si una falla, se descartan todas las siguientes.

> [!important] Tamaños de Ventana
> - **Ventana de emisión ($W$):** $> 1$
> - **Ventana de recepción:** Siempre es $1$.
> - **Límite Máximo:** Si $k$ es el número de bits de secuencia:
>   $$W_{max} = 2^k - 1$$

> [!danger] El "Costo" del Error
> Si la trama $n$ falla, el emisor debe **retransmitir la trama $n$ y todas las posteriores** que ya había enviado, aunque estas hayan llegado bien originalmente.

> [!tip] ¿Por qué se usa?
>Es más eficiente que Parada y Espera en enlaces rápidos. Se prefiere cuando los errores son poco frecuentes, ya que evita la complejidad de almacenar tramas desordenadas en el receptor. El tamaño máximo de 2k−1 es crítico para evitar que el emisor confunda un ACK de una ventana vieja con uno de la ventana nueva si se pierden las confirmaciones

# 3. Repetición Selectiva (Selective Repeat)

> [!star] Característica Principal
> Es el más eficiente, pero mas complejo. El receptor tiene memoria (búfer) y **acepta tramas desordenadas**. Solo se retransmite exactamente lo que se perdió. Utiliza ACKs acumulativos y a menudo **NAKs** (acuse negativo) para acelerar la recuperación de errores

> [!example] Configuración Técnica
> - **Ventana de emisión ($W$):** $> 1$
> - **Ventana de recepción:** $> 1$
> - **Condición de ventana:** Para evitar ambigüedad entre tramas nuevas y viejas:
>   $$W \le \frac{MAX\_SEQ + 1}{2}$$ o tambien 
>  $$W_{max} = 2^{k - 1}$$
> 

 
> [!todo] ¿Por qué se usa?
> Se utiliza en enlaces con un gran **producto ancho de banda-retardo** (como enlaces satelitales o fibra transcontinental) y donde la tasa de error justifica la complejidad adicional de memoria en el receptor. La restricción del tamaño de ventana a la mitad del espacio de secuencia es indispensable para evitar el traslape de ventanas y la ambigüedad entre tramas nuevas y retransmisiones

# Cuadro Comparativo Rápido

| Protocolo            | Ventana Emisor | Ventana Receptor | Re-envío            | Complejidad |
| :------------------- | :------------: | :--------------: | :------------------ | :---------- |
| **Stop-and-Wait**    |       1        |        1         | Solo la actual      | Mínima      |
| **Go-Back-N**        |      $N$       |        1         | Todo desde el error | Media       |
| **Selective Repeat** |      $N$       |       $N$        | Solo la perdida     | Alta        |
## Protocolo PPP (Point-to-Point Protocol)

> El **PPP (Point-to-Point Protocol)**, definido en el **RFC 1661**, es el protocolo estándar de Internet utilizado para transportar paquetes a través de **enlaces punto a punto**. Se utiliza habitualmente en conexiones de routers, módems de cable y enlaces de banda ancha como **ADSL**.

# 1. Componentes principales de PPP
PPP ofrece tres características fundamentales para garantizar la comunicación:
 1. **Método de entramado:** Un mecanismo para **delimitar el inicio y el fin de cada trama, que además incluye **detección de errores**.
 
2. **LCP (Link Control Protocol):** Un protocolo de control de enlace diseñado para **activar las líneas, probarlas, negociar opciones y desactivarlas** cuando ya no se necesitan.

3. **NCP (Network Control Protocol):** Una familia de protocolos para negociar las opciones de la capa de red de forma independiente al protocolo utilizado (como **IPv4 o IPv6**). Existe un NCP diferente para cada capa de red soportada.
# 2. Formato de la trama PPP
![[Pasted image 20260521184129.png]]
La trama está orientada a bytes y su diseño se basa en el protocolo **HDLC**.

> [!success] Estructura de Campos
> - **Flag (Bandera):** Un solo byte (**0x7E** o 01111110) que indica el inicio y el fin de la trama.
> - **Address (Dirección):** Un byte con el valor fijo **11111111**, indicando que todas las estaciones deben aceptar la trama (evita asignar direcciones individuales).
> - **Control:** Un byte con el valor fijo **00000011**, que indica una trama no numerada (servicio sin conexión ni confirmación).
> - **Protocol (Protocolo):** Indica qué tipo de paquete viaja en la carga útil (p. ej., IP, IPv6, LCP o un NCP específico). Por defecto ocupa 2 bytes.
> - **Payload (Carga útil):** Datos de longitud variable.
> - **Checksum (Suma de comprobación):** Normalmente un **CRC de 2 o 4 bytes** para detectar errores de transmisión.
# 3. Mecanismo de Relleno de Bytes (Byte Stuffing)

> [!danger] ¡Importante: Evitar Confusión de Banderas!
> Para evitar que el byte de bandera (**0x7E**) aparezca accidentalmente dentro de los datos, PPP utiliza un **byte de escape (0x7D)**:
> 
> 1. Si el byte **0x7E** aparece en los datos, se inserta **0x7D** antes y se aplica una función **XOR con 0x20** al byte original (convirtiéndolo en **0x5E**).
> 2. Al recibir la trama, el receptor busca el byte de escape, lo elimina y aplica nuevamente XOR al siguiente byte para recuperar el dato original.

# 4. Diagrama de estados (Ciclo de vida del enlace)

> [!warning] Fases del Enlace
> 1. **DEAD:** El enlace físico no tiene conexión activa.
> 2. **ESTABLISH:** Los pares intercambian paquetes **LCP** para negociar opciones del enlace.
> 3. **AUTHENTICATE (Opcional):** Las partes verifican sus identidades.
> 4. **NETWORK:** Se utilizan los protocolos **NCP** para configurar la capa de red (como asignar IPs).
> 5. **OPEN:** Estado donde ocurre el **transporte de datos real** (paquetes IP encapsulados).
> 6. **TERMINATE:** Se cierra el enlace de forma lógica y vuelve al estado DEAD.

# 5. Aplicaciones prácticas

> [!example] Implementaciones Reales
> - **Packet over SONET:** PPP se utiliza sobre enlaces de fibra óptica SONET en redes troncales de ISPs para delimitar paquetes.
> - **ADSL:** En conexiones domésticas, se suele usar **PPPoA (PPP over ATM)**, donde la trama PPP se adapta para viajar sobre celdas ATM mediante la capa **AAL5**.
> - **DOCSIS:** En redes de televisión por cable, PPP también puede implementarse para el acceso a Internet.
,


[[La Sub-capa MAC parte 1]]