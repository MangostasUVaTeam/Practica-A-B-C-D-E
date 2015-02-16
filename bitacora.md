---
title: Bitácora Práctica - ESO
author: garciparedes 
---

# Bitácora Prácticas A+B+C+D+E: 
## Sergio García Prado
## Adrián Calvo Rojo

### 9 de Febrero de 2015

Hemos empezado a hacer la primera práctica, que consiste en instalar Minix en VirtualBox. Nos hemos descargado los archivos y estamos configurando la maquina virtual. Hemos añadido un nuevo controlador de disquete y también hemos cambiado el orden de arranque para que este lo haga primero. También hemos desactivado el arranque por CD. 
    
Procedemos a arrancar la maquina... Pide un login. Nos sentimos confusos. Hemos descubierto que lo que había que hacer es acceder como "root". Hemos descubierto que tecleando 'alt + Fi' se cambia entre los distintos terminales.
 
#### Tutorial Sobre Vim:
    - `cursores`	: para moverse
    - `escape`	    : para usar comandos
    - `i`  		    : Insertar en la linea.
    - `u`  		    : Añadir en la linea.
    - `x` 		    : Sirve para borrar.
    - `:w` 		    : sirve para guardar.
    - `:q`		    : Sirve para salir.
    - `a` 		    : Escribir despues del cursor.
    - `nyy`        : Copia n lineas desde el cursor
    - `p`           : pegar
    - `%`           : cambia el cursor entre la pareja del corchete.
    - `ndd`         : Borra n lineas desde el cursor
    - `u`           : Deshacer
    - `!`           : Forzar comando
		
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
        for (int i = 0 ; i < numH; i++){
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


### X de Febrero de 2015

#### Programación

