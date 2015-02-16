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
    - `a` 		    : Escribir al inicio de la linea.
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

Vamos a proceder a realizar la copia de Minix, para ello tenemos que modificar el identificador interno (UUID) del nuevo fichero vdi

### X de Febrero de 2015
