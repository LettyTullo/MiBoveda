## Medios de transmision 
## a) Guiados
El concepto de **medio de transmisión guiado** se refiere a aquellos medios que utilizan un **cable o hilo físico** para transportar la información. El propósito fundamental de estos medios dentro de la capa física de una red es **transportar bits de una máquina a otra** de manera confiable.
Los **medios de transmisión guiada más comunes son los cables de cobre**, que se presentan en formas como el par trenzado y el cable coaxial, y la **fibra óptica**.
# - Almacenamiento persistente:
Aunque no es un "cable", el transporte físico de medios magnéticos o de estado sólido (cintas, CD, DVD o incluso camiones llenos de discos duros) es un método de transmisión física y guiada muy eficaz. Una de sus ventajas es que tiene un ancho de banda efectivo y su desventaja es el retardo
# - Par Trenzado:
Un cable de par trenzado consiste en dos hilos de cobre aislados,
- **UTP (Unshielded Twisted Pair):** Es el cable de par trenzado sin apantallar. Son cuatro pares de hilos de colores dentro de una funda gris simple. Es el más económico, fácil de instalar y el más utilizado en redes LAN Ethernet y telefonía, pero es el más susceptible a la interferencia externa.
- **FTP (Foiled Twisted Pair):** Cuenta con una hoja o lámina metálica que rodea los cuatro pares trenzados en su conjunto, brindando una protección intermedia.
- **STP (Shielded Twisted Pair):** Incorpora una malla metálica o vaina que envuelve los pares, reduciendo drásticamente la interferencia. Su desventaja es que son más costosos, gruesos, pesados y difíciles de manejar.
- **SFTP:** Combina técnicas de blindaje, añadiendo protección tanto a cada par individual como al conjunto completo del cable.
A nivel de rendimiento, el par trenzado ha evolucionado en diferentes **categorías**:

- **Categoría 3 (Clase C):** Alcanza un ancho de banda de 16 MHz y es de tipo UTP. 
- **Categoría 5 y 5e (Clase D):** Ofrecen un ancho de banda de 100 MHz. Para Ethernet a 100 Mbps, se utilizan solo dos de los cuatro pares (uno en cada dirección), mientras que para velocidades de 1 Gbps se utilizan los cuatro pares de forma simultánea.
- **Categoría 6 (Clase E):** Soporta hasta 200 MHz de ancho de banda y posee un mayor número de torsiones por metro para reducir la diafonía (crosstalk), permitiendo conexiones de hasta 10 Gbps.
- **Categoría 7 (Clase F):** Llega a 600 MHz y utiliza configuraciones SFTP (pares trenzados con blindaje individual y global), limitando significativamente la interferencia con cables cercanos.
- **Categoría 8:** Es un cableado nuevo con un ancho de banda de 2 GHz. Está diseñado para redes Ethernet ultrarrápidas de 25 y 40 Gbps, pero su limitación es que solo soporta estas velocidades a distancias cortas (hasta 30 metros), por lo que se destina principalmente a centros de datos.
Para realizar las terminaciones de estos cables, se utilizan los conectores **RJ-45**. La asignación de los hilos de colores a los pines está regida por los estándares **EIA/TIA T568A y T568B**. En la actualidad, la norma T568A está prácticamente en desuso y ha sido reemplazada por la T568B.
Se pueden crear cables **directos (straight-through)** si se usa el mismo estándar en ambos extremos, o cables **cruzados (crossover)** si se mezclan asignaciones para cruzar las vías de transmisión y recepción.
# - Cable Coaxial
- Ofrece un **mejor apantallamiento y mayor ancho de banda** que el par trenzado, permitiendo distancias más largas a altas velocidades.
- Su rendimiento está limitado por la atenuación y el ruido.
- Se utiliza tanto para señales analógicas (con amplificadores cada pocos km) como digitales (con repetidoras cada 1 km).
- Existe una variante denominada **cable twinaxial**. (usados en ethernet de 10GB)
- **Construcción:** Tiene un núcleo de cobre rígido rodeado por un aislante, un conductor cilíndrico (malla trenzada) y una funda protectora
# - Líneas Eléctricas (PLC - Power Line Communications)
Esta tecnología aprovecha el **cableado eléctrico existente** en los hogares para transmitir datos.
- **Uso:** Es muy conveniente porque permite conectar dispositivos a Internet simplemente enchufándolos a la toma de corriente.
- **Desventaja:** El cableado eléctrico es un entorno "hostil" para los datos, ya que genera mucho ruido y las propiedades eléctricas cambian constantemente al encender o apagar electrodomésticos
# -  Fibra Óptica
Es el medio con mayor capacidad tecnológica actual, capaz de transportar cientos de Gbps a distancias de decenas de kilómetros sin amplificación.
- **Funcionamiento:** Basado en la reflexión total interna de la luz (generada por LED o Láser) dentro del cable.
- **Modos de transmisión:**
	- **Monomodo:** El núcleo es tan delgado que la luz viaja en línea recta. Es más cara pero ideal para **largas distancias**.
	- **Multimodo:** Permite que varios rayos de luz reboten en distintos ángulos dentro del núcleo. Es más económica y se usa para **distancias cortas** (hasta 15 km)
	- **Clasificación por perfil de refracción:**
		Existen dos tipos fundamentales basados en cómo viaja la luz a través del núcleo:
	   - **Índice escalonado (Step index):** En este tipo, los rayos de luz rebotan en las paredes del núcleo en diferentes ángulos.
	  - **Índice gradual (Graded index):** En lugar de rebotes bruscos, los rayos siguen trayectorias curvas debido a una variación gradual en el índice de refracción del núcleo.
