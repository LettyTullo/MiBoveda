## Redes Ethernet
# 1. Ethernet Clasico
La Ethernet clásica funciona a una **tasa de bits de 10 Mbps** y utiliza la **codificación Manchester** para la señalización. 

>[!info] Implementaciones fisicas
>- **10Base5 (Ethernet gruesa):** Utilizaba un cable coaxial grueso amarillo ("manguera de jardín") que recorría el edificio; permitía segmentos de hasta 500 metros con 100 máquinas.
>- **10Base2 (Ethernet delgada):** Más barata y flexible, utilizaba conectores BNC y permitía tramos de 185 metros con 30 máquinas.
>- **10Base-T:** Utiliza cable de **par trenzado (UTP)** conectado a un dispositivo central llamado **hub (concentrador)**. Utiliza cable de par trenzado (_twisted pair_) de categoría 3 como mínimo. Su distancia máxima es de **100 metros** y se conecta habitualmente a un _hub_.
>- **10Base-F:** Utiliza fibra óptica y permite distancias de hasta **2000 metros**

>[!important] QUE ES EL HUB?
>Es un dispositivo que actúa como un repetidor de bits, quiere decir que, cualquier señal recibida en un puerto se retransmite a todos los demás, formando un único **dominio de colisión**.
>- Todos los puertos del HUB constituyen un "dominio de colision"
>- El hub es comparable con un bus de datos.
>
 
# Ethernet Clasico: Subcapa MAC y CSMA/CD
Para gestionar el acceso al medio compartido, la Ethernet clásica utiliza el protocolo **CSMA/CD 1-persistente:
1. **Detección de portadora:** La estación escucha el canal; si está libre, transmite inmediatamente. Si está ocupado, espera hasta que se libere.
2. **Detección de colisiones:** La estación sigue escuchando mientras transmite. Si la señal recibida difiere de la enviada, se ha producido una colisión.
3. **Señal de bloqueo (Jam Signal):** Al detectar la colisión, la estación interrumpe la transmisión y genera una ráfaga de ruido para avisar a los demás.
4. **Backoff Exponencial Binario:** Tras la colisión, la estación espera un tiempo aleatorio antes de reintentar. El intervalo de espera se duplica tras cada colisión consecutiva (hasta un máximo de 1023 ranuras), tirando la toalla tras 16 intentos fallidos. El algoritmo es injusto (no es "first in, first out")
## Formato de la trama Ethernet 
![[Pasted image 20260429114642.png]]

>[!danger] Campos que componen la trama: 
>- **Preámbulo (7 bytes):** Consiste en un patrón de bits (10101010) que permite al reloj del receptor sincronizarse con el del emisor.
> - **Delimitador de inicio de trama (SFD) (1 byte):** Contiene el patrón `10101011`, donde los dos últimos bits en "1" indican al receptor que el resto de la trama está por comenzar.
>- **Direcciones MAC de Destino y Origen (6 bytes cada una):** Identifican a las estaciones involucradas en el intercambio. Los **3 primeros bytes** de la dirección MAC corresponden al **OUI** (identificador único del fabricante) asignado por el IEEE, y los **3 últimos bytes** son el número de serie asignado por el fabricante. La dirección de difusión (**broadcast**) se representa con todos los bits en 1 (**FF:FF:FF:FF:FF:FF**).
>- **Tipo o Longitud (2 bytes):** Este campo tiene un significado dual dependiendo del valor:
>     - Si el valor es **menor o igual a 1536 (0x600)**, indica la **longitud** de los datos presentes en la carga útil (estándar 802.3).
>     - Si el valor es **mayor a 1536**, indica el **Tipo de protocolo** de capa de red al que pertenecen los datos (Ethertype), como IPv4 (0x0800).
>- **Datos (Carga útil):** Contiene el paquete proveniente de la capa de red. Tiene un límite **máximo de 1500 bytes**.
> - **Relleno (Padding):** Ethernet exige que las tramas válidas tengan una **longitud mínima de 64 bytes** (desde la dirección de destino hasta el CRC). Si los datos son menores a **46 bytes**, se utiliza este campo para completar el tamaño mínimo y asegurar que las colisiones sean detectadas correctamente.
>- **Suma de comprobación (CRC) (4 bytes):** Es un código de detección de errores de **32 bits** que permite al receptor determinar si los bits de la trama se recibieron correctamente; de lo contrario, la trama es descartada.

