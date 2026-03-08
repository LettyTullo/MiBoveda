Conjunto de reglas y normas que deben cumplir los dispositivos para la comunicacion  de datos de una red.
# Cuestiones para analisis de protocolos
- Sintaxis: Longitudes de encabezados, campos y reglas de relleno
- Semantica: Significado de cada campo o encabezado
- Temporizacion: Timers
![[Pasted image 20260307203254.png]]

## Objetivos de diseño
# - Fiabilidad:
Consiste en diseñar una red que funcione correctamente aunque este formada por un conjunto de componentes que, a su vez, no son fiables. Ej:
- **Deteccion de errores:** La informacion recibida incorrectamente puede retransmitirse hasta que se reciba correctamente
- **Correccion de errores:** Recuperan el mensaje correcto a partir de los bits posiblemente incorrectos que se recibieron originalmente
- **Enrutamiento**: Encontrar un camino que funcione a traves de una red, permitiendo a routers individuales tomar una decision automaticamente
# - Asignacion de recursos:
- Se dice que los recursos que siguen funcionando bien cuando la red crece son escalables. Para lograr la escalabilidad, se utilizan mecanismos como las **estructuras de capas**, que dividen los problemas complejos en partes manejables, y el **enrutamiento jerárquico**, que permite a los routers manejar prefijos de red en lugar de direcciones de hosts individuales para reducir el tamaño de sus tablas
- Un reparto basado en las estadisticas de demanda se denomina multiplexacion estadistica
- Un problema comun de asignacion que se plantea en todos los niveles es como evitar que un emisor rapido sature de datos a un receptor lento, a este se lo denomina control de flujo. Este  ocurre cuando el emisor tiene una capacidad de transmisión superior a la capacidad de procesamiento o almacenamiento del receptor. 
- A veces la red se sobrecarga porque demasiados ordenadores quieren enviar demasiado trafico y la red no puede entregarlo todo a esta sobrecarga  se le denomina **congestion.**
- La **calidad de servicios** se refiere al conjunto de mecanismos que permiten a la red conciliar las demandas contrapuestas de recursos para satisfacer las necesidades de las aplicaciones.
# -Capacidad de evolucion:
- Con el tiempo las redes crecen y surgen nuevos diseños que deben conectarse a la red existente. 
- La estratificacion es el mecanismo de estructuracion mas importante para soportar el cambio es la organizacion de la red en una pila de capas o niveles.
- Dado que hay muchos ordenadores en la red, cada capa necesita un mecanismo para identificar a los remitentes y receptores que participan en un mensaje concreto. Este mecanismo se denomina direccionamiento o denominacion, en las capas baja y alta respectivamente.
- Otro ejemplo son las diferencias en el tamaño maximo de los mensajes que pueden transmitir las redes. Esto da lugar a mecanismos para desensamblar, transmitir y volver a ensamblar mensajes. Esto se denomina internetworking.  
# -Seguridad:
- Se refiere a proteger la red contra distintos tipos de amenazas. 
- Los mecanismos que proporcionan confidencialidad defienden contra esa amenaza , y se utilizan en multiples capas.
- Los mecanismos de autenticacion impiden que alguien se haga pasar por otra persona.
- Otros mecanismos de integridad evitan cambios subrepticios en los mensajes.
## Estratificacion de protocolos
- El proposito de cada capa es ofrecer ciertos servicios a las capas superiores.
- Las entidades que comprenden las capas correspondientes en diferentes maquinas se denominan pares.
- Cada instancia del protocolo se comunica virtualmente con su par.
- Entre cada par de capas adyacentes hay una interfaz. La interfaz define qué operaciones y servicios primitivos pone la capa inferior a disposición de la superior.
- Cada capa pasa datos e información de control a la capa inmediatamente inferior, hasta llegar a la capa más baja. Por debajo de la capa 1 se encuentra el medio físico a través del cual se produce la comunicación real.
- Un conjunto de capas y protocolos se denomina arquitectura de red.
- Una lista de los protocolos utilizados por un determinado sistema, un protocolo por capa se denomina pila de protocolos
# Aspectos de diseño de capas:
- **Direccionamiento:** Necesarios para identificar emisores y receptores. Campos "Direccion de origen", "Direccion de destino"
- **Control de errores**
- **Numeracion:** Forma de secuenciar los mensajes. Campos de "numero de secuencia"
- **Control de flujo**
- **Segmentacion:** Mecanismos para segmentar y re-ensamblar mensajes. Campos de "mas fragmentos" o "ultimo fragmento"
- **Multiplexado** 
- **Enrutamiento**
## Primitivas de servicio
# Servicios orientados a la conexion 
Este servicio se modela según el **sistema telefónico**. Para utilizarlo, el usuario debe seguir tres fases diferenciadas:
1. **Establecimiento:** Se establece la conexión y, frecuentemente, se realiza una **negociación** de parámetros como la calidad de servicio (QoS) y el tamaño máximo de los mensajes.
2. **Transferencia de datos:** Los datos se envían a través de la conexión, que actúa como un "tubo" donde los bits llegan al receptor en el mismo **orden** en que fueron enviados.
3. **Liberación:** Se cierra la conexión para liberar recursos en los hosts y dispositivos intermedios
# Servicios no orientados a la conexion
Este servicio sigue el modelo del **sistema postal** o un telegrama. Sus características principales son:
- **Independencia de mensajes:** No requiere configuración previa; cada mensaje (denominado **datagrama**) lleva la dirección completa de destino y se enruta de forma independiente a los demás.
- **Orden de llegada:** Los paquetes pueden llegar al destino en un **orden distinto** al de salida, ya que pueden seguir rutas diferentes según el estado de la red.
- **Eficiencia y Robustez:** Es más eficiente al no desperdiciar ancho de banda con reservas fijas y permite una recuperación más fácil ante fallos de enrutadores, aunque dificulta garantizar la Calidad de Servicio
## Servicios confiables y no confiables
# Servicios confiables
El receptor emite un mensaje de **confirmación (ACK)** por cada dato recibido. Si el emisor no recibe el ACK en un tiempo determinado, retransmite el dato. Un servicio sin conexión pero fiable es el **WiFi (802.11)**, que reconoce individualmente cada trama enviada
Necesita en general, la numeracion de los mensajes y utilizacion de "timers"
Tambien se puede utilizar una "confirmacion negativa" o NAK
# Servicios no confiables
También llamado de "mejor esfuerzo", donde no se confirman los mensajes y se deja la gestión de errores para las capas superiores. Ejemplos comunes son **Ethernet** y el correo basura electrónico

## Servicio VS Protocolo
Un servicio es un conjunto de primitivas (operaciones) que una capa proporciona a la capa superior. El servicio define que operaciones puede realizar la capa en nombre de sus usuarios, pero no dice nada en lo absoluto de como se implementan estas operaciones. Esta relacionado con la interfaz entre dos capas.
Un protocolo es un conjunto de reglas que rigen el formato y el significado de los paquetes o mensajes que intercambian las entidades pares dentro de una capa. Las entidades utilizan protocolos para aplicar sus definiciones de servicio.
