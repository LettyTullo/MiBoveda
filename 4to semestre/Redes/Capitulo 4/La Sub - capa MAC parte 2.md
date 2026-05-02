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
>- **Preámbulo (7 bytes):** Consiste en un patrón de bits que permite al reloj del receptor sincronizarse con el del emisor.
> - **Delimitador de inicio de trama (SFD) (1 byte):** Contiene un patron, donde los dos últimos bits en "1" indican al receptor que el resto de la trama está por comenzar.
>- **Direcciones MAC de Destino y Origen (6 bytes cada una):** Identifican a las estaciones involucradas en el intercambio. Los **3 primeros bytes** de la dirección MAC corresponden al **OUI** asignado por el IEEE, y los **3 últimos bytes** son el número de serie asignado por el fabricante. La dirección de difusión es **broadcast**
>- **Tipo o Longitud (2 bytes):** Este campo tiene un significado dual dependiendo del valor:
>     - Si el valor es **menor o igual a 1536 (0x600)**, indica la **longitud** de los datos presentes en la carga útil (estándar 802.3).
>     - Si el valor es **mayor a 1536**, indica el **Tipo de protocolo** de capa de red al que pertenecen los datos (Ethertype), como IPv4 (0x0800).
>- **Datos (Carga útil):** Contiene el paquete proveniente de la capa de red. Tiene un límite **máximo de 1500 bytes**.
> - **Relleno (Padding):** Ethernet exige que las tramas válidas tengan una **longitud mínima de 64 bytes** (desde la dirección de destino hasta el CRC). Si los datos son menores a **46 bytes**, se utiliza este campo para completar el tamaño mínimo y asegurar que las colisiones sean detectadas correctamente.
>- **Suma de comprobación (CRC) (4 bytes):** Es un código de detección de errores de **32 bits** que permite al receptor determinar si los bits de la trama se recibieron correctamente; de lo contrario, la trama es descartada.

## Ethernet DIX 
**Ethernet DIX** es la norma de **10 Mbps** desarrollada conjuntamente por **Digital Equipment Corporation, Intel y Xerox** en 1978. Fue la base de lo que hoy conocemos como Ethernet comercial, evolucionando a partir de los diseños experimentales. El estándar DIX surgió de la necesidad de comercializar la tecnología Ethernet que Xerox había inventado pero no estaba interesada en explotar masivamente. En 1983, este estándar fue la base para la creación del **IEEE 802.3**, aunque inicialmente presentaban una diferencia clave en el formato de la trama que causó controversia en la industria durante años.
# Formato de la trama:
![[Screenshot 2026-05-02 161543.png]]

- **Preámbulo (8 bytes):** Un patrón de bits que genera una onda cuadrada para que el reloj del receptor se sincronice con el del emisor. El último byte termina en `11` para indicar el inicio de la trama
- **Dirección de Destino (6 bytes):** La dirección MAC del equipo que debe recibir la trama. Puede ser **Unicast** , **Multicast** o **Broadcast**
- **Dirección de Origen (6 bytes):** La dirección MAC única global del fabricante asignada por el IEEE. Los 3 primeros bytes corresponden al identificador del fabricante (**OUI**) y los 3 últimos al número de serie.
- **Tipo (Ethertype) (2 bytes):** Este es el campo distintivo de DIX. Indica al receptor a qué protocolo de la capa de red debe entregarse el paquete.
- **Datos (Carga útil):** El paquete proveniente de la capa superior. Su tamaño oscila entre **46 y 1500 bytes**.
- **Relleno (Pad):** Si los datos son menores a 46 bytes, se añade relleno para asegurar que la trama alcance el tamaño mínimo de **64 bytes** (necesario para que el mecanismo CSMA/CD detecte colisiones correctamente).
- **Suma de comprobación (Checksum / CRC) (4 bytes):** Un código de detección de errores (CRC de 32 bits) que permite al receptor descartar la trama si los bits se dañaron durante la transmisión.
## Direcciones Ethernet
# MAC ADREES
# 1. Formato y Estructura
Una dirección MAC estándar (como la de Ethernet) consta de **48 bits (6 bytes)**, generalmente
representados por 12 dígitos hexadecimales. 

>[!success] Su estructura se divide en dos partes principales:
>- **OUI (Organizationally Unique Identifier):** Corresponde a los **3 primeros bytes** y es un valor asignado por el IEEE para identificar de manera única al fabricante del hardware.
>- **Número de serie:** Son los **3 últimos bytes**, asignados por el propio fabricante para distinguir cada equipo individual.

# 2. Tipos de Direcciones MAC

>[!important] Existen tres categorías:
>- **Unicast:** Identifica a una sola interfaz de red, mensaje para una sola red. Se distingue porque el bit menos significativo del primer byte es **0**.
>- **Multicast (Multidifusión):** Permite que un grupo de estaciones escuchen una misma dirección. Se identifica porque el bit menos significativo del primer byte es **1**.
>- **Broadcast (Difusión):** Es una dirección especial compuesta por todos los bits en 1 (**FF:FF:FF:FF:FF:FF**), lo que obliga a que todas las estaciones de la red acepten y procesen la trama, mensajes para todos en la red.

[[Ethernet Conmutado]]
