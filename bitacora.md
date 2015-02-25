
# Bitácora Prácticas A+B+C+D+E: 
## Sergio García Prado
## Adrián Calvo Rojo

## Numero grupo 46

## **Práctica A**
### 9 de Febrero de 2015

Hemos empezado a hacer la primera práctica, que consiste en instalar Minix en VirtualBox. Nos hemos descargado los archivos y estamos configurando la maquina virtual. Hemos añadido un nuevo controlador de disquete y también hemos cambiado el orden de arranque para que este lo haga primero. También hemos desactivado el arranque por CD. 
    
Procedemos a arrancar la maquina... Pide un login. Nos sentimos confusos. Hemos descubierto que lo que había que hacer es acceder como "root". Hemos descubierto que tecleando 'alt + Fi' se cambia entre los distintos terminales.
 
#### Tutorial Sobre Vim:
    - 'cursores'	: para moverse
    - 'escape'	    : para usar comandos
    - 'i'  		    : Insertar en la linea.
    - 'u'  		    : Añadir en la linea.
    - 'x' 		    : Sirve para borrar.
    - ':w' 		    : sirve para guardar.
    - ':q'		    : Sirve para salir.
    - 'a' 		    : Escribir despues del cursor.
    - 'nyy'         : Copia n lineas desde el cursor
    - 'p'           : pegar
    - '%'           : cambia el cursor entre la pareja del corchete.
    - 'ndd'         : Borra n lineas desde el cursor
    - 'u'           : Deshacer
    - '!'           : Forzar comando
    - 'set nu'      : Muestra el número de líneas
    - '/[texto]'    : Busca [texto] dentro del fichero
    - ':100'        : Te lleva hasta la linea 100
		
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
Vamos a proceder a realizar la copia de Minix, para ello tenemos que modificar el identificador interno (UUID) del nuevo fichero vdi. Los nuevos ficheros se llamarán 'kernel-2.0.0 copia.img' y 'minix2.0.0-copia.vdi' por lo que para cambiar el identificador utilizamos el siguiente comando:
    'VBoxManage internalcommands sethduuid minix2.0.0-copia.vdi'
Una vez hecho esto procedemos a crear la segunda máquina virtual en VirtualBox. 

#### Cambiando el shell:
Tras comprobar que esta funciona correctamente, nos disponemos a cambiar el shell por defecto por el 'ash'. Para ello tenemos que modificar el fichero '/etc/passwd'. Para hacer esto tan solo hay que modificar la linea donde se indica que se está usando 'sh' por 'ash'. Gracias al nuevo shell, ya funciona correctamente la función autocompletar al pulsar el tabulador.

#### Creando un nuevo usuario:
Lo primero que haremos será crear el directorio para este nuevo usuario. Lo hemos creado en '/home/usuario'. Lo siguiente que hemos hecho ha sido entrar en el fichero 'passwd' y hemos creado el usuario. Le hemos asignado el nombre **usuario** , le hemos asignado el Id de usuario 9 y el de grupo 4. También le hemos asignado su directorio y por último el shell 'ash'. Por último le hemos asignado una contraseña con el comando 'passwd'. La contraseña que se le ha asignado es **pass**.

Tras Comprobar que todo funciona adecuadamente accediendo al usuario y comprobando que su directorio home está en el lugar adecuado con el comando 'pwd' reiniciamos la máquina y seguidamente la hemos apagado como indica el guión de la práctica.

#### Compilación del núcleo
Lo primero que hemos hecho ha sido modificar el mensaje de bienvenida de la máquina modificando la función 'tty_task()' del fichero '/usr/src/kernel/tty.c' en la que hemos añadido nuestros nombres. Ahora compilaremos el nucleo.

Para verificar que la compilación del nucleo se haya realizado correctamente vemos el contenido del fichero '/usr/stc/tools/revision' que en este caso es **11**. Después de la compilación este debería incrementarse. Procedemos a aplicar el comando 'make hdboot' para compilar. Tras unos problemas menores debido a que al modificar el mensaje de bienvenida borramos por error una de las comillas, que ya han sido solucionados, se ha compilado correctamente creandose una copia en '/minix' y incrementandose a 12 el contenido del fichero 'revision'.

Tras reiniciar la máquina vimos que no había cambiado nada. Esto es debido a que seguiamos ejecutando la imagen que esta en el disquete por lo que procedemos a apagarla y cambiar el orden de arranque en la configuración de VirtualBox. Ahora si, la versión que corre la máquina es la compilada por nosotros y se muestra el mensaje que habíamos modificado.


### 19 de Febrero de 2015

#### Programación
Hemos creado la carpeta '/root/PracticaA' que usaremos para trabajar en la parte de programación de esta práctica. Para hacer el programa, hemos creado el fichero 'creaHilos.c'. Tras utilizar un código muy similar al que nos mostró el profesor en clase vemos que funciona correctamente pero nos salen algunos "warnings" que vamos a intentar solventar. Las causas de esto era que simplemente faltaban 'includes' y que en la función hijo no pusimos 'void' como parametro en la función, es decir, no pusimos 'void hijo(void);' Tras tener dificultades debido a que en la función hijo pusimos un 'printf' no veiamos la linea donde 'fork()' devolvia -1, así que estuvimos un buen rato buscando el motivo hasta que se nos ocurrio probar quitando el mensaje, por si había aparecido algo entre medias, y efectivamente, no inidica que el numero máximo de hilos creado es **27**.

