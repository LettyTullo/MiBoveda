## Medios de transmision 
# a) Guiados
El concepto de **medio de transmisión guiado** se refiere a aquellos medios que utilizan un **cable o hilo físico** para transportar la información. El propósito fundamental de estos medios dentro de la capa física de una red es **transportar bits de una máquina a otra** de manera confiable.
Los **medios de transmisión guiada más comunes son los cables de cobre**, que se presentan en formas como el par trenzado y el cable coaxial, y la **fibra óptica**.
# **- Almacenamiento persistente:**
Aunque no es un "cable", el transporte físico de medios magnéticos o de estado sólido (cintas, CD, DVD o incluso camiones llenos de discos duros) es un método de transmisión física y guiada muy eficaz. Una de sus ventajas es que tiene un ancho de banda efectivo y su desventaja es el retardo
# **- Par Trenzado:** 
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