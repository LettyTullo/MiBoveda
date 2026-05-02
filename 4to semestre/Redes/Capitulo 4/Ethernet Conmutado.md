El **Ethernet conmutado** es la arquitectura moderna de las redes de área local (LAN) en la que se utilizan dispositivos llamados **conmutadores (switches)** para conectar los diferentes ordenadores mediante enlaces punto a punto. A diferencia de la Ethernet clásica, donde todos los dispositivos compartían un único cable o "bus", en la conmutada cada ordenador tiene un cable dedicado que va al switch, lo que permite un uso mucho más eficiente de la red.
# Diferencias entre Hub y Switch
| **Característica**             | **Hub (Concentrador)**                                                                                   | **Switch (Conmutador)**                                                                                 |
| ------------------------------ | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Capa Modelo OSI**            | **Capa 1 (Física)**: Actúa como un simple repetidor de señales analógicas.                               | **Capa 2 (Enlace de Datos)**: Entiende tramas Ethernet y direcciones MAC.                               |
| **Gestión de Tráfico**         | Al recibir bits por un puerto, los repite por todos los demás sin distinción.                            | Solo envía la trama al puerto donde se encuentra el destinatario específico.                            |
| **Dominios de Colisión**       | **Único dominio**: Todos sus puertos comparten el mismo dominio; si dos transmiten a la vez, hay choque. | **Segmentación**: Aísla cada puerto en su propio dominio de colisión separado.                          |
| **Inteligencia y Aprendizaje** | **Nula**: No sabe quién está conectado; no posee registros de direcciones.                               | **Aprendizaje hacia atrás**: Construye una **Tabla CAM** asociando direcciones MAC con puertos físicos. |
| **Manejo de Desconocidos**     | No discrimina, siempre envía a todos.                                                                    | Si no conoce el destino, inunda todos los puertos; una vez aprendido, la transmisión es directa.        |
| **Modo de Transmisión**        | **Half-duplex (semidúplex)**: Debe gestionar colisiones, por lo que no puede enviar y recibir a la vez.  | **Full-duplex (dúplex completo)**: Envía y recibe simultáneamente sin riesgo de colisiones.             |
| **Eficiencia y Protocolos**    | Obliga al uso de **CSMA/CD** para gestionar los choques de señales.                                      | Elimina la necesidad de CSMA/CD y permite comunicaciones en paralelo a alta velocidad.                  |

# Modos de trabajo del Switch

Los switches modernos pueden operar bajo tres estrategias para procesar las tramas:

1. **Store and forward:** Recibe la trama completa, verifica el CRC (errores) y luego la reenvía; es el más seguro pero tiene más retardo.
2. **Cut-through:** Comienza a retransmitir en cuanto lee la dirección de destino, incluso antes de recibir toda la trama, minimizando el retardo.
3. **Free of segments:** Espera a recibir los primeros 64 bytes para asegurarse de que no es un fragmento de una colisión antes de reenviar.

En resumen, el hub reparte el ancho de banda total entre todos los usuarios, mientras que el switch proporciona a cada conexión punto a punto el ancho de banda completo del enlace, mejorando drásticamente el rendimiento y la seguridad de la red.