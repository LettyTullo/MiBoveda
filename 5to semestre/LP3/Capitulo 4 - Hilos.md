# ¿Qué es un hilo (thread)?
En el entorno de Linux, un **hilo** es una **unidad de ejecución más fina que un proceso** que vive dentro de él. A diferencia de los procesos (donde cada uno tiene su propio espacio de memoria aislado):
- **Todos los hilos de un mismo proceso comparten el mismo espacio de direcciones de memoria, descriptores de archivos abiertos y recursos del sistema**.
- Si un hilo modifica una variable global o una estructura en memoria, **los demás hilos ven ese cambio de forma instantánea**.
- No obstante, **cada hilo tiene su propia pila de llamadas (call stack)**, lo que les permite ejecutar diferentes funciones, tener sus propias variables locales y evolucionar de manera independiente bajo la planificación asíncrona del núcleo de Linux.
- Existe un riesgo: dado que comparten el mismo espacio de memoria, **un hilo errante con un puntero corrupto puede dañar la memoria de los otros hilos** y hacer caer a todo el proceso. Asimismo, si cualquier hilo llama a una función de la familia `exec()`, todos los demás hilos de ese proceso finalizan inmediatamente.

Para trabajar con hilos en GNU/Linux se utiliza la API estándar de **POSIX Threads (pthreads)**, la cual requiere incluir la cabecera **`<pthread.h>`** en el código C y compilar añadiendo la bandera de enlace **`-lpthread`** (por ejemplo: `gcc -o programa programa.c -lpthread`).
# Funciones fundamentales para la implementación de hilos

#### 1. Creación y Ciclo de Vida de los Hilos

- **`pthread_create`**
    - **Firma:** `int pthread_create(pthread_t* thread, const pthread_attr_t* attr, void* (*start_routine)(void*), void* arg);`
    - **Para qué sirve:** Inicia un nuevo hilo de ejecución en el proceso actual.
    - **Argumentos:**
        1. `thread`: Puntero a una variable de tipo `pthread_t` donde se guardará el identificador único del nuevo hilo.
        2. `attr`: Puntero a una estructura de atributos (por ejemplo, para crearlo desacoplado). Pasar `NULL` aplica los atributos por defecto (hilo unible/joinable).
        3. `start_routine`: Puntero a la función ordinaria que ejecutará el hilo. Esta función debe obligatoriamente recibir un argumento de tipo `void*` y retornar un `void*`.
        4. `arg`: El argumento (de tipo `void*`) que se le pasará a la función del hilo.
- **`pthread_join`**
    - **Firma:** `int pthread_join(pthread_t thread, void** retval);`
    - **Para qué sirve:** Sincroniza la ejecución bloqueando al hilo llamador (normalmente el hilo principal o `main`) hasta que el hilo especificado en `thread` termine. Es el equivalente a `wait()` para procesos.
    - **Utilidad clave:** Si `retval` no es nulo, se almacena en él el valor de retorno que devolvió la función del hilo o el que se pasó a `pthread_exit`. **Saber que un hilo unible (joinable) no libera sus recursos del sistema hasta que otro hilo le hace un `join`**; si no se hace, se comporta como un hilo "zombie".
- **`pthread_exit`**
    - **Firma:** `void pthread_exit(void* retval);`
    - **Para qué sirve:** Finaliza de manera explícita el hilo que la invoca, devolviendo el puntero `retval` como su valor de retorno.
- **`pthread_self` y `pthread_equal`**
    - **Firmas:** `pthread_t pthread_self(void);` e `int pthread_equal(pthread_t t1, pthread_t t2);`
    - **Para qué sirven:** `pthread_self()` obtiene el identificador del hilo actual en ejecución. Como las estructuras `pthread_t` pueden variar entre arquitecturas, no deben compararse con `==`; se debe usar `pthread_equal()` para determinar si dos IDs de hilos son idénticos.
- **`pthread_detach`**
    - **Firma:** `int pthread_detach(pthread_t thread);`
    - **Para qué sirve:** Desacopla un hilo unible en cualquier momento de su ejecución. Un hilo desacoplado (detached) se limpia y libera sus recursos automáticamente al terminar, por lo que ningún otro hilo puede hacerle un `pthread_join` ni obtener su valor de retorno.

#### 2. Cancelación de Hilos desde el Exterior

- **`pthread_cancel`**: Envía una solicitud de terminación a otro hilo. El hilo de destino puede reaccionar de manera asíncrona (cancelarse inmediatamente) o síncrona/diferida (esperar a alcanzar un "punto de cancelación" seguro). El valor de retorno de un hilo cancelado es la constante especial `PTHREAD_CANCELED`.
- **`pthread_setcancelstate` y `pthread_setcanceltype`**: Permiten habilitar/deshabilitar la cancelación (creando secciones críticas no cancelables) y cambiar el tipo de cancelación, respectivamente.
- **`pthread_testcancel`**: Establece un punto de cancelación explícito en bucles o secciones de cálculo intensivo que no realizan de manera nativa otras llamadas bloqueantes.
- **`pthread_cleanup_push` y `pthread_cleanup_pop`**: Registran y retiran funciones de limpieza (_cleanup handlers_) que se ejecutan automáticamente si el hilo termina o es cancelado de forma imprevista, evitando fugas de memoria o recursos bloqueados.

#### 3. Datos Específicos del Hilo (Thread-Specific Data)

- Permiten que una variable global tenga un valor independiente para cada hilo (por ejemplo, un descriptor de log propio por hilo). Se implementa mediante las funciones **`pthread_key_create()`**, **`pthread_setspecific()`** y **`pthread_getspecific()`**.
# Mecanismos de Sincronización

La concurrencia trivializa compartir datos pero exige cuidar rigurosamente las **condiciones de carrera** (dos hilos intentando modificar la misma estructura a la vez). 
#### Que es una condicion de carrrera?
Una **condición de carrera** (o _race condition_) es un error que ocurre en entornos concurrentes (como programas con múltiples hilos o procesos) cuando dos o más de ellos compiten por acceder y modificar una misma estructura de datos o recurso compartido.

En este escenario, el resultado final de la ejecución es impredecible y depende exclusivamente del orden y de la velocidad con la que el planificador del sistema operativo asigne tiempo de CPU a cada hilo o proceso. El software solo funciona correctamente si un hilo específico se programa antes o con más frecuencia que el otro, lo cual no se puede garantizar de forma nativa.

### Mecanismo de fallo y consecuencias

Dado que los hilos dentro de un proceso comparten de forma trivial el mismo espacio de direcciones de memoria, los datos globales y los recursos del sistema:

1. Si un hilo actualiza solo de forma parcial una estructura de datos común y, antes de finalizar, el planificador del sistema lo interrumpe para dar paso a otro hilo.
2. El segundo hilo leerá o modificará una información incompleta o inconsistente, provocando fallos imprevisibles o caóticos.
3. Esto suele derivar en corrupción de datos en memoria, comportamientos anómalos imposibles de reproducir consistentemente en fase de depuración, o terminaciones abruptas del sistema por accesos inválidos, como un **error de segmentación** (_segmentation fault_).

### Un ejemplo clásico: la cola de trabajos (`job_queue`)

Imagina una cola de tareas compartida representada por una lista enlazada, donde múltiples hilos trabajadores consumen trabajos a medida que finalizan. Si queda **un único trabajo** en la cola y dos hilos se liberan casi al mismo tiempo:

- El **Hilo A** comprueba si la cola tiene trabajos y, al ver que no está vacía, guarda el puntero de ese único trabajo.
- Antes de que el Hilo A pueda desvincular ese trabajo de la lista, el sistema operativo lo interrumpe y cede el control al **Hilo B**.
- El **Hilo B** evalúa la cola, ve exactamente el mismo trabajo disponible y lo toma.
- Al reanudarse, ambos hilos intentarán procesar y posteriormente liberar (`free`) el mismo bloque de memoria del trabajo, provocando una inconsistencia y un error de segmentación cuando uno de ellos intente evaluar un puntero que ya apunta a nulo o que fue destruido.

### ¿Cómo se solucionan las condiciones de carrera?

Para eliminar estos fallos, es necesario convertir las lecturas y escrituras sobre variables compartidas en **operaciones atómicas** (es decir, secuencias indivisibles e ininterrumpibles). En Linux, se utilizan diversos mecanismos de sincronización:

- **Mutexes (Exclusión mutua):** Actúan como un cerrojo que solo permite que un único hilo a la vez acceda a la sección crítica (la porción de código que accede a los datos compartidos).
- **Semáforos:** Sincronizan el acceso mediante contadores seguros que bloquean de forma atómica a los hilos cuando un recurso limitado llega a cero.
- **Variables de condición:** Permiten suspender hilos de forma eficiente hasta que se cumpla una condición específica y otro hilo les envíe una señal de aviso.

---

🏃‍♂️ ¿Te gustaría que implementemos el código de la cola de trabajos utilizando un mutex en C para ver cómo se previene de forma práctica esta condición de carrera en tus ejercicios?
#### A. Mutexes (Exclusión Mutua)

Un mutex actúa como el pestillo de una puerta: solo un hilo puede poseerlo en un momento dado.

- **Inicialización:** `pthread_mutex_t m = PTHREAD_MUTEX_INITIALIZER;` (o mediante `pthread_mutex_init`).
- **`pthread_mutex_lock(&m)`:** Solicita el cerrojo. Si otro hilo lo tiene, bloquea al hilo llamador hasta que se libere.
- **`pthread_mutex_unlock(&m)`:** Libera el cerrojo. Debe ser invocado siempre por el mismo hilo que adquirió el bloqueo.
- **`pthread_mutex_trylock(&m)`:** Intenta tomar el cerrojo de manera no bloqueante. Si está ocupado, retorna instantáneamente el código `EBUSY`.

#### B. Semáforos (`<semaphore.h>`)

Es un contador entero no negativo que sincroniza hilos mediante mecanismos de espera y aviso.

- **Inicialización:** `sem_init(&sem, 0, valor_inicial);`.
- **`sem_wait(&sem)`:** Decrementa el contador. Si el valor es `0`, bloquea al hilo hasta que el contador sea positivo.
- **`sem_post(&sem)`:** Incrementa el contador y despierta a uno de los hilos que estaban bloqueados esperando en él.

#### C. Variables de Condición

Permiten suspender un hilo de manera eficiente hasta que se cumpla una condición arbitrariamente compleja y otro hilo le mande una señal de aviso. **Siempre se deben usar junto a un mutex**.

- **Inicialización:** `pthread_cond_t cv = PTHREAD_COND_INITIALIZER;`.
- **`pthread_cond_wait(&cv, &mutex)`:** Se invoca con el mutex bloqueado. De forma **atómica**, la función libera el mutex y pone el hilo a dormir en la variable de condición. Cuando es despertado, vuelve a adquirir el mutex de forma automática antes de retornar.
- **`pthread_cond_signal(&cv)`:** Despierta a un hilo bloqueado en la condición.
- **`pthread_cond_broadcast(&cv)`:** Despierta a todos los hilos que estén esperando en dicha condición.
# Claves y patrones esenciales para resolver ejercicios de hilos

Cuando programes o resuelvas problemas prácticos de concurrencia con pthreads, ten en cuenta las siguientes directivas obligatorias:

#### 1. Evitar "Punteros Colgantes" en los Argumentos de `pthread_create`
- **El peligro:** Si creas estructuras de argumentos (por ejemplo, para indicar qué rango de datos procesa un hilo) como variables locales del stack de una función, e inicias los hilos de manera asíncrona, la función creadora puede terminar y liberar esas variables del stack antes de que los hilos hayan terminado de leerlas.
- **La solución:** Asegúrate de llamar a **`pthread_join`** en la función creadora antes de salir de su ámbito para asegurar la permanencia de los argumentos. Alternativamente, reserva la memoria de los argumentos dinámicamente en el heap con `malloc()` y haz que sea el propio hilo el encargado de liberar esa memoria con `free()` al inicio o al finalizar su ejecución.
#### 2. El Patrón Correcto de la Variable de Condición: El bucle `while`

- **El error común:** Usar un `if` para verificar la bandera antes de esperar:
    
    ```
    // ¡INCORRECTO!
    pthread_mutex_lock(&m);
    if (!condicion) {
        pthread_cond_wait(&cv, &m);
    }
    // procesar...
    pthread_mutex_unlock(&m);
    ```
    
- **Por qué falla:** Existe el riesgo de "despertares espurios" o de que otro hilo consumidor se despierte primero y modifique el estado de la condición antes de que el hilo actual retome el control.
- **La clave de resolución:** Siempre se debe evaluar la condición en un bucle **`while`**:
    
    ```
    pthread_mutex_lock(&m);
    while (!condicion) {
        pthread_cond_wait(&cv, &m);
    }
    // Ahora es 100% seguro procesar
    pthread_mutex_unlock(&m);
    ```
    

#### 3. Proteger Todo Acceso Compartido (Lectura y Escritura)

- Cualquier variable global o estructura compartida (como un puntero a una cola de trabajos `job_queue`) debe ser tratada como una **sección crítica**.
- No basta con bloquear al escribir en ella; **las lecturas también deben estar protegidas** por el mismo mutex. El bloqueo debe ocurrir antes de evaluar la variable y el desbloqueo solo después de haber terminado de operar con ella.

#### 4. Evitar Interbloqueos (Deadlocks) Ordenando Recursos

- Si tus hilos necesitan adquirir más de un recurso (por ejemplo, `Mutex_1` y `Mutex_2`) para realizar una operación, asegúrate de que **todos los hilos del programa soliciten y bloqueen los recursos siempre exactamente en el mismo orden**.
- Si el Hilo A toma el Mutex 1 y luego el Mutex 2, pero el Hilo B toma el Mutex 2 y luego el Mutex 1, se bloquearán mutuamente de forma irreversible bajo ciertas planificaciones.

#### 5. Evitar la Espera Activa (_Busy Waiting_)

- Nunca pongas a un hilo a consultar constantemente una variable global en un bucle vacío (`while(!bandera);`). Esto consume el 100% de la CPU de forma ineficiente. Usa siempre un **semáforo** o una **variable de condición** para suspender el hilo de forma pasiva y dejar libre la CPU para otros hilos o procesos del sistema.