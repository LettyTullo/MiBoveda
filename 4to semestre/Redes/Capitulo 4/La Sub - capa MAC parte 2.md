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
Para gestionar el acceso al medio compartido, la Ethernet clásica utiliza el protocolo **CSMA/CD 1-persistente** (Acceso Múltiple con Detección de Portadora y Detección de Colisiones):

1. **Detección de portadora:** La estación escucha el canal; si está libre, transmite inmediatamente. Si está ocupado, espera hasta que se libere.
2. **Detección de colisiones:** La estación sigue escuchando mientras transmite. Si la señal recibida difiere de la enviada, se ha producido una colisión.
3. **Señal de bloqueo (Jam Signal):** Al detectar la colisión, la estación interrumpe la transmisión y genera una ráfaga de ruido para avisar a los demás.
4. **Backoff Exponencial Binario:** Tras la colisión, la estación espera un tiempo aleatorio antes de reintentar. El intervalo de espera se duplica tras cada colisión consecutiva (hasta un máximo de 1023 ranuras), tirando la toalla tras 16 intentos fallidos