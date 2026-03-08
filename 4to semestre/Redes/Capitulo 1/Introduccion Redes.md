[[Protocolo de Red]]
## Concepto de Redes:
Conjunto de dispositivos autonomos interconectados para intercambiar informacion a traves de medios de transmision.
## Uso de redes informaticas
# 1. **Acceso a la informacion:** 
Para acceder a la gran parte de la informacion de Internet se utiliza el modelo de cliente-servidor en donde el cliente solicita informacion a un servidor que aloja dicha informacion. El modelo cliente-servidor es aplicable no solo cuando se encuentran en el mismo edificio (y pertenecen a la misma empresa), sino tambien cuando estan alejados.
Otro modelo utilizado es p2p (peer-to-peer) en el que no existe un servidor que contiene la informacion sino que hay un grupo informal de personas que se comunican y se transfieren informacion entre si. 

# 2. **Comunicación de persona a persona:**  
Se encuentran en las redes sociales. De otra forma, es una web donde un grupo de personas pueden colaborar para crear contenidos y editar. Un wiki seria una explicacion de eso. 

# 3. **Comercio electronico:**
Un ejemplo de esto podria ser las compras en linea, a diferencia del comercio electronico tradicional que sigue el modelo cliente-servidor, las subastas en linea son de igual a igual, en el sentido que los consumidores pueden actuar como compradores y hay un servidor central que contiene toda la informacion.

# 4. **Entretenimiento:** 
IPTV(Internet Protocol Television) tecnologia que transmite señales de TV, series y peliculas a traves de internet. Pueden moverse a traves de la casa entre distintos dispositivos normalmente a traves de una red inalambrica.

# 5. **Internet de los objetos (Iot):**
Los dispositivos electronicos de consumo estan conectados a la red. Por ej, las camaras de gama alta,etc
## Tipos de redes informaticas

# - **Redes de banda ancha:** 
Infraestructuras de telecomunicaciones de alta velocidad conectan a los usuarios finales a la red central del proveedor de servicios. En muchas partes del mundo el acceso de banda ancha llega a los hogares por fibra optica, cable coaxial,etc.

# - **Redes de acceso movil e inalambricas:**  
Un ejemplo de las redes inalambricas son las redes telefonicas. Los puntos de acceso inalambricos basados en la norma de 802.11(Wifi), GPS,redes de sensores, NFC (Near Field Communicacion) el movil puede interactuar como tarjeta inteligente de RFID (Identificacion por radiofrecuencia) e interactuar con un lector cercano para pagar.

# - **Redes de acceso:**
Muchos servicios de internet se conectan desde "la nube" o redes de centros de datos. Los primeros diseños de redes de centro de datos se basaban en una topologia de arbol simple con tres capas: acceso, agredado, nucleo. Las redes proveedoras de contenidos utilizan servicios de Internet como CDN (Red de entrega de contenidos) un gran conjunto de servidores que estan geograficamente distribuidos de forma que los contenidos se situen lo mas cerca posible de los usuarios.

# - **Redes de transito:** 
Cuando el proveedor de contenidos y su ISP(proveedor de servicios de internet) no estan conectados directamente se utiliza una red de transito para transportar el trafico entre ellos. Las redes de transito se le llama redes troncales porque su funcion ha sido transportar el trafico entre dos puntos finales.

# - **Redes de empresa:** 
Redes usadas en campus, edificios de oficinas y otras organizaciones, los datos se almacenan en servidores, utilizan VPN (redes privadas virtuales) conectan las redes individuales a distintos sitios a una sola red logica. El escritorio compartido permite a los trabajadores remotos ver e interactuar con una pantalla grafica de ordenador.

## Tecnologia de redes
# 1. Redes de area personal (PAN): 
Las PAN permiten que los dispositivos se comuniquen al rededor  de una persona. Puede utilizarse para conectar auriculares a un telefono movil sin cable, Bluetooth, las redes bluetooth utilizan el paradigma maestro-esclavo.
# 2. Redes de area local (LAN):
LAN es una red privada que funciona en una unica casa o edificio. Es estos sistemas cada ordenador tiene un radiomodem y una antena que utiliza para comunicarse con otros ordenadores, tambien se denominan Access Point(AP), router inalambrico o estacion base. El estandar popular de las LAN inalambricas es el IEEE 802.11 (WIFI). Los modos fisicos de transmision mas comunes son: cable de cobre, cable coaxial y fibra optica, IEEE 802.3 es el Ethernet y es un tipo de LAN cableada. Las LAN cableadas tienen menor latencia, menor perdida de paquetes y mayor rendimiento que las inalambricas.
# 3. Redes domesticas:
Son un tipo de LAN, pueden tener una amplia gama de dispositivos interconectados en la red, deben ser faciles de instalar y mantener. Las redes de linea electrica tambien permiten que los aparatos se conecten a las tomas y difundan informacion por toda la casa.