## **Práctica B**
Durante estos días hemos pensado que lo mejor sería leer por separado el todo el código al que se hace referencia en el guión de la práctica, y en el caso de que nos surgiesen dudas resolverlas juntos. Esto es debido a que creemos que para poder entenderlo necesitamos mucha concentración, y esto se hace mejor en solitario. Cuando hayamos comprendido bien como funciona nos pondremos a realizar en común la parte practica de este apartado.

### 22 de Febrero de 2015

Las variables en mayúscula se refieren a constantes alfanuméricas.

Siguiendo el guión vemos que la única lo único que hace el fichero '_fork.c' es llamar a la función '_syscall()' con los parámetros apropiados, el primero indica qué tipo de operación se va a realizar, en este caso (MM) que se refiere a *Administración de Memoria*. el segundo sirve para identificar la operación, en este caso (FORK) y el último es la direción a una variable de tipo (message) declarada en el fichero '_fork.c'. En la función '_syscall()' se llama a la función '_sendrec' a la cual se le envia como parámetros 'who' (MM) y 'message' despues de haber definido su 'm_type' como la constante (FORK) en este caso. El resto de esta función sirve para tratar el resultado devuelto por 'sendrec'.

Tras revisar la función '_taskcall' vemos que realiza la misma función pero con un tratamiento de excepciones diferente (devuelve los errores negativamente y no utiliza 'errno'), alegando que es mejor para las operaciones de tipo MM (*Administración de Memoria*) y FS (*Sistema de Ficheros*).

Por tanto pasamos a analizar la función '_sendrec'. Lo primero que nos llama la atención de este fichero (i386/rts/_sendrec.s)es que es dependiente de cada arquitectura. Esto es devido a que está escrito en ensamblador. Lo que hace la función __senrec es mover los parámetros que habiamos enviado a esta función de memoria a registros. Tras esto es cuando se produce la interrupción software, es decir 'int SYSVEC' El argumento que se le pasa es '33', que es el valor de la variable alfanumérica SYSVEC. Es en este punto a partir del cual se entra en *Modo Privilegiado*.

Ahora como indica el guión de la práctica nos dirigimos a 'src/kernel/protect.c' que contiene el código para la inicialización del modo protegido. Una de las cosas que llaman la atención de este fichero es que tiene etiquetas condicionales. Estas son etiquetas para el compilador y se utilizan para que dependiendo de distintos parámetros como por ejemplo la arquitectura el compilador decida si saltarse o no ciertos trozos de código.

Nos dirigimos a la función 'prot_init()' y empezamos a leer su código. Lo primero que hace es configurar las tablas para el modo protegido. Ahora vemos 'SYS386_VECTOR' que está declarado en 'const.h'. Esta vector está definido como 33.

Ahora seguiremos la pista de la función 's_call()' como indica el guión de la práctica.

### 25 de Febrero de 2015

Antes de seguir la pista a la función 's_call()', hemos decidido mirar mejor el vector  'SYS386_VECTOR'. Dentro del archivo 'const.h', en la línea 43 esta dicho vector con el número **33** que es el mismo que tenia 'SYSVEC'. Ahora volvemos a la función 's_call()' escrita en ensamblador. Esta función aparentemente crea una pila con los argumentos para despues, con ellos, llamar a otra funcion: 'sys_call()'.

La función 'sys_call()', dentro del fichero '/usr/src/kernel/proc.c' recibe 3 argumentos: el primero puede ser 1 = **SEND**, 2 = **RECEIVE**, 3 = **BOTH** en función de como se quiera usar; el segundo argumento es la fuente a la que se quiere enviar o desde la que se va a recibir el mensaje; el tercero es un puntero al mensaje creado previamente en 'fork()'. En función de los argumentos que haya recibido envía y/o recibe el mensaje con las llamadas a las funciones mini_send() y mini_rec().

Hemos mirado 'proc.h' y creemos que la variable 'proc' es la que indica qué marcos de memoria forman parte del proceso...

Acabamos de darnos cuenta de cómo funciona. Las funciones anteriores ('mini_send()' y 'mini_rec()') laman a una funcion, 'CopyMess()' que a su vez llama a otra, 'cp_mess'. Esta se encarga de convertir una dirección virtual en una direción física y de colocar en la tabla de vectores (TVI) la función que se va a ejecutar que en este caso es 'fork'. Como incluye el fichero '/usr/include/minix/com.h', que posee todas las funciones como constantes, pone en la tabla la funcion 'fork'.

'usr/src/kernel/mpx386s.s' es el fichero encargado de guardar la mayoría del estado de la máquina cuando hay una interrupción y una ve tratada recuperar ese estado.

'table.c' se encarga de redireccionar, en funcion de la tabla, al metodo correspondiente, en este caso a  'sys_task'  que esta en 'system.c' y esta invoca a 'do_fork' que es el metodo llamado al inicio. Por lo tanto, modificando este metodo podemos poner el mensaje.