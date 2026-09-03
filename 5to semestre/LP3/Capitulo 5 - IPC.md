El **Capítulo 5** aborda la **Comunicación Interprocesos (IPC)**. Hasta ahora, la creación de procesos con `fork()` y `wait()` limitaba la comunicación a los códigos de salida de los hijos. Estos mecanismos de control básicos no permiten enviar ni recibir datos de un proceso mientras se está ejecutando, ni comunicarse con procesos fuera de la jerarquía directa de padre e hijo.
# 1. Memoria Compartida (Shared Memory)

Es el **mecanismo de IPC más rápido** en sistemas UNIX. Permite que dos o más procesos compartan directamente una porción de su memoria virtual física, como si todos hubieran llamado a `malloc()` y recibido exactamente el mismo puntero de memoria real. El acceso es inmediato (lectura y escritura directa) sin entrar en el núcleo del sistema operativo en cada interacción, evitando copias de datos innecesarias.

La contrapartida es que **el kernel no sincroniza los accesos**; si dos procesos escriben simultáneamente o uno lee mientras otro escribe, el resultado es indefinido. Por ende, requiere que el programador implemente un protocolo de sincronización utilizando semáforos.
#### **Funciones necesarias para su implementación:**

- **`shmget` (Asignación):** Asigna o recupera un segmento de memoria compartida.
    - **Cabecera:** `#include <sys/shm.h>`, `#include <sys/ipc.h>`.
    - **Prototipo:** `int shmget(key_t key, size_t size, int shmflg);`
    - **Parámetros:**
        - `key`: Una clave numérica que identifica el segmento en el sistema operativo. Al usar la constante especial `IPC_PRIVATE` se garantiza que se asigne un segmento exclusivo y completamente nuevo.
        - `size`: Tamaño deseado del segmento en bytes (el kernel lo redondea automáticamente al tamaño de página del sistema, que suele ser de 4 KB).
        - `shmflg`: Banderas de control unidas mediante un operador OR de bits (`|`), como `IPC_CREAT` (para crear el segmento si no existe), `IPC_EXCL` (para forzar que falle si el segmento ya existía de antemano), y los permisos estándar de lectura y escritura para el dueño (`S_IRUSR | S_IWUSR`).
    - **Retorno:** Devuelve el identificador único del segmento (`segment_id`) en caso de éxito, o `-1` si hay un error.
- **`shmat` (Adjuntar):** Mapea el segmento de memoria compartida en el espacio de direcciones virtuales de tu proceso para que sea accesible a través de un puntero.
    - **Cabecera:** `#include <sys/shm.h>`.
    - **Prototipo:** `void* shmat(int shmid, const void *shmaddr, int shmflg);`
    - **Parámetros:**
        - `shmid`: El ID del segmento devuelto previamente por `shmget`.
        - `shmaddr`: La dirección física de memoria donde se desea forzar el mapeo. Si se pasa `0` o `NULL`, el kernel de Linux elige automáticamente la primera dirección de memoria disponible, que es el comportamiento recomendado.
        - `shmflg`: Banderas para modificar el mapeo. Se puede pasar `SHM_RDONLY` para evitar escrituras accidentales o `SHM_RND`.
    - **Retorno:** El puntero de inicio de la memoria compartida (`shared_memory`) o `(void*) -1` en caso de error.
- **`shmdt` (Desvincular):** Desasocia el segmento de memoria de las direcciones de tu proceso.
    - **Cabecera:** `#include <sys/shm.h>`.
    - **Prototipo:** `int shmdt(const void *shmaddr);`
    - **Parámetros:** El puntero de memoria compartida devuelto anteriormente por `shmat`.
    - **Nota de seguridad:** Invocar `exit()` o funciones de la familia `exec` desvinculan automáticamente la memoria compartida.
- **`shmctl` (Control y Desasignación):** Permite obtener información sobre el estado de un segmento o marcarlo para destrucción definitiva en el sistema operativo.
    - **Cabecera:** `#include <sys/shm.h>`.
    - **Prototipo:** `int shmctl(int shmid, int cmd, struct shmid_ds *buf);`
    - **Parámetros:**
        - `shmid`: ID del segmento de memoria compartida.
        - `cmd`: Comando a ejecutar. Para obtener información se usa `IPC_STAT` y se rellena una estructura del tipo `shmid_ds`. Para destruir y desasignar el segmento del kernel, se usa `IPC_RMID` (en este caso el tercer parámetro se pasa como `NULL` o `0`).
    - **Regla de oro:** Es obligatorio llamar explícitamente a `shmctl` con `IPC_RMID` al terminar, ya que la memoria compartida persiste en el kernel de Linux incluso después de que los procesos finalicen, lo que puede provocar que el sistema agote sus límites globales de memoria compartida.

> **Caso de uso típico:** Un marcador de fútbol en tiempo real (`marcador.c`). Un proceso "anotador" actualiza el marcador en memoria compartida, y múltiples procesos de "pantalla" leen la estructura de datos al instante, de manera simultánea y sin demora de E/S.
# 2. Semáforos de Proceso (System V Semaphores)

Son la contraparte a nivel de procesos de los semáforos de pthreads estudiados anteriormente. Se utilizan principalmente para **sincronizar y coordinar el acceso compartido** a la memoria y evitar condiciones de carrera. Estos semáforos se crean, administran y destruyen usando claves globales del sistema y **vienen en conjuntos (sets)** administrados de manera unificada.

#### **Funciones necesarias para su implementación:**

- **`semget` (Asignación):** Obtiene o crea un conjunto de semáforos en el kernel.
    - **Cabecera:** `#include <sys/sem.h>`, `#include <sys/ipc.h>`, `#include <sys/types.h>`.
    - **Prototipo:** `int semget(key_t key, int nsems, int semflg);`
    - **Parámetros:** `key` (la clave del conjunto de semáforos), `nsems` (la cantidad de semáforos que tendrá el conjunto) y `semflg` (permisos y banderas análogas a las de memoria compartida).
    - **Retorno:** ID del conjunto de semáforos en el sistema (`semid`).
- **`semctl` (Control, Inicialización y Destrucción):** Administra los semáforos del conjunto.
    - **Cabecera:** `#include <sys/sem.h>`, `#include <sys/ipc.h>`, `#include <sys/types.h>`.
    - **Prototipo:** `int semctl(int semid, int semnum, int cmd, ...);`
    - **Parámetros:**
        - `semid`: ID de semáforos obtenido de `semget`.
        - `semnum`: El índice específico del semáforo dentro del conjunto (numerado de `0` en adelante).
        - `cmd`: Acción a realizar. `SETVAL` para darle un valor inicial a un semáforo individual, `SETALL` para inicializarlos todos en masa usando un arreglo, o `IPC_RMID` para liberar y desasignar los recursos del semáforo inmediatamente del kernel.
        - `arg` (Opcional): Una unión especial que el programador **debe definir explícitamente en el código C** denominada `union semun`. Contiene un entero `val` (para `SETVAL`) o un puntero de tipo `unsigned short *array` (para `SETALL`).
- **`semop` (Operaciones Atómicas):** Realiza operaciones de incremento (aviso/post) o decremento (espera/wait) de forma estrictamente atómica sobre los semáforos del conjunto.
    - **Cabecera:** `#include <sys/sem.h>`, `#include <sys/ipc.h>`, `#include <sys/types.h>`.
    - **Prototipo:** `int semop(int semid, struct sembuf *sops, size_t nsops);`
    - **Parámetros:** `semid` (ID del conjunto), `sops` (puntero a un arreglo de una o más estructuras `struct sembuf` que detallan las operaciones) y `nsops` (longitud del arreglo de estructuras).
    - **El componente `struct sembuf`**: Esta estructura define de forma precisa la operación a ejecutar mediante tres campos:
        1. `sem_num`: El semáforo del conjunto en el que se opera.
        2. `sem_op`: El valor entero del cambio.
            - Si es **negativo (ej: `-1`)**, actúa como un **`wait()`**: decrementa el contador del semáforo, y si su valor actual es menor que el valor absoluto, bloquea de forma segura al proceso hasta que se incremente.
            - Si es **positivo (ej: `+1`)**, actúa como un **`post()`**: incrementa el contador del semáforo y reactiva a un proceso bloqueado de forma inmediata.
            - Si es **cero**, bloquea al proceso llamador hasta que el valor de ese semáforo se reduzca a cero.
        3. `sem_flg`: Banderas de control de la operación. Es fundamental pasar la bandera **`SEM_UNDO`**. Esto instruye a Linux para que lleve un registro de las operaciones y, si el proceso finaliza de forma abrupta o imprevista mientras mantiene bloqueado el recurso, el sistema operativo deshaga los efectos sobre el semáforo de manera automática, previniendo bloqueos perpetuos de otros procesos.