# 4. Redes de area metropolitana (MAN):
Cubre una ciudad completa. Un ejemplo mas conocido son las redes de television por cable, ademas del cable, otra tecnologia asociada es la WiMaX (estandar del IEEE 802.16)

# 5. Redes de area extensa(WAN):
Abarca una gran zona geografica como un pais, continente o incluso varios continentes. Suelen pertenenecer a proveedores de servicios (como ISP). Tiene dos componentes fundamentales:
1. **Hosts:** Son las máquinas (como computadoras de escritorio o servidores) donde se ejecutan las aplicaciones de usuario.
2. **Subred de comunicación:** Es el conjunto de infraestructuras que conectan los hosts. Esta subred consta de **líneas de transmisión** (cables de cobre, fibra óptica o radioenlaces) y **elementos de conmutación**, conocidos comúnmente como **routers**.
**Tipos de WAN:**
- **Redes de ISP:** La propia infraestructura de un proveedor de servicios de Internet es una WAN.
- **Redes Privadas Virtuales (VPN):** Son WANs construidas con enlaces virtuales que funcionan sobre la Internet pública.
- **SD-WAN:** Son redes definidas por software que permiten optimizar el rendimiento y coste de conectividad entre sitios geograficamente distribuidos
- **WANs Inalámbricas:** Incluyen los sistemas de comunicación por **satélite** y las redes de **telefonía móvil** (desde 1G hasta 5G).

## Internet
Conjunto de redes interconectadas. Conecta entre sí, a proveedoras de contenido,redes de acceso, redes empresariales, redes domesticas y otras. La red combina sub-redes y host. Una sub-red puede ser (ISP). El internet tambien puede ser descripto como una WAN. Puede ser entendida como una interconeccion de redes independientes, conectando una LAN y una WAN o conectando dos LANs.
El dispositivo que establece conexion entre dos o mas redes se le denomina "gateway"
## Ejemplos de redes
# 1-  Internet:
Es una  vasta coleccion de redes que utilizan ciertos protocolos comunes y prestan ciertos servicios
# - ARPANET: 
Fue la precursora de la Internet, demostro que la conmutacion de paquetes era viable y robusta, sentando las bases tecnicas (como TCP/IP y el concepto de red distribuida) que permiten el funcioanmiento de la red global actual.
# - NSTNET:
Fue la red que sucedio a la ARPANET y funciono como la red troncal (backbone) de la primera internet entre (1985-1995) fue el puente fundamental que transformó una red de investigacion academica en la infraestructura global,comercial y competitiva que hoy conocemos como internet
# La arquitectura del internet
La arquitectura continua evolucionando. Pero los componentes fundamentales ya actuales son:
- Centros de datos (datacenters): La mayoria del trafico se origina en datacenters masivos que conectan miles o millones de servidores
- ISP Y POP: Los usuarios se unen a la red a traves de un proveedor de servicios de internet (ISP), conectandose fisicamente a una ubicacion llamada punto de presencia (POP)
- Routers: Son los dispositivos que conmutan paquetes en la capa de red, utilizando tablas de enrutamiento para encontrar el camino mas eficiente entre redes.
- SDN (Redes definidas por software): Una tendencia moderna en la arquitectura es la separación del **plano de control** (donde se toman las decisiones de enrutamiento) y el **plano de datos** (donde se mueven los paquetes), permitiendo que la red sea programable mediante software 
# 2. Redes moviles - arquitectura:
En lugar de utilizar un único transmisor de gran potencia, las redes móviles dividen una región geográfica en **celdas**.
- **Estaciones Base (BS):** Cada celda tiene una estación base (en 4G llamada **eNodeB**) que se comunica por radio con los dispositivos móviles dentro de su alcance.
- **Reutilización de frecuencias:** El diseño permite que celdas no adyacentes utilicen las mismas frecuencias, incrementando drásticamente la capacidad de la red.
- **Handover (Traspaso):** Cuando un usuario se desplaza de una celda a otra, el flujo de datos se re-rutea automáticamente a la nueva estación base para mantener la continuidad
# Evolución de la Arquitectura (1G a 5G)
- **1G (AMPS):** Redes analógicas diseñadas exclusivamente para voz.
- **2G (GSM):** Introdujo la transmisión digital, los mensajes de texto (SMS) y la **tarjeta SIM** para identificar al abonado. Su arquitectura incluía el **BSC** (Controlador de Estación Base) y bases de datos como el **HLR** (Registro de Ubicación de Origen) y **VLR** (Registro de Ubicación de Visitante).
- **3G (UMTS):** Evolucionó para ofrecer servicios de banda ancha, permitiendo el acceso masivo a Internet desde móviles.
- **4G (LTE):** Representó un cambio radical al eliminar la conmutación de circuitos, basándose exclusivamente en la **conmutación de paquetes** (todo es IP). Su arquitectura se divide en:
    - **E-UTRAN:** La red de acceso radioeléctrico.
    - **EPC (Evolved Packet Core):** La red principal que contiene el **MME** (gestión de movilidad), el **S-GW** (pasarela de servicio), el **P-GW** (pasarela a Internet) y el **HSS** (servidor de abonados).