- **Beneficios:** Ofrece un ancho de banda enorme (Terahertz), tasas de datos de cientos de Gbps, baja atenuación, peso ligero e inmunidad a interferencias electromagnéticas.
- **Desventajas:** Requiere conocimientos específicos para su instalación, los cables son frágiles, el costo es mayor que el cobre y la comunicación suele ser unidireccional por hilo.
- **Usos comunes:** Redes de "backbone" de larga distancia, acceso a Internet de alta velocidad (FTTH) y redes LAN.
# Componentes de Infraestructura Relacionados
- **Racks o Gabinetes:** Estructuras para alojar equipos de red, con anchos estándar de 19 pulgadas y alturas medidas en Unidades "**U**" (1U = 4,445 cm).
- **Patch Panels:** Elementos pasivos donde termina el cableado físico de la instalación.
- **Patch Cords:** Cables cortos armados de fábrica para conectar equipos dentro del rack.
## b) No Guiados
Son aquellos que transportan datos sin un medio fisico, sino que se propagan por las ondas a traves del aire, el vacio o el agua. 
# - El espectro electromagnetico:
Se propaga mediante las ondas para transportar informacion, dependiendo de la potencia y el ancho de banda.
**El Espectro Electromagnético: Bases Teóricas**
- **Naturaleza de la señal:** El movimiento de los electrones genera ondas electromagnéticas que pueden propagarse por el espacio.
- **Propiedades fundamentales:** Las ondas se definen por su **frecuencia** (f), medida en Hertz (Hz); su **longitud de onda** (λ), que es la distancia entre dos máximos; y su **fase** (ϕ).
- **Relación matemática:** En el vacío, todas las ondas viajan a la velocidad de la luz (c≈3×108 m/seg), cumpliendo la fórmula c=λ⋅f.
- **Capacidad de datos:** La cantidad de información que puede transportar una señal es proporcional a su **ancho de banda**
# **Tipos de medios y usos del espectro** 
# 1) **Radio frecuencias de (3KHZ a 2GHZ):** 
Son faciles de generar, pueden recorrer largas distancias y **penetran edificios con facilidad**.
	- Son **omnidireccionales**, lo que significa que viajan en todas las direcciones y no requieren alineación precisa entre antenas.
	- Su propagación varía según la banda: las de baja frecuencia siguen la curvatura de la Tierra (**ondas superficiales**), mientras que las de alta frecuencia rebotan en la ionosfera (**ondas aéreas**)