> **Caso de uso típico:** Un estacionamiento de capacidad limitada con un cupo fijo de N lugares (`estacionamiento.c`). Los autos (procesos independientes) decrementan el semáforo al entrar; al llegar el contador a 0, el kernel de Linux bloquea de forma pasiva a cualquier nuevo vehículo sin necesidad de usar esperas activas (_busy waiting_) consumiendo CPU.
# 3. Memoria Mapeada (Mapped Memory)

Asocia de manera unívoca un archivo existente en el disco rígido con un rango de la memoria virtual de tu proceso. El archivo se divide en fragmentos del tamaño de página y se cargan automáticamente en memoria.

De este modo, puedes leer y modificar la información del archivo de disco simplemente haciendo accesos a variables o escribiendo en arreglos en memoria ordinaria, sin invocar llamadas tradicionales como `read()` o `write()`. Al mapear con la bandera **`MAP_SHARED`**, cualquier modificación se sincroniza de inmediato al archivo y se hace visible para cualquier otro proceso que mapee el mismo archivo en el sistema.
#### **Funciones necesarias para su implementación:**

- **`mmap` (Crear Mapeo):** Crea la asociación de memoria sobre un archivo.
    - **Cabecera:** `#include <sys/mman.h>`, `#include <fcntl.h>`.
    - **Prototipo:** `void* mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);`
    - **Parámetros:**
        - `addr`: Dirección sugerida para el mapeo. Se pasa `0` o `NULL` para dejar que Linux decida.
        - `length`: El tamaño en bytes del archivo o porción de archivo a mapear.
        - `prot`: Nivel de protección de acceso de la región. OR de bits de `PROT_READ` (lectura), `PROT_WRITE` (escritura) y `PROT_EXEC` (ejecución).
        - `flags`: Opciones de compartición. Se usa obligatoriamente **`MAP_SHARED`** para IPC, lo cual escribe los cambios en el archivo físico de forma síncrona, o **`MAP_PRIVATE`** (Copy-On-Write) para trabajar con una copia privada cuyos cambios nunca se guardan en el archivo original de disco.
        - `fd`: El descriptor de archivo de un archivo abierto previamente de manera tradicional mediante `open()`. Cabe notar que una vez que se llama a `mmap()`, **se puede cerrar el descriptor de archivo de manera inmediata con `close(fd)`** y el mapeo continuará funcionando y permaneciendo vivo de forma normal.
        - `offset`: Desplazamiento desde el inicio del archivo a partir de dónde mapear (suele pasarse `0` para mapear todo el archivo).
    - **Retorno:** Puntero al inicio de la memoria mapeada, o la constante de error **`MAP_FAILED`** si falla.
- **`munmap` (Liberar Mapeo):** Destruye el mapeo establecido sobre las direcciones virtuales del proceso.
    - **Cabecera:** `#include <sys/mman.h>`.
    - **Prototipo:** `int munmap(void *addr, size_t length);`
    - **Parámetros:** Puntero de inicio de la memoria y la longitud del mapeo.
- **`msync` (Forzar Volcado a Disco):** Fuerza la sincronización manual de los búferes internos del sistema operativo de vuelta al archivo en el sistema de archivos.
    - **Cabecera:** `#include <sys/mman.h>`.
    - **Prototipo:** `int msync(void *addr, size_t length, int flags);`
    - **Parámetros:** Dirección, longitud y banderas de control como `MS_SYNC` (bloquea la llamada de sistema hasta que el archivo físico esté completamente escrito en disco), `MS_ASYNC` (programa la escritura de forma paralela en segundo plano) y `MS_INVALIDATE` (indica que todos los demás mapeos paralelos actualicen sus lecturas frente a la modificación de este proceso).

> **Caso de uso típico:** El registro persistente de puntajes altos de un juego de Arcade (`arcade.c`). Dado que el récord debe sobrevivir ante apagados repentinos del equipo, el proceso mapea a memoria un archivo físico (`high_score.dat`). Al rebasar la marca, se escribe directamente en un entero mapeado y el archivo se actualiza en el disco rígido de forma transparente para el programador.

# 4. Pipes (Tuberías) y FIFOs (Named Pipes)

Son canales de comunicación unidireccionales y secuenciales en un solo sentido; los datos escritos en un extremo se reciben en el mismo orden en el extremo opuesto (dispositivos seriales). El canal bloquea al escritor si el lector no consume y la tubería se llena, y bloquea al lector si no hay datos, sincronizando de forma transparente los procesos.

Existen dos tipos de tuberías:

- **Pipes Comunes:** Carecen de nombre en el sistema y se heredan por descriptores duplicados. Por ello, **solo pueden conectar procesos con relación de parentesco** (padre e hijo creados mediante `fork()`).
- **FIFOs (Tuberías Nombradas):** Aparecen como un archivo especial de tipo tubería (`p`) en el sistema de archivos. Permiten que **procesos sin ninguna relación de parentesco** se comuniquen de manera idéntica.

#### **Funciones necesarias para su implementación:**

- **`pipe` (Crear Tubería Común):** Abre un canal de tubería anónimo.
    - **Cabecera:** `#include <unistd.h>`.
    - **Prototipo:** `int pipe(int fds);`
    - **Parámetros:** Un arreglo de dos enteros (`fds`). El sistema operativo lo inicializa colocando el descriptor de **lectura en `fds`** y el descriptor de **escritura en `fds`**.
- **`dup2` (Redirección de Descriptores):** Permite equiparar un descriptor de archivo arbitrario sobre otro, cerrando el de destino de ser necesario.
    - **Cabecera:** `#include <unistd.h>`.
    - **Prototipo:** `int dup2(int oldfd, int newfd);`
    - **Parámetros:** `oldfd` (descriptor actual del pipe) y `newfd` (el descriptor estándar que se desea sobrescribir, comúnmente `STDIN_FILENO` o `STDOUT_FILENO`). Esto permite enlazar el flujo del pipe a la entrada o salida estándar del proceso antes de realizar un `exec()`, permitiendo crear cadenas de comandos como `ls | sort`.
- **`popen` y `pclose` (Atajo de Alto Nivel):** Simplifican drásticamente el proceso al encapsular internamente las llamadas a `pipe()`, `fork()`, `dup2()` y la familia `exec()` en un solo llamado, manejando la comunicación mediante flujos de biblioteca estándar de C (`FILE*`).
    - **Cabecera:** `#include <stdio.h>`.
    - **Prototipo:** `FILE* popen(const char *command, const char *type);` e `int pclose(FILE *stream);`
    - **Parámetros:** `command` (el comando de consola a ejecutar en el subproceso usando `/bin/sh`) y `type` (se pasa `"r"` si el padre quiere leer la salida estándar generada por el comando hijo, o `"w"` si el padre desea escribir en la entrada estándar del comando). Al terminar de operar, se llama obligatoriamente a `pclose()` para cerrar el pipe de manera asíncrona y esperar a que el subproceso termine recuperando su código de salida.
- **`mkfifo` (Crear Tubería con Nombre):** Crea el archivo de tubería especial en el disco para la comunicación sin parentesco.
    - **Cabecera:** `#include <sys/types.h>`, `#include <sys/stat.h>`.
    - **Prototipo:** `int mkfifo(const char *pathname, mode_t mode);`
    - **Parámetros:** `pathname` (la ruta física donde se guardará el archivo, por ejemplo `/tmp/fifo`) y `mode` (los permisos del archivo). Una vez creado, cualquier proceso no relacionado puede acceder a él usando las funciones de bajo nivel habituales (`open()`, `read()`, `write()`, `close()`) o las de alto nivel (`fopen()`, `fprintf()`, `fscanf()`, `fclose()`).

> **Caso de uso típico:** El sistema de envío de pedidos de hamburguesas en una cocina (`pedidos.c`). Un cocinero escribe de manera secuencial los nombres de las hamburguesas preparadas de un lado del pipe, y el proceso "despachador" las lee y empaca en el mismo orden en que fueron enviadas, de forma secuencial y ordenada.

# 5. Sockets

Es un mecanismo de comunicación **bidireccional** sumamente flexible que permite enlazar dos procesos de manera directa, ya sea que corran bajo la misma máquina local o en **computadoras geográficamente distintas conectadas mediante una red** (Internet).
#### **Conceptos Fundamentales de Sockets:**

1. **Estilo de comunicación:**
    - `SOCK_STREAM` (Orientado a conexión): Fiable, garantiza que todos los paquetes lleguen intactos y en el mismo orden enviado (ejemplo clásico: el protocolo **TCP**).
    - `SOCK_DGRAM` (Estilo Datagrama): No garantiza orden ni entrega ("mejor esfuerzo"). Los paquetes pueden perderse, comportándose como el correo postal (ejemplo clásico: **UDP**).
2. **Namespace (Espacio de nombres):**
    - `PF_LOCAL` / `PF_UNIX` (Local): Las direcciones son simples nombres de archivos en el disco y sirve para comunicar procesos en la misma máquina local de forma extremadamente veloz.
    - `PF_INET` (Internet): Las direcciones de conexión se conforman por la dirección IP de 32 bits de la máquina en red y un número de puerto que distingue al socket entre los demás procesos del host.
#### **Llamadas al sistema del Servidor (Orientado a Conexión):**

- **`socket` (Creación):** Genera el descriptor del socket inicial.
    - **Cabecera:** `#include <sys/socket.h>`.
    - **Prototipo:** `int socket(int domain, int type, int protocol);`
    - **Parámetros:** `domain` (el namespace como `PF_LOCAL` o `PF_INET`), `type` (el estilo de transmisión, ej: `SOCK_STREAM`) y `protocol` (se pasa `0` para elegir por defecto el protocolo correcto asociado a la combinación). Devuelve el descriptor del socket.
- **`bind` (Enlazar Dirección):** Asocia e imprime una dirección física al socket creado para que los clientes puedan localizarlo.
    - **Cabecera:** `#include <sys/socket.h>`.
    - **Prototipo:** `int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);`
    - **Parámetros:** El descriptor del socket (`sockfd`), un puntero a una estructura de dirección (`struct sockaddr *addr`) y la longitud en bytes de dicha estructura (`addrlen`).
        - Para sockets de dominio local, el puntero apunta a una estructura del tipo `struct sockaddr_un` (con el campo `sun_family = AF_LOCAL` y `sun_path` especificando el nombre del archivo).
        - Para Internet, apunta a `struct sockaddr_in` (con `sin_family = AF_INET`, `sin_addr` conteniendo la IP de 32 bits, y `sin_port` el puerto).
- **`listen` (Habilitar Escucha):** Pone al socket en modo pasivo, preparado para recibir conexiones entrantes de clientes.
    - **Cabecera:** `#include <sys/socket.h>`.
    - **Prototipo:** `int listen(int sockfd, int backlog);`
    - **Parámetros:** El socket y `backlog`, que define el tamaño máximo de la cola para retener temporalmente conexiones entrantes pendientes de ser procesadas por el servidor.
- **`accept` (Aceptar Conexión):** Extrae y despacha la primera petición de conexión de la cola.
    - **Cabecera:** `#include <sys/socket.h>`.
    - **Prototipo:** `int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);`
    - **Retorno clave:** Genera y devuelve un **nuevo descriptor de socket de forma dinámica e independiente** destinado única y exclusivamente a interactuar con ese cliente en particular. El socket original del servidor permanece intacto y libre para seguir escuchando y aceptando a otros clientes simultáneamente.
#### **Llamadas al sistema del Cliente:**

- **`connect` (Iniciar Conexión):** Inicia de forma activa la conexión del socket local del cliente hacia la dirección del servidor receptor.
    - **Cabecera:** `#include <sys/socket.h>`.
    - **Prototipo:** `int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);`
    - **Parámetros:** Su socket, la estructura de la dirección del servidor (`addr`) y la longitud de la estructura.
#### **Herramientas de conversión esenciales (Sockets de Internet):**

- **`htons` (Host to Network Short):** Convierte el puerto entero de 16 bits del orden de bytes nativo del microprocesador local al orden de bytes estándar requerido por los protocolos de la red de Internet (_network byte order_), garantizando portabilidad.
- **`gethostbyname`:** Traduce nombres de dominio legibles por humanos (ej: "www.google.com") en direcciones IP numéricas de 32 bits estructuradas. Cabecera: `#include <netdb.h>`.

> **Caso de uso típico:** Un servidor de pedidos de una heladería (`heladeria.c`). Los clientes físicos locales se comunican mediante un socket en el namespace local `PF_LOCAL` (mostrador de la heladería), mientras que clientes distantes ingresan solicitudes mediante el namespace de Internet `PF_INET` (pedidos telefónicos o por red) al puerto 80 del servidor.