- **5G:** Enfocada en tasas de hasta **10 Gbps** y latencia ultrabaja. Utiliza tecnologías como **Massive MIMO** (múltiples antenas) y **fragmentación de red** (_network slicing_) para crear redes virtuales sobre una misma infraestructura física
# Diferencia entre conmutacion de paquetes y conmutacion de circuitos
Históricamente, la telefonía móvil se basaba en la **conmutación de circuitos** (un camino físico dedicado por llamada), pero las redes modernas (4G/5G) utilizan la **conmutación de paquetes**, donde los datos se dividen en fragmentos independientes que viajan por la ruta más eficiente, permitiendo una asignación de recursos mucho más dinámica
![[Pasted image 20260305161237.png]]
# 3. Redes inalambricas(WIFI):
Las **redes inalámbricas**, comúnmente conocidas como **WiFi**, se rigen por el estándar **IEEE 802.11** 
- **Modo Infraestructura:** Es el más común, donde cada cliente (laptop, smartphone) se asocia a un **Punto de Acceso (AP)** o router inalámbrico
- **Modo Ad-hoc:** Los dispositivos se comunican directamente entre sí sin necesidad de un punto de acceso central.
- **Red Mallada (Mesh):** Los nodos retransmiten paquetes entre sí, una configuración popular en hogares grandes o regiones en desarrollo
WiFi utiliza frecuencias en **bandas sin licencia** (ISM), como 2,4 GHz y 5 GHz, lo que significa que debe coexistir con interferencias de hornos microondas y otros dispositivos inalámbricos.
- **Control de Acceso (CSMA/CA):** Debido a que las radios no pueden detectar colisiones mientras transmiten, utilizan el "acceso múltiple con detección de portadora y evitación de colisiones". Los dispositivos escuchan el canal y esperan un tiempo aleatorio antes de transmitir.
- **Acuses de Recibo (ACK):** A diferencia de Ethernet, WiFi requiere que el receptor envíe un acuse de recibo por cada trama para confirmar que no hubo colisiones.
- **Problemas de Señal:** Las redes WiFi enfrentan desafíos como el **desvanecimiento multitrayecto** (reflexiones de la señal en objetos sólidos) y el problema del **terminal oculto** (cuando dos estaciones no se ven entre sí pero interfieren al transmitir al mismo receptor)
# Versiones (LEER)
- **802.11 (Original, 1997):** Definía una LAN inalámbrica que funcionaba a **1 Mbps o 2 Mbps** mediante saltos de frecuencia o espectro ensanchado.
- **802.11b (1999):** Funcionaba en la banda de **2,4 GHz** con velocidades de hasta **11 Mbps**. Fue un gran éxito comercial por su facilidad de despliegue.
- **802.11a (1999):** Introdujo la técnica **OFDM** y operaba en la banda de **5 GHz**, alcanzando hasta **54 Mbps**. Aunque era más rápida, tenía un alcance menor que la versión "b".
- **802.11g (2003):** Utilizó los métodos de modulación OFDM en la banda de **2,4 GHz**, ofreciendo hasta **54 Mbps** y manteniendo la compatibilidad con dispositivos 802.11b.
- **802.11n (2009):** Introdujo la tecnología **MIMO** (múltiples antenas) para transmitir varios flujos de datos simultáneamente, superando los **100 Mbps** de rendimiento real.
- **802.11ac (2013):** Utiliza canales más anchos, modulación 256-QAM y **MU-MIMO** (MIMO multiusuario) en la banda de **5 GHz**, con una capacidad teórica de hasta **7 Gbps** (aunque en la práctica es menor).
- **802.11ad (2012):** Funciona en la banda de **60 GHz** (ondas milimétricas) alcanzando **7 Gbps**. Su alcance es muy corto y no atraviesa paredes, por lo que es ideal para uso dentro de una sola habitación.
- **802.11ax / WiFi 6 (2019):** Conocida como "inalámbrica de alta eficiencia", utiliza **OFDMA** y 1024-QAM para alcanzar velocidades teóricas de hasta **11 Gbps**. Está diseñada para mejorar la eficiencia en entornos densos y reducir el consumo de energía.
- **802.11ay:** Es una mejora de la versión "ad" que multiplica por cuatro el ancho de banda en la frecuencia de 60 GHz

> [NOTA!]
>***interconectados***: se dice que dos ordenadores estan interconectados si pueden intercambiar informacion 
>***Datacenter***:
>Es un edificio o una sala dedicada exclusivamente a alojar una gran cantidad de computadoras (servidores) y equipos de almacenamiento conectados a la red las 24 horas del día.
