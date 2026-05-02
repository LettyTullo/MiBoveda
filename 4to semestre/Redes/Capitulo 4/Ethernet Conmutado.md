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

>[!danger] Estan bajo 3 estrategias para procesar las tramas.
>1. **Store and forward:** Recibe la trama completa, verifica el CRC (errores) y luego la reenvía; es el más seguro pero tiene más retardo.
>2. **Cut-through:** Comienza a retransmitir en cuanto lee la dirección de destino, incluso antes de recibir toda la trama, minimizando el retardo.
>3. **Free of segments:** Espera a recibir los primeros 64 bytes para asegurarse de que no es un fragmento de una colisión antes de reenviar.

## Tabla CAM
El switch (o conmutador) gestiona el tráfico de red de forma inteligente mediante el algoritmo de aprendizaje hacia atrás. Este mecanismo permite que el dispositivo identifique la ubicación de cada equipo en la red sin configuración manual.
# 1. La Tabla CAM (Content Addressable Memory)
La tabla CAM es una base de datos interna que asocia una dirección MAC con un puerto físico del switch.
- Al iniciar el dispositivo, la tabla se encuentra vacía.
- El switch no conoce qué equipo está conectado a qué puerto hasta que se genera tráfico.
- El aprendizaje se realiza observando la dirección MAC de origen de las tramas entrantes.
## 2. El Proceso de Aprendizaje hacia atrás

Cuando un equipo envía una trama, el switch realiza los siguientes pasos:

1. Inspecciona el campo de dirección MAC de origen de la trama.
    
2. Identifica por qué puerto físico ingresó dicha trama.
    
3. Crea una entrada en la tabla: MAC [Dirección] -> Puerto [Número].
    
4. Añade un temporizador (aging timer) a la entrada. Si el equipo no envía datos durante un tiempo determinado, la entrada se borra para mantener la tabla limpia y permitir cambios físicos en la red.
    

## 3. El Dilema del Destino Desconocido: Inundación (Flooding)

Cuando llega una trama, el switch consulta la dirección MAC de destino en su tabla CAM. Si el destino no figura en la tabla, el switch no sabe por dónde enviarlo.

- **Algoritmo de Inundación:** El switch copia la trama y la envía por todos los puertos activos, excepto por aquel puerto por el que recibió la trama originalmente.
    
- **Resultado:** El destinatario real recibe el mensaje y responde. En cuanto esa respuesta llega al switch, este aprende la ubicación del destinatario siguiendo el proceso de aprendizaje de origen.
    

## 4. Transmisiones Directas y Aislamiento de Tráfico

Una vez que la tabla CAM tiene registradas las direcciones, el switch optimiza la red:

- **Conmutación inmediata:** Si el Host A quiere hablar con el Host D y ambos están en la tabla, el switch conecta sus puertos de forma privada a través de su electrónica interna.
    
- **Eficiencia:** A diferencia de un hub, los demás puertos no reciben este tráfico. Esto permite que múltiples pares de computadoras se comuniquen en paralelo sin generar colisiones entre sí.
    

---

## Resumen del Algoritmo de Reenvío (Lógica de Decisión)

Ante cada trama recibida, el switch sigue este flujo lógico:

1. **¿Es una dirección de difusión (Broadcast)?**
    
    - Si: Inunda todos los puertos (excepto el de origen).
        
2. **¿Conozco el puerto de destino en mi tabla CAM?**
    
    - No: Inunda todos los puertos (excepto el de origen).
        
    - Si: Verifica el puerto de salida.
        
3. **¿El puerto de salida es el mismo por el que entró la trama?**
    
    - Si: Descarta la trama (filtrado), pues el destino ya está en ese segmento.
        
    - No: Envía la trama directamente al puerto de destino correspondiente (Unicast).