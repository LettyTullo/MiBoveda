
##  Tipos principales de protocolos

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

## 3. Repetición Selectiva (Selective Repeat)

> [!star] Característica Principal
> Es el más eficiente, pero mas complejo. El receptor tiene memoria (búfer) y **acepta tramas desordenadas**. Solo se retransmite exactamente lo que se perdió. Utiliza ACKs acumulativos y a menudo **NAKs** (acuse negativo) para acelerar la recuperación de errores

> [!example] Configuración Técnica
> - **Ventana de emisión ($W$):** $> 1$
> - **Ventana de recepción:** $> 1$
> - **Condición de ventana:** Para evitar ambigüedad entre tramas nuevas y viejas:
>   $$W \le \frac{MAX\_SEQ + 1}{2}$$ o tambien 
>  $$W_{max} = 2^k - 1$$
> 

 2 a la k menos 1 hina es
 
> [!todo] ¿Por qué se usa?
> Se utiliza en enlaces con un gran **producto ancho de banda-retardo** (como enlaces satelitales o fibra transcontinental) y donde la tasa de error justifica la complejidad adicional de memoria en el receptor. La restricción del tamaño de ventana a la mitad del espacio de secuencia es indispensable para evitar el traslape de ventanas y la ambigüedad entre tramas nuevas y retransmisiones

# Cuadro Comparativo Rápido

| Protocolo            | Ventana Emisor | Ventana Receptor | Re-envío            | Complejidad |
| :------------------- | :------------: | :--------------: | :------------------ | :---------- |
| **Stop-and-Wait**    |       1        |        1         | Solo la actual      | Mínima      |
| **Go-Back-N**        |      $N$       |        1         | Todo desde el error | Media       |
| **Selective Repeat** |      $N$       |       $N$        | Solo la perdida     | Alta        |