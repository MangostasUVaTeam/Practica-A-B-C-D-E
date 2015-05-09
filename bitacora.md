
# Bitácora Prácticas A+B+C+D+E + Práctica Abierta: 
## Grupo 46
###### Sergio García Prado
###### Adrián Calvo Rojo

## **Práctica A**
### 9 de Febrero de 2015

Hemos empezado a hacer la primera práctica, que consiste en instalar Minix en VirtualBox. Nos hemos descargado los archivos y estamos configurando la maquina virtual. Hemos añadido un nuevo controlador de disquete y también hemos cambiado el orden de arranque para que este lo haga primero. También hemos desactivado el arranque por CD. 
    
Procedemos a arrancar la maquina... Pide un login. Nos sentimos confusos. Hemos descubierto que lo que había que hacer es acceder como "root". Hemos descubierto que tecleando ``alt + Fi`` se cambia entre los distintos terminales.
 
#### Tutorial Sobre Vim:
    - cursores	  : para moverse
    - escape	  : para usar comandos
    - i  		  : Insertar en la linea.
    - u  		  : Añadir en la linea.
    - x 		  : Sirve para borrar.
    - :w          : sirve para guardar.
    - :q		  : Sirve para salir.
    - a 		  : Escribir despues del cursor.
    - nyy         : Copia n lineas desde el cursor.
    - p           : pegar
    - %           : cambia el cursor entre la pareja del corchete.
    - ndd         : Borra n lineas desde el cursor
    - u           : Deshacer
    - !           : Forzar comando
    - set nu      : Muestra el número de líneas
    - /[texto]    : Busca [texto] dentro del fichero
    - :100        : Te lleva hasta la linea 100
		
Como se indica en el guón de la practica, hemos creado una copia de la imagen de Minix y la de disquete. Tras esto hemos cambiado el UUID con el siguiente comando:

``VBoxManage internalcommands sethduuid minixCopia.vdi``

#### Lenguaje de comandos en shell. 
El ejemplo del profesor será crear un comando llamado "creaH" que lo que hará será crear un  numero n de hilos que se le pase por parámetro.

``$ ./creaH 5``

Utilizaremos la función "errno"

El codigo será el siguiente:

```java
    #include <errno.h>
    main(int argc, char* argv[]){
        int numH, i, ret;
        if (argc != 2){
            printf("creah,  <numHijos>\n");
            exit(-1);
        }
        numh = atoi(argv[1]);
        for (i = 0 ; i < numH; i++){
            if ((ret = fork()) == 0){ /* hijo */
                hijo();
                exit(0);
            } else  if (ret < 0){
                /*error en fork */
                perror("fork");
                printf("Creados %d primos\n", i);
                exit(-1);
            }
        }
    }
```

### 16 de Febrero de 2015

#### Creando copia de Minix:
Vamos a proceder a realizar la copia de Minix, para ello tenemos que modificar el identificador interno (UUID) del nuevo fichero vdi. Los nuevos ficheros se llamarán ``kernel-2.0.0 copia.img`` y ``minix2.0.0-copia.vdi`` por lo que para cambiar el identificador utilizamos el siguiente comando:
    ``VBoxManage internalcommands sethduuid minix2.0.0-copia.vdi``
Una vez hecho esto procedemos a crear la segunda máquina virtual en VirtualBox. 

#### Cambiando el shell:
Tras comprobar que esta funciona correctamente, nos disponemos a cambiar el shell por defecto por el ``ash``. Para ello tenemos que modificar el fichero ``/etc/passwd``. Para hacer esto tan solo hay que modificar la linea donde se indica que se está usando ``sh`` por ``ash``. Gracias al nuevo shell, ya funciona correctamente la función autocompletar al pulsar el tabulador.

#### Creando un nuevo usuario:
Lo primero que haremos será crear el directorio para este nuevo usuario. Lo hemos creado en ``/home/usuario``. Lo siguiente que hemos hecho ha sido entrar en el fichero ``passwd`` y hemos creado el usuario. Le hemos asignado el nombre **usuario** , le hemos asignado el Id de usuario 9 y el de grupo 4. También le hemos asignado su directorio y por último el shell ``ash``. Por último le hemos asignado una contraseña con el comando ``passwd``. La contraseña que se le ha asignado es **pass**.

Tras Comprobar que todo funciona adecuadamente accediendo al usuario y comprobando que su directorio home está en el lugar adecuado con el comando ``pwd`` reiniciamos la máquina y seguidamente la hemos apagado como indica el guión de la práctica.

#### Compilación del núcleo
Lo primero que hemos hecho ha sido modificar el mensaje de bienvenida de la máquina modificando la función ``tty_task()`` del fichero ``/usr/src/kernel/tty.c`` en la que hemos añadido nuestros nombres. Ahora compilaremos el nucleo.

Para verificar que la compilación del nucleo se haya realizado correctamente vemos el contenido del fichero ``/usr/stc/tools/revision`` que en este caso es **11**. Después de la compilación este debería incrementarse. Procedemos a aplicar el comando ``make hdboot`` para compilar. Tras unos problemas menores debido a que al modificar el mensaje de bienvenida borramos por error una de las comillas, que ya han sido solucionados, se ha compilado correctamente creandose una copia en ``/minix`` y incrementandose a 12 el contenido del fichero ``revision``.

Tras reiniciar la máquina vimos que no había cambiado nada. Esto es debido a que seguiamos ejecutando la imagen que esta en el disquete por lo que procedemos a apagarla y cambiar el orden de arranque en la configuración de VirtualBox. Ahora si, la versión que corre la máquina es la compilada por nosotros y se muestra el mensaje que habíamos modificado.


### 19 de Febrero de 2015

#### Programación
Hemos creado la carpeta ``/root/PracticaA`` que usaremos para trabajar en la parte de programación de esta práctica. Para hacer el programa, hemos creado el fichero ``creaHilos.c``. Tras utilizar un código muy similar al que nos mostró el profesor en clase vemos que funciona correctamente pero nos salen algunos "warnings" que vamos a intentar solventar. Las causas de esto era que simplemente faltaban ``includes`` y que en la función hijo no pusimos ``void`` como parametro en la función, es decir, no pusimos ``void hijo(void);`` Tras tener dificultades debido a que en la función hijo pusimos un ``printf`` no veiamos la linea donde ``fork()`` devolvia -1, así que estuvimos un buen rato buscando el motivo hasta que se nos ocurrio probar quitando el mensaje, por si había aparecido algo entre medias, y efectivamente, no inidica que el numero máximo de hilos creado es **27**.

## **Práctica B**
Durante estos días hemos pensado que lo mejor sería leer por separado el todo el código al que se hace referencia en el guión de la práctica, y en el caso de que nos surgiesen dudas resolverlas juntos. Esto es debido a que creemos que para poder entenderlo necesitamos mucha concentración, y esto se hace mejor en solitario. Cuando hayamos comprendido bien como funciona nos pondremos a realizar en común la parte practica de este apartado.

### 22 de Febrero de 2015

Las variables en mayúscula se refieren a constantes alfanuméricas.

Siguiendo el guión vemos que lo único que hace el fichero ``_fork.c`` es llamar a la función ``_syscall()`` con los parámetros apropiados, el primero indica qué tipo de operación se va a realizar, en este caso (MM) que se refiere a *Administración de Memoria*. el segundo sirve para identificar la operación, en este caso (FORK) y el último es la direción a una variable de tipo (message) declarada en el fichero ``_fork.c``. En la función ``_syscall()`` se llama a la función ``_sendrec`` a la cual se le envia como parámetros ``who`` (MM) y ``message`` despues de haber definido su ``m_type`` como la constante (FORK) en este caso. El resto de esta función sirve para tratar el resultado devuelto por ``sendrec``.

Tras revisar la función ``_taskcall`` vemos que realiza la misma función pero con un tratamiento de excepciones diferente (devuelve los errores negativamente y no utiliza ``errno``), alegando que es mejor para las operaciones de tipo MM (*Administración de Memoria*) y FS (*Sistema de Ficheros*).

Por tanto pasamos a analizar la función ``_sendrec``. Lo primero que nos llama la atención de este fichero (i386/rts/_sendrec.s)es que es dependiente de cada arquitectura. Esto es devido a que está escrito en ensamblador. Lo que hace la función __senrec es mover los parámetros que habiamos enviado a esta función de memoria a registros. Tras esto es cuando se produce la interrupción software, es decir ``int SYSVEC`` El argumento que se le pasa es ``33``, que es el valor de la variable alfanumérica SYSVEC. Es en este punto a partir del cual se entra en *Modo Privilegiado*.

Ahora como indica el guión de la práctica nos dirigimos a ``src/kernel/protect.c`` que contiene el código para la inicialización del modo protegido. Una de las cosas que llaman la atención de este fichero es que tiene etiquetas condicionales. Estas son etiquetas para el compilador y se utilizan para que dependiendo de distintos parámetros como por ejemplo la arquitectura el compilador decida si saltarse o no ciertos trozos de código.

Nos dirigimos a la función ``prot_init()`` y empezamos a leer su código. Lo primero que hace es configurar las tablas para el modo protegido. Ahora vemos ``SYS386_VECTOR`` que está declarado en ``const.h``. Esta vector está definido como 33.

Ahora seguiremos la pista de la función ``s_call()`` como indica el guión de la práctica.

### 25 de Febrero de 2015

Antes de seguir la pista a la función ``s_call()``, hemos decidido mirar mejor el vector  ``SYS386_VECTOR``. Dentro del archivo ``const.h``, en la línea 43 esta dicho vector con el número **33** que es el mismo que tenia ``SYSVEC``. Ahora volvemos a la función ``s_call()`` escrita en ensamblador. Esta función aparentemente crea una pila con los argumentos para despues, con ellos, llamar a otra funcion: ``sys_call()``.

La función ``sys_call()``, dentro del fichero ``/usr/src/kernel/proc.c`` recibe 3 argumentos: el primero puede ser 1 = **SEND**, 2 = **RECEIVE**, 3 = **BOTH** en función de como se quiera usar; el segundo argumento es la fuente a la que se quiere enviar o desde la que se va a recibir el mensaje; el tercero es un puntero al mensaje creado previamente en ``fork()``. En función de los argumentos que haya recibido envía y/o recibe el mensaje con las llamadas a las funciones mini_send() y mini_rec().

Hemos mirado ``proc.h`` y creemos que la variable ``proc`` es la que indica qué marcos de memoria forman parte del proceso...

Acabamos de darnos cuenta de cómo funciona. Las funciones anteriores (``mini_send()`` y ``mini_rec()``) laman a una funcion, ``CopyMess()`` que a su vez llama a otra, ``cp_mess``. Esta se encarga de convertir una dirección virtual en una direción física y de colocar en la tabla de vectores (TVI) la función que se va a ejecutar que en este caso es ``fork``. Como incluye el fichero ``/usr/include/minix/com.h``, que posee todas las funciones como constantes, pone en la tabla la funcion ``fork``.

``usr/src/kernel/mpx386s.s`` es el fichero encargado de guardar la mayoría del estado de la máquina cuando hay una interrupción y una ve tratada recuperar ese estado.

``table.c`` se encarga de redireccionar, en funcion de la tabla, al metodo correspondiente, en este caso a  ``sys_task``  que esta en ``system.c`` y esta invoca a ``do_fork`` que es el metodo llamado al inicio. Por lo tanto, modificando este metodo podemos poner el mensaje.

### 2 de Marzo de 2015

El último párrafo del día anterior no es correcto. El código encargado de recibir los mensajes de tipo MM (memory manage) se encuentra en el directorio ``/usr/src/mm`` y la función que se encarga de estar esperando su llegada es el ``main()`` del fichero ``main.c``, que una vez inicializa el manejador de memoria, entra en un bucle a la espera de mensajes de tipo MM. En la función ``get_work()`` es donde se extrae el ``mm_call``(numero de identificación de la llama). Tras esto se llama a la función correspondiente mediante un tabla alojada en ``table.c`` que contiene una estructura de datos llamada ``call_vec[]``. Este array llama a la función corresponiente según el identificador enviado, en nuestro caso el **2**. Entonces se llama a la función ``do_fork()`` alojada en ``/usr/src/minix/forkexit.c``. 

El parámetro que recibe esta función es el puntero del proceso padre que vamos al cual vamos a hacer fork. Lo primero que hace es comprobar si hay sitio en la tabla de procesos ``mproc``. Despues se calcula el tamaño del proceso hijo, que será solo para el segmento de datos y pila. Una vez hecho esto se modifica la estructura ``hole_head`` que se encarga de almacenar los espacios libres en memoria. Lo siguiente que sucede es que mediante la función ``sys_copy()`` que está alojada en el fichero ``/usr/src/lib/syslib`` se copia el espacio de memoria del padre al hijo. Para hacer esto se envia otro mensaje que invoca la función ``do_copy`` alojada en ``/usr/src/kernel/system.c``. Despues buscamos una entrada libre en la tabla de procesos y se copian los valores del proceso padre en el hijo para despues personalizarlos con las características propias del hijo. Para finalizar la operación se notifica al servidor de ficheros y al nucleo sobre la creación de un nuevo proceso.

Una vez completado todo esto se ejecuta la función ``_restart()`` alojada en ``/usr/src/kernel/mpx386.s/`` que termina el modo privilegiado y vuelve al modo usuario para seguir con normalidad el transcurso del código que ejecutó la función ``fork()``. Para comprobar que este es el camino correcto hemos añadido un sentencia ``printf("Soy un fork");`` en la rutina do_fork() antes mencionada y tras recompilar el nucleo hemos visto que funciona correctamente y se muestra cada vez que se ejecuta un fork en el sistema. Tras comprobar que funciona de manera apropiada comentamos la linea y recompilamos para que no moleste el mensaje.

### 4 de Marzo de 2015

## **Práctica C**

Lo primero que haremos para empezar empezar la práctica es ir al fichero ``/usr/include/minix/callnr.h`` para añadir la nueva instrución que se llama ``ASOPS`` a la cual definimos el valor *77*, por lo que tenemos que incrementar ``NCALLS`` (número de llamadas al sistema) y dejarlo con el valor *78*.

En la función ``main()`` alojada en ``/usr/src/mm/main.c/`` se encuentra el bucle que gestiona la llamadas que son enviadas al *Manejador de Memoria*. Para saber qué llamada es la que se ha realizado se accede a ``call_vec[mm_call]`` que es el vector de punteros que contiene las llamadas a las funciones que se tendrán que ejecutar en cada caso. Por lo tanto, nosotros tenemos que añadir a esta lista de vectores alojada en ``/usr/src/mm/table.c``. Hemos decidido llamarla ``do_asops()``. También hay que añadirla en ``/usr/src/mm/proto.h``, es decir, tenemos que crear el prototipo de la función. Para ello añadimos la línea: ``_PROTOTYPE(int do_asops, (void)   );``.

Como se indica en el guión de la práctica, alojaremos el código de esta función en ``/usr/src/mm/utility.c`` que hemos extraido del guión.

Seguidamente en el fichero ``/usr/src/kernel/system.c`` hemos añadido el prototipo de la función ``do_asops(message *m_ptr)``. Tambien añadimos un nuevo *case* al switch que está en la función ``sys_task()`` con la variable alfanumérica ``ASOPS``, y que llama a la función ``do_asops(&m)``. Nos damos cuenta de que el resto de casos son por ejemplo ``SYS_FORK`` y investigado acerca de ello nos damos cuenta de que es por que en el método ``_taskcall`` el segundo parámetro es ``SYS_FORK`` pero en nuestro caso el parámetro pasado es ``ASOPS``.

A continuación nos hemos dispuesto a escribir la función ``do_asops()`` que  lo que hará será mostrar por pantalla el contenido del mensaje y sumar ``m1_i1+m1_i2``  y guardarlo en ``m1_i3``.

ANOTACION: bill_ptr-> variable que tiene el pid del proceso que realizo la llamada al sistema.

Una vez realizado esto hemos compilado el núcleo y ahora empezaremos el programa de prueba ``llamasis.c``. 

### 7 de Marzo de 2015

Vamos a comenzar a hacer el fichero ``llamasis.c``; importamos las librerias ``lib.h``, ``sys/types.h`` y ``unistd.h``. Hemos creado un metodo main muy simple con una variable de tipo mensaje ``msg`` al que le damos dos atributos ``m1_i1``y ``m1_i2``. Luego llamamos a la función ``_taskcall()`` y vemos que, usando la función creada anteriormente ``ASOPS``, lo envia al sistema correctamente y posteriormente imprimimos las variables modificadas correctamente desde el núcleo.

Segunda Parte:

Vamos a crear ahora nuestra propia llamada, con el nombre ``CUSTOMCALL``. Empezamos modificando el fichero ``/usr/include/minix/callnr.h``. Para ello incrementamos la variable ``NCALLS`` en 1 y definimos nuestra función ``CUSTOMCALL`` con el número **78**. Desde la función ``main()`` se va a llamar a nuestra función. Es llamada a traves del vector de llamadas a funciones``call_vec[mm_call]``, para ello añadimos la llamada en ``/usr/src/mm/table.c``. En ``/usr/src/mm/proto.h`` definimos nuestro prototipo de la función. Ahora en ``/usr/src/mm/utility.c`` creamos la función ``do_customcall()`` que va a reenviar el mensaje a la tarea del sistema situada en el nivel 2.
Creamos una funcion similar a la anterior pero esta vez solo redirigimos el mensaje con la linea ``_taskcall(SYSTASK, CUSTOMCALL, &mm_in);``. Todas las funciones creadas estan documentadas correctamente en fichero con la misma nomenclatura y estilo que las funciones de minix.

Nos movemos a ``/usr/src/kernel/system.c`` creamos el prototipo de la función ``do_customcall`` y la introducimos al final del switch para que pueda ser llamada. Despues en el mismo fichero, debajo de ``do_asops`` creamos nuestra función. Va a tener un parametro de entrada mediante y un switch se eligira la tarea que se va a realizar. Su funcionamiento será:

    - En caso de enviar un 1 mostrará el PID del proceso (pero esto lo programaremos más adelante).
    - Un 0 devuelve "Prueba de llamada personalizada"
    - Si no se envian argumentos o se envia mas de uno, nos pide que introduzcamos uno.

Una vez creado todo lo necesario para hacer la llamada al sistema, en la carpeta ``root/PracticaC`` creamos un fichero en C para probar esta funcion llamado ``customcall.c``. Probamos a compilar el núcleo para comprobar que no sale ningun error. Ha aparecido un error. Volvemos a ``/usr/src/kernel/system.c`` y parece que se nos ha olvidado poner el ``return(OK)``. Tambien en la funcion ``do_customcall`` habiamos puesto un salto del linea en un String (parece que en C no se puede, al contrario que en Java). Al compilar de nuevo el núcleo, compila correctamente. Tras comprobar que todo funciona como debe, volvemos a ``/usr/src/kernel/system.c`` para acabar de programar la función que hará que nos muestre el PID del proceso. 

Creamos una nueva función llamada ``muestra_pid``. Con la variable ``bill_ptr`` obtenemos el PID del proceso que realizo la llamada al sistema y lo mostramos. Compilamos otra vez el núcleo sin problemas. Ahora reiniciamos el sistema y volvemos a nuestro programa en C, ``/root/PracticaC/customcall.c`` y lo compilamos. Lo ejecutamos, le damos el parametro 1 y aparece una cadena de numeros sospechosamente larga, por lo que la la variable ``bill_ptr`` contiene más datos (los que imprime todos) de los que necesitamos (por lo que imprime todos)  al tratarse de un ``struct``. Entonces volvemos a la función que creamos para mostrar el PID y en vez de imprimir todo el ``bill_ptr``, imprimimos ``bill_ptr->p_pid``. Ahora todo funciona correctamente.

### 11 de Marzo de 2015
## **Práctica D**

En el fichero ``/src/kernel/clock.c`` está la rutina de servicio de interrupción del reloj. En las colas ``Rdy_head``y ``Rdy_tail`` se alojan los procesos en espera de ejecución. Estas colas aparecen en tres niveles, en el de servicios y en el de tareas son colas de tipo ``FIFO`` (*First in, First out*), mientras que la última está en el nivel de usuario y está gestionada mediante un algoritmo ``Robin Round``. 

Las cuatro funciones más importantes de Minix para la gestión de procesos son las siguientes:

    - sched()
    - ready()
    - unready()
    - pick_proc()

Hay que buscar la porción de código que hace que el último proceso creado es el primero creado.

#### Problema

Debido a un problema con ``git`` al hacer ``commit`` hemos perdido la segunda parte de la práctica C, pero como anotamos todos los pasos en la bitacora tan solo tendremos que repetir los pasos que hemos escrito en ella. ``NCALLS``tendrá el valor **79** con la nueva llamada ``CUSTOMCALL``.

### 18 de Marzo de 2015

Ahora si, vamos a continuar con la *Práctica D*, para ello primero no vamos a dirigir al directorio ``/usr/src/kernel/``. 

Una vez aquí entramos en el fichero ``proc.c``. Lo primero que nos llama la atención al ver la función ``pick_proc()`` es la variable ``NIL_PROC`` que tras ver que está definida en ``proc.h`` descubrimos que es un casting de *(0)* a *(struct proc *)*, lo que creemos que sería algo así como una referencia nula. La función de ``pick_proc()`` es seleccionar el siguiente proceso que se ejecutará. Para ello se apoya en la variable ``rdy_head[]``, que es un array que contiene punteros a la cabeza de cada una de las colas de cada nivel (``TASK_Q``,``SERVER_Q``,``USER_Q``). Lo que se hace con ellas es ver que no son nulas. Se va comprobando según el orden de prioridad empezando por ``TASK_Q``. Con esto se consigue que se ejecuten siempre primero los procesos en la cola de tareas, luego de servidores, y seguidamente los procesos de usuario. En el caso de que no haya nada en ninguna de estas colas se ejecuta una tarea vacía ``ÌDLE``.

Seguidamente vemos la función ``ready()`` es añadir un proceso listo a su correspondiente cola. Para ello  primero obtiene el tipo de proceso que es para saber en cual de las colas lo deberá introducir. Si la correspondiente cola está vacía lo colocara en la posición head, es decir, en la cabeza. Si no es así, lo insertará en la cola. Como en el anterior método, primero se comprueba la cola de tareas, luego la de servidores y por último la de usuarios. Nos llama la atención el código que se encuentra entre un if que se ejecuta si la variable alfanumérica ``SHADOWING`` tiene el valor *1*. Esto activa el uso de una cola adiccional llamada ``SHADOW_Q``.


La función ``unready()`` sirve para desalojar el proceso de la cola de listos, es decir, según la cola a la que pertenezca (``TASK_Q``,``SERVER_Q`` o ``USER_Q``) da a ``rdy_head[cola]`` el valor del siguiente proceso en dicha cola a través de ``p_nextready``, que es un campo del ``struct proc rp``. Seguidamente llama a ``pick_proc()`` para elegir el siguiente proceso que se ejecutará. En el caso de que el proceso que se quiere extraer de la cola de listos se busca en el cuerpo de la cola y se sustituye por el siguiente listo despues de esto, es decir, se expulsa.

La función ``sched()`` sirve únicamente para procesos de usuario y lo que hace es poner el proceso que está en la cabeza de listos en la cola y el siguiendte de este en la primera posición de la cola. Esta función es llamada cuando un proceso está usando durante mucho tiempo la cpu. Para los procesos de usuario que están en la cola se puedan ejecutar, se pasa el proceso que tarda mucho a la cola de la cola y así se pueden ejecutar los procesos que estaban esperando por él. Seguidamente se llama a ``pick_proc()`` para elegir el proceso que se ejecutará.

Despues de haber comprendido como funcionaban estas funciones y la utildad que tienen accederemos a ``clock.c``. La primera función que revisaremos será ``init_clock()``. Hace que el reloj funcione de manera continua, carga el tiempo en la canal 0 de tiempo, selecciona el capturador de interrupciones del reloj ``clock_handler` y lo activa. Este método aparece varias veces por que dependiendo del chipset del computador la forma en que se inicializa es diferente. Para lograr esto se utilizan estructuras condicionales precedidas de **#** para que el compilador elija el fragmento de código a ejecutar según el tipo chip.

### 31 de Marzo de 2015

Vamos a crear la aplicación encargada de crear hilos ligeros y pesados para a poder ver la diferencia entre una planificación de procesos mediante un algoritmo **FIFO** y uno **Robin Round**. La aplicación tan solo recibirá un valor **n** por linea de comandos, y creara **n-1** procesos ligeros y uno pesado. Nos referiremos como procesos ligeros a los que tardan 1 segundo aproximadamente y proceso pesado al que tardará unos diez. Para producir una carga de trabajo equivalente hemos hecho un generador de numeros primos en un rango indicado. Los procesos ligeros calculan los números primos entre 100 y 10000 mientras que el pesado lo hace entre 300 y 40000.

La variable ``SCHED_RATE`` es la que almacena el tiempo de ejecución de cada proceso de usuario en el procesador. Su valor es 6, ya que la variable ``HZ``alojada en ``const.h`` tiene el valor **60**. Debido a esto el cuantum asignado a cada proceso de usuario es de *100 milisegundos*.

La siguiente función que vamos a ver será ``clock_handler())``. Esta función es la encargada de "manejar" el reloj, es decir, ir incrementando el tiempo de los procesos del sistema y en el caso de que el tiempo del cuantum se haya terminado. generar una interrupción. Para ello lo primero que hace es dar el valor a ``ticks`` (una variable local) de los ticks perdidos + 1, es decir, el tick actual. Seguidamente se le da a la variable local ``now`` de tipo ``clock_t`` el valor de ``realtime + pending_ticks``. La variable realtime no es actualizada aquí ya que se necesita proteger el sistema, es por ello que esta se actualiza desde ``clock_task``. Este valor es el que se utilizará para comprobar si es necesario que se produzca una interrupción, bien sea por que ``next_alarm <= now`` o por que al proceso se le va a termianr el cuantum. Esto se consigue gracias a la variable ``sched_ticks`` que representa el número restante de ticks que quedan para que se termine el cuantum. Si esta variable llega a cero se vuelve a reiniciar el quantum.

Ahora explicaremos el funcionamiento de ``clock_task()``. Este es el código encargado de gestionar las tareas del reloj. Lo que hace es iniciar el reloj y seguidamente entra en un bucle que se encarga de actualizar la variable ``realtime`` antes citada de forma protegida gracias a las funciones ``lock()`` y ``unlock()``, que están definidas en el fichero ``sunsighandle.c``. Su segunda función es actuar como un distribuidor de funciones de interrupciones relacionadas con el reloj. Nos centraremos en la función ``do_clocktick()`` que es invocada cuando el opcode de es igual a ``HARD_INT``. Luego explicaremos cómo se realiza esta llamada a partir ``interrupt()``.

La función ``do_clocktick()`` a pesar de su nombre no se ejecuta en cada tick. Lo que hace este código es comprobar si se ha producido una alarma y en caso de que haya ocurrido tratarla y tambien se encarga de invocar a la función ``sched()``, que como antes comentabamos se encarga de reorganizar la cola de procesos a la vez que se reinicia la variable ``sched_ticks``.

La proxima función que comentaremos está en el fichero ``proc.c`` y se llama ``interrupt()``. Es invocada cuando el cuantum ha finalizado. En el caso de que ``k_reentrer `` sea distinto de 0 o switching se añade el proceso de usuario a una cola de retenidos. En el caso de que el proceso esté esperando por alguna interrupción se bloquea. Seguidamente se manda el mensaje ``HARD_INT`` citado anteriormente y por último se coloca ``rp`` en la cola de tareas.

### 2 de Abril de 2015

Vamos a crear el comando cambiaQ que será el encargado de modificar el cuantum del sistema. Con esto lograremos que el algoritmo **RR** utilizado en el nivel de usuario por minix parezca un cola **FIFO**. Para realizar esto nos vamos a apoyar en nuestra llamada personalizada al sistema que creamos en la Practica C a la cual denominamos *CUSTOMCALL*. 

Lo primero que hemos hecho ha sido reaprobechar la función creada para la practica anterior, para ello hemos copiado el fichero C en el directorio que estamos usando para esta práctica y seguidamente le adaptaremos para que el valor de ``m1_i1`` sea constante, es decir, distribuya la llamada hacia el manejador del reloj, y el campo ``m1_i2`` sera el valor que le daremos al cuantum, que por defecto es **6**. todas las funciones del distribuidor son de tipo *int* para que en el caso de que algo salga mal se pueda notificar del error.

Una vez comprobado que el distribuidor está bien codificado, definimos la funcion ``int cambiaQ(message m)`` en el fichero ``system.c``, que mandará otro mensaje al gestor de interrupciones del reloj alojado en ``clock.c``, en concreto es la función ``clock_task()`` donde se llamará a la función que después implementaremos que será quien cambie el cuantum.

Tras visualizar el código y repasar bien como se ha llegado hasta ``system.c``, es decir, el nivel de tareas, nos damos cuenta de que el mensaje es mandado a ``SYSTASK``, por lo cual decidimos investigar donde está definida esta constante. Gracias al comando ``grep -n "lo que sea" *`` llegamos al ficher ``/usr/include/minix/com.h`` y gracias a esto, y como sabemos que el reloj es una tarea, descubrimos que para enviar mensajes a la funcion ``clock_task()`` tan solo hay que realizar la misma operación que con ``SYSTASK`` pero esta vez utilizando ``CLOCK``. Al ver que las constantes alfanuméricas utilizadas en este distribuidor están definidas en este fichero (``com.h``) definimos también aquí ``CAMBIAQ`` a la cual hemos decidido asignarle el valor **7**.

Utilizando esta estrategia, que en un principio nos pareció la mejor forma la llamada llega a ``do_cambiaQ()`` pero sin embargo el contenido del mensaje se modifica, por lo cual no podemos hacer que el valor que queremos asignarle al cuantum llegue alli.

La estrategia que vamos a utilizar ahora será llamar directamente a la función desde system.c, que estamos casi seguros de que funcionará. Efectivamente llamando directamente a la función ``do_cambiaQ()`` desde ``system.c`` funciona correctamente. Hemos tenido que modificar la estructura de esta ya que en un principio teniamos pensado que recibiera un ``message`` pero con la remodelación recibe un ``int``. Lo único que hace esta función es dar a la variable ``sched_ticks``, que por defecto es **6** el valor que reciba en la entrada. Para que todo funcione correctamente al hacer el prototipo de la misma en el fichero ``system.c`` hay que prescindir de la "etiqueta" ``FORWARD`` que como está definido en el fichero ``/usr/include/minix/const.h`` hace a la función ``static``, es decir, que la función solo sea accesible desde el fichero en donde está declarada, es por por ello que hemos tenido que eliminar esta etiqueta.

Una vez comprobado que todo funciona correctamente expliquemos los resultados: 

    - Para valores de Q pequeños la planificación de procesos de usuario actua con un algoritmo RR, es decir, por divisiones temporales para cada proceso. Por lo cual, a pesar de que en nuestra prueba el proceso pesado sea el primero en empezar la ejecución, es el que más tarde termina.
    - Para valores de Q grandes la planificación de procesos de usuario actua como una cola FIFO, es decir, hasta que no termina el proceso que está en ejecución no comienza el siguiente. En el caso de nuestra prueba todos los procesos ligeros, que tienen una duración en la cpu pequeña tiene que esperar a que termine el pesado para poder ejecutarse, es decir, se produce un efecto comboy.

### 3 de Abril de 2015
## **Práctica E**

Para esta práctica primero haremos todas las llamadas al sistema necesarias y después se implementará el código a nivel de usuario. Nos apoyaremos en nuestra función ``CUSTOMCALL``.

Lo primero que hemos hecho ha sido añadir las correspondientes variables alfanuméricas que necesitaremos en el fichero ``/usr/src/include/minix/callnr.h``, que luego utilizaremos en los distintos distribuidores en los que nos apoyaremos.

Una vez hecho esto vamos al fichero ``/usr/src/mm/utility.c/`` y modificamos la función ``do_customcall()``, que era la que usabamos para redirigir las llamadas de memoria al sistema. Lo que haremos aquí será un distribuidor que este compuesto por los siguientes casos:

    - ``PRINT_HOLE_HEAD``
    - ``PRINT_TASKS_TABLES``
    - ``PRINT_USERS_TABLES``
    - ``PRINT_PROC_TABLES``
    - ``default``: redirige la llamada al sistema, es decir, al distribuidor alli alojado.

Hemos decidido crear un campo para las tareas, para que seá más claro de ver y llamar desde ``mm_init``,  aunque luego haya que redirigirlas al sistema, por que estas no se encuentran en ``mproc[NR_PROCS]``, ya que como indica su propia declaración tan solo hay procesos de usuario.

Comenzamos con la función que imprimirá ``hole_head``. Está función la hemos alojado en ``/usr/src/mm/alloc.c``, que es donde está definido tanto esta, como el ``struct hole``. lo que hace la función ``print_hole_head()`` es "iterar" ``hole_head`` a través de ``->h_next`` hasta que encuentra un agujero nulo (``NIL_HOLE``) y en cada uno de los elementos llama a ``print_hole()`` que imprime en pantalla la base y el tamaño de este. Estos són los dos campos más que tiene el struct ``hole`` junto con ``h_next``. ``hole`` representa los *agujeros libres* en la memoria física.

Seguidamente llamamos a esta función en ``/usr/src/mm/main.c:mm_init`` y tras comprobar que todo funciona correctamente después de compilar y reiniciar (aparecen dos agujeros) continuaremos con las tareas del sistema.

Volvemos al distribuidor alojado en ``/usr/src/mm/utility.c:do_customcall()``  y llamamos a ``print_tasks_seg_tab()``. En este caso como estos procesos no están en la el array ``mproc[]`` por lo mencionado anteriormente tenemos que redirigir la llamada al sistema, a traves de ``CUSTOMCALL`` para acceder a ``pproc_addr[NR_TASKS + NR_PROCS]``, que es un array de tipo ``proc`` que si que contiene todos los procesos. Para ello añadimos un campo más al distribuidor del kernel ``/usr/src/kernel/system.c:do_customcall()`` que llame a la función ``print_seg_tab()`` que hemos alojado en ``/usr/src/kernel/proc.c``. Esta función reocorre ``pproc_addr`` desde el inicio hasta el ``NR_TASK`` y llama a ``print_seg_tables(struct proc *act_proc)`` que se encarga de recorrer ``p_map[NR_SEGS]``, es decir, las tablas de segmentos y llama a ``print_seg`` que es la encargada de imprimir cada uno de los segmentos con los siguientes campos:

    - Direccion virtual.
    - Direccion física.
    - Tamaño.

Seguidamente llamamos a la funcion ``/usr/src/mm/utility.c:print_tasks_seg_tab()`` desde ``/usr/src/mm/main.c:mm_init`` y tras comprobar que todo funciona comprovamos que todas las tareas de sistema comparten los mismos segmentos. Esto es debido a que se presupone que el código de las tareas está bien diseñado y no hará cosas sospechosas por lo que no necesita medidas de protección como es el caso de los procesos de usuario.

Ahora vamos a hacer la parte que mostrará las tablas de segmentos de procesos de usuario. Ahora no necesitamos acceder al kernel para hacerlo, es decir, lo podemos hacer desde ``/usr/src/mm``. Para ello llamaremos a ``print_users_seg_table()`` que recorre la variable ``mproc[NR_PROCS]`` y va llamando a ``pr_seg_tab()`` pasandole el proceso en cada caso y a su vez esta por cada una de las tres segmentos de cada proceso alojados en ``mp_seg[NR_SEGS]`` llama a ``pr_seg()``  que imprime cada uno de ellos en pantalla. Como es natural, estás funciones son casi identicas a las que habíamos creado en el kernel para imprimir las tablas de segmentos de las tareas del sistema, con la diferencia de que antes usabamos el ``struct proc`` y ahora lo hacemos con ``struct mproc``. La diferencia entre estos, es que ``mproc`` es algo así como una versión "ligera" (con menos campos) de ``proc`` para hacer más eficiente la gestión de memoria.

Por último nos queda la llamada para que muestre tabla de segmentos del proceso actual, es decir, el que está referenciado en la variable ``mp``, que está declarada en ``/usr/src/mm/glo.h``. Seguimos el mismo procedimiento que en el los anteriores casos, es decir, redirigir desde ``do_systemcall()`` a ``print_proc_seg_table()`` que pasa llama directamente  ``pr_seg_table()`` pero esta vez enviandole ``mp``. El funcionamiento de ``pr_seg_table()`` está indicado anteriormente.

### 5 de Abril de 2015

Ahora que ya están todas las llamadas del sistema implementadas tan solo tenemos que hacer los comandos para utilizarlas. 

Primero creamos el comando ``memLibre`` que muestra los *agujeros libres* en memoria. Como vemos conforme se crean procesos se van apareciendo más agujeros en memoria.

Seguidamente hemos creado el comando que muestra tanto la lista ``hole_head`` como las tablas de segmentos de los procesos de usuario. Para ello creamos una llamada al sistema que combina dos de las creadas anteriormente en una sola. 

Por último creamos el comando para monitorizar el uso de ``fork``. Tras analizar los resultados comprobamos que al ejecutar ``fork()``, en el caso del padre la localización de las tablas de segmentos siguen en la el mismo lugar que al principio, pero en el caso del hijo, a pesar de que las direcciones virtuales se mantienen, las físicas han cambiado de posición en la memoria. Esto se debe a que al ejecutar ``fork()`` se crea un nuevo proceso lo que conlleva asignarle un nuevo espacio de memoria para él.


## **Práctica Abierta**

### 22 de Abril de 2015

En esta práctica nos vamos a centrar en el **Sistema de Ficheros** de *Minix*, que creemos que es una parte interesante, además coincide con los siguientes temas que vamos a estudiar en la parte teórica de la asignatura. Tenemos pensado primero entender el funcionamiento del **FS** para seguidamente una vez que comprendamos cómo funciona, hacer pequeñas variaciones en el código de algunas partes. 

La estrategia que seguiremos será analizar primero el fichero ``/usr/src/fs/main.c`` y a partir de ahí seguir describiendo el funcionamiento de las estructuras de datos que nos encontremos (**inode**, **filp**, **super_block**, etc).

Vamos a comenzar con ``/usr/src/fs/main.c``. La primera función que nos encontramos es ``main()`` que como indican los comentarios en su interior contiene el receptor de mensajes del servidor del sistema de ficheros. Antes de hacer esto se inicializa el **FS** en la función ``fs_init()``. Por lo cual primero estudiaremos esta funcion y luego seguiremos con el receptor de mensajes.

En esta función se inicializan las variables globales, tablas, etc. La primera estructura de datos que vemos aquí es **inode**. Esta Estructura está declarada en ``/usr/src/fs/inode.h``  y representa las características de un archivo, directorio, etc. Es decir, es la estructura que representa los atributos. Vemos que los campos de esta estructura tienen tipos definidos en ``/usr/include/sys/types.h``. Aquí comprobamos que a través de **typedef** se "encapsula" el tipo real de las variables. Vemos que por ejemplo ``mode_t``, que representa el tipo de fichero a la vez que los bits de permiso, es decir **RWX**; la variable **i_nlinks**, es un **char** que representa el numero de enlaces hardware y software que apuntan al fichero.


### 6 de Mayo de 2015
Benjamin: ""Si añadimos un campo al PCB, se inicializa en do_fork""

Continuamos en la función ``fs_init()``. Hace una llamada al método ``buf_pool()``, el cual declara una variable del tipo **buf**, definido en ``/usr/src/fs/buf.h``. Este tipo de estructura es la encargada de inicializar la cache de bloques del **FS**. Esta caché esta formada por varios **buf**. Cuando una rutina llama al método ``get_block()``, un bloque aumenta en 1 la variable ``b_count``, para dar a entender que esta en uso por ese proceso. Los bloques que no estan en uso, se ponen en una lista con el algoritmo LRU de forma que el primero en esta lista será el bloque que mas tiempo haya estado sin utilizarse. En caso de no haber bloques libres, el **FS** buscará en esta lista e ira cogiendo en orden los bloques que necesite.

Con estos bloques de buffer crea una lista doblemente enlazada que va desde el primero de los bloques, **front** (primera posicion fisica definida para el **FS**), hasta el último **rear** (direccion fisica obtenida desde la primera hasta ``NR_BUFS``, definida en ``usr/include/minix/config.h``. En cada uno de estos elementos se inicializan a nulo las variables ``b_blocknr`` (número de bloque en el dispisitivo) y ``b_dev`` (numero de dispisitivo donde se encuentra el bloque). También se enlazan los bloques por hash. Seguidamente se asignan al primer y al ultimo bloque de buffer, la variable ``NIL_BUF``, definida en ``/usr/src/fs/buf.h``, que indica la ausencia de bloque. De esta forma se indican al buffer del **FS** cuales son sus limites.

Despues de ``buf_pool()``, aparece ``get_boot_parameters()`` que hace una llamada al sistema,  enviandole un mensaje con el fin de preguntarle por los parametros del arranque.

La siguiente funcion a la que llama ``fs_init`` es ``load_ram()``. Al iniciar MINIX, el unico dispositivo que existe es el raíz, que posee el directorio raiz ``/``, en el que se encuentran todos los demás directorios. El sistema de ficheros raíz puede residir en diskete, en una partición del disco duro, o en disco RAM. Si el dispositivo raiz se indica que reside en disco RAM, deberá disponerse una copia, de tal forma que al finalizar la carga del sistema dicha copia ha de traslade al disco RAM. Esta copia se realiza bloque por bloque y el tamaño usado en la RAM es el mismo que el de la imagen del dispositivo raiz. La forma más común de ubicar esta imagen es en una partición del disco duro, ya que si por ejemplo copiamos MINIX desde un diskette al disco RAM, y modificamos en el disco RAM, estas modificaciones se perderán. En el caso de que el disco RAM no se haya definido como el dispositivo raíz, solamente asigna un disco RAM con los parametros de arranque, que se han cargado en la función anterior.

La ultima funcion llamada es ``load_super()``,

### 7 de Mayo de 2015

A partir de ahora vamos a pasar a la parte práctica de la tarea, para ello vamos a montar una llamada al *Sistema de Ficheros* de minix para realizar distintas acciones tal y como hicimos con las llamadas al *Sistema de Memoria*.

La llamada la determinaremos como ``FSCALL``. Lo primero que vamos a hacer es declararla en ``/usr/include/minix/callnr.h`` e incrementar el valor de ``NRCALLS``. Seguidamente añadimos una entrada en ``/usr/src/fs/table.c:call_vector[NCALLS]`` que es quién reenviará la llamada a una función del sistema de ficheros. A esta la vamos a llamar ``do_fscall`` y estará alojada en ``/usr/src/fs/utility.c``. Lo único que nos queda antes de crear la función será definir su prototipo en ``usr/src/fs/proto.h``.

Ahora que ya conocemos mejor la estructura de minix nos hemos dado cuenta de que aquí es donde se tienen que alojar todos los prototipos de las funciones a las cuales se puede acceder desde un fichero diferente, mientras que cuando el prototipo se define en el propio fichero este debe ir acompañadao de ``FORWARD`` que hace *static* la función, es decir, accesible unicamente desde el propio fichero.

Ahora ya solo nos queda hacer la función do_fscall que será un distribuidor semejante a ``do_asops`` de la práctica anterior. En este caso el mensaje en vez de llegar a partir de de ``mm_in`` como se hacía a través del sistema de memoria, está alojado en ``m``, que está definido en ``/usr/src/fs/glo.h``. 

### 8 de Mayo de 2015

Una vez comprobado que funciona correctamente seguiremos con la práctica. Lo que queremos hacer es mostrar la estructura ``super_block``, que está alojada en ``/usr/src/fs/super.h``.

Esta estructura es en la que se almacenan los campos correspondientes a los bloques del sistema de ficheros. Contiene el número total de bloques, el número de bloques libres, usados por inodes, el tamaño máximo de fichero que permite el sistema de ficheros...

Varios de estos campos tienen tipos definidos por los desarrolladores del sistema que sirven para encapsular el tipo real y que sea más facil de comprender. Para conocer el tipo real de estas variables hay que dirigirse a ``/usr/include/sys/types.h``.

Además de tener los campos citados anteriormente el superbloque tiene también el ``inode`` que representa la raiz del sistema de ficheros asi como otro ``inode`` al directorio donde se ha montado el sistema de archivos. Procedemos a realizar la función que imprimirá los campos. Esta se alojará en ``/usr/src/fs/super.c`` y la denominaremos ``print_super_list()``. Esta función recorre la lista de superbloques ``super_block[NR_SUPERS]`` y en cada caso llama a ``print_super()`` al que se le pasa como atributo un ``super_block`` y es aquí donde imprimiremos los campos:
    
    - *s_dev*           Indica a qué dispositivo se refiere.
    - *s_zones*         Número de zonas asignadas.
    - *s_max_size*      Tamaño máximo de fichero permitido.
    - *s_ninodes*        Numero de inodes disponibles.
    - *s_imap_blocks*   Número de bloques usados por el mapeo de inodos
    - *s_zmap_blocks*   Número de bloques usados por el mapeo de zonas.

