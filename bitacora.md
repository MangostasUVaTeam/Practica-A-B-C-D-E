
# Bitácora Prácticas A+B+C+D+E: 
#### Sergio García Prado
#### Adrián Calvo Rojo
#### Grupo 46

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

El parámetro que recibe esta función es el puntero del proceso padre que vamos al cual vamos a hacer fork. Lo primero que hace es comprobar si hay sitio en la tabla de procesos ``mproc``. Despues se calcula el tamaño del proceso hijo, que será solo para el segmento de datos y pila. Una vez hecho esto se reserva espacio en la estructura ``hole_head`` que se encarga de almacenar los espacios libres en memoria. Lo siguiente que sucede es que mediante la función ``sys_copy()`` que está alojada en el fichero ``/usr/src/lib/syslib`` se copia el espacio de memoria del padre al hijo. Para hacer esto se envia otro mensaje que invoca la función ``do_copy`` alojada en ``/usr/src/kernel/system.c``. Despues buscamos una entrada libre en la tabla de procesos y se copian los valores del proceso padre en el hijo para despues personalizarlos con las características propias del hijo. Para finalizar la operación se notifica al servidor de ficheros y al nucleo sobre la creación de un nuevo proceso.

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

### 18 de Marzo de 2015

Vamos a crear la aplicación encargada de crear hilos ligeros y pesados para a poder ver la diferencia entre una planificación de procesos mediante un algoritmo **FIFO** y uno **Robin Round**. La aplicación tan solo recibirá un valor **n** por linea de comandos, y creara **n-1** procesos ligeros y uno pesado. Nos referiremos como procesos ligeros a los que tardan 1 segundo aproximadamente y proceso pesado al que tardará unos diez. Para producir una carga de trabajo equivalente hemos hecho un generador de numeros primos en un rango indicado. Los procesos ligeros calculan los números primos entre 100 y 10000 mientras que el pesado lo hace entre 300 y 40000.

La variable ``SCHED_RATE`` es la que almacena el tiempo de ejecución de cada proceso de usuario en el procesador. Su valor es 6, ya que la variable ``HZ``alojada en ``const.h`` tiene el valor **60**. Debido a esto el cuantum asignado a cada proceso de usuario es de *100 milisegundos*.

La siguiente función que vamos a ver será ``clock_handler())``. Esta función es la encargada de "manejar" el reloj, es decir, ir incrementando el tiempo de los procesos del sistema y en el caso de que el tiempo del cuantum se haya terminado. generar una interrupción. Para ello lo primero que hace es dar el valor a ``ticks`` (una variable local) de los ticks perdidos + 1, es decir, el tick actual. Seguidamente se le da a la variable local ``now`` de tipo ``clock_t`` el valor de ``realtime + pending_ticks``. La variable realtime no es actualizada aquí ya que se necesita proteger el sistema, es por ello que esta se actualiza desde ``clock_task``. Este valor es el que se utilizará para comprobar si es necesario que se produzca una interrupción, bien sea por que ``next_alarm <= now`` o por que al proceso se le va a termianr el cuantum. Esto se consigue gracias a la variable ``sched_ticks`` que representa el número restante de ticks que quedan para que se termine el cuantum. Si esta variable llega a cero se vuelve a reiniciar el quantum.

Ahora explicaremos el funcionamiento de ``clock_task()``. Este es el código encargado de gestionar las tareas del reloj. Lo que hace es iniciar el reloj y seguidamente entra en un bucle que se encarga de actualizar la variable ``realtime`` antes citada de forma protegida gracias a las funciones ``lock()`` y ``unlock()``, que están definidas en el fichero ``sunsighandle.c``. Su segunda función es actuar como un distribuidor de funciones de interrupciones relacionadas con el reloj. Nos centraremos en la función ``do_clocktick()`` que es invocada cuando el opcode de es igual a ``HARD_INT``. Luego explicaremos cómo se realiza esta llamada a partir ``interrupt()``.

La función ``do_clocktick()`` a pesar de su nombre no se ejecuta en cada tick. Lo que hace este código es comprobar si se ha producido una alarma y en caso de que haya ocurrido tratarla y tambien se encarga de invocar a la función ``sched()``, que como antes comentabamos se encarga de reorganizar la cola de procesos a la vez que se reinicia la variable ``sched_ticks``.

La proxima función que comentaremos está en el fichero ``proc.c`` y se llama ``interrupt()``. Es invocada cuando el cuantum ha finalizado. En el caso de que ``k_reentrer `` sea distinto de 0 o switching se añade el proceso de usuario a una cola de retenidos. En el caso de que el proceso esté esperando por alguna interrupción se bloquea. Seguidamente se manda el mensaje ``HARD_INT`` citado anteriormente y por último se coloca ``rp`` en la cola de tareas.