# TIPOS DE PROPAGACION
# Propagación superficial (Ground-wave)
Este tipo de propagación es característico de las bandas de baja frecuencia: **VLF (muy baja frecuencia), LF (baja frecuencia) y MF (frecuencia media)**.
	- En este modo, las ondas de radio **siguen la curvatura de la Tierra**.
	- Pueden detectarse a distancias de unos **1,000 km** en las frecuencias más bajas, aunque este alcance disminuye a medida que aumenta la frecuencia.
	- Un ejemplo común de su uso es la **radiodifusión AM**
# Propagación aérea (Sky-wave)
Se asocia específicamente con la banda **HF (alta frecuencia)**.
- Las ondas terrestres en estas bandas tienden a ser absorbidas por la Tierra, pero las señales que se dirigen hacia arriba llegan a la **ionosfera** (una capa de partículas cargadas entre 100 y 500 km de altura)
-  La ionosfera **refracta estas ondas** y las envía de vuelta a la Tierra, permitiendo comunicaciones a larga distancia.
-  Dependiendo de las condiciones atmosféricas, la señal puede rebotar varias veces entre la atmósfera y el suelo
# Propagación por línea de vista (Line-of-sight)
Es el método predominante para las bandas **VHF (frecuencia muy alta) y UHF (frecuencia ultra alta)**, así como para las **microondas**.
- A frecuencias superiores a 100 MHz, las ondas viajan prácticamente en **línea recta** y pueden enfocarse de forma estrecha.
- Requiere que el emisor y el receptor estén alineados físicamente sin obstáculos importantes.
- Debido a que viajan en línea recta, si las antenas están muy separadas, la **curvatura de la Tierra** se interpone en el camino, lo que obliga a utilizar **repetidores** periódicos para mantener la comunicación a larga distancia
# 2) **Microondas de (2GHZ a 300GHZ):** 
Viajan en línea recta y son altamente direccionales, permitiendo enfocar la energía en haces estrechos mediante antenas parabólicas.
	- Requieren **línea de vista** despejada entre el emisor y el receptor.
	- Se utilizan para telecomunicaciones de larga distancia, satélites, telefonía celular, Bluetooth y WiFi.
	- Su principal desventaja es que son absorbidas por el agua, lo que causa atenuación durante la lluvia
# 3) **Luz Infrarroja (300 GHz a 400 THz):** 
Se utiliza para comunicaciones de corto alcance, como mandos a distancia e interconexión de laptops (estándar IrDA).
	- **No atraviesa objetos sólidos**, lo que proporciona seguridad contra interferencias entre habitaciones adyacentes y no requiere licencia gubernamental para su uso
	- Es usada en controles de TV, aire y ampliamente usadas en fibra optica 
# 4)  **Ondas de Luz Visible:**
- Incluye la tecnología **Li-Fi**, donde los LEDs parpadean en periodos de nanosegundos (imperceptibles al ojo humano) para transmitir datos.
- También se utiliza el láser aéreo para conectar redes entre edificios cercanos, aunque puede verse afectado por corrientes de convección térmica.
## Espectro expandido
Es un método de codificación fundamental para las comunicaciones inalámbricas en el que la señal de datos se expande a través de un ancho de banda mucho mayor que el rango de frecuencias mínimo necesario para transmitir la información.
Este metodo ofrece inmunidad al ruido y a la distorsion de trayectoria multple (multipath) , esconder y encriptar señales, varios usuarios pueden compartir el mismo ancho de banda sin muchas interferencias.
# Espectro Expandido por Salto de Frecuencia (FHSS)
En el FHSS (_Frequency-Hopping Spread Spectrum_), el transmisor **salta de una frecuencia a otra** cientos de veces por segundo siguiendo una serie seudoaleatoria.
- **Funcionamiento:** El receptor debe estar en perfecto sincronismo con el transmisor para saltar entre las mismas frecuencias y reconstruir la señal.
- **Tipos:** Se clasifica en **Slow FHSS** (donde el tiempo del salto es mayor o igual a la duración del símbolo) y **Fast FHSS** (donde el tiempo del salto es menor que la duración del símbolo), siendo este último el que ofrece mejor rendimiento.
- **Uso:** Fue inventado por Hedy Lamarr en 1942 para fines militares y hoy se utiliza comercialmente en tecnologías como **Bluetooth**
# Espectro Expandido de Secuencia Directa (DSSS)
El DSSS (_Direct Sequence Spread Spectrum_) utiliza una secuencia de códigos para distribuir la señal de datos por una banda de frecuencias más amplia.
- **Funcionamiento:** Cada bit de datos se representa mediante múltiples bits denominados **chips** usando un código de expansión. Por ejemplo, el estándar 802.11b utiliza una secuencia específica llamada **secuencia de Barker**.
- **Relación con CDMA:** Esta técnica permite que varias señales compartan la misma banda asignando códigos diferentes a cada una, lo que constituye la base del **Acceso Múltiple por División de Código (CDMA)** utilizado en redes 3G y GPS.
# Comunicación de Banda Ultra Ancha (UWB)
Aunque a veces se clasifica por separado, las fuentes la incluyen como una técnica que utiliza bandas de frecuencia muy anchas.
- **Funcionamiento:** Envía una serie de **impulsos rápidos de baja energía** que varían sus frecuencias. Se define por tener un ancho de banda de al menos 500 MHz o el 20% de su frecuencia central. (la norma IEE 802.15.4a)
- **Ventajas:** Tolera interferencias fuertes de otras señales de banda estrecha y, debido a su baja energía por frecuencia, no causa interferencias significativas a otros sistemas.
- **Uso:** Es ideal para aplicaciones de corto alcance en interiores, radares de precisión y sistemas de localización.

Estas técnicas son vitales para operar en las **bandas ISM** (Industrial, Científica y Médica), que son bandas de uso libre sin licencia (como la de 2.4 GHz) donde muchos dispositivos deben convivir y manejar la interferencia mutua.

# Bandas ISM (Industrial, Scientific, and Medical)
Su nombre corresponde a las siglas en inglés para **Industrial, Científica y Médica**.
- **Propósito y Regulación:** Fueron definidas originalmente por la UIT-R para aplicaciones no relacionadas con las comunicaciones, pero hoy son la base de las redes LAN e inalámbricas domésticas. 
- **Frecuencias Principales:**
    - **900 MHz (902-928 MHz):** Utilizada por versiones antiguas de WiFi; actualmente está muy saturada.
    - **2.4 GHz (2.4-2.4835 GHz):** Es la banda más común, utilizada ampliamente por **WiFi (802.11b/g/n)**, **Bluetooth** y Zigbee.
    - **5.8 GHz (5.725-5.825 GHz):** Proporciona más ancho de banda y es utilizada por estándares WiFi más modernos
# Bandas U-NII (Unlicensed National Information Infrastructure)
Las siglas corresponden a **Infraestructura Nacional de Información sin Licencia**.
- **Ubicación y Uso:** Se encuentran en la porción de **5 GHz** del espectro. Aunque inicialmente estaban poco desarrolladas, se han vuelto masivas gracias a estándares de alta velocidad como **802.11ac y 802.11ax (WiFi 6)**
- **Ventajas:** Ofrecen un **mayor ancho de banda** que las bandas de 2.4 GHz. Sin embargo, al operar en frecuencias más altas, sus señales tienen una menor capacidad para atravesar paredes y obstáculos sólidos

[[Capa fisica Parte 2]]