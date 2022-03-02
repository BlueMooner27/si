# ADMINISTRACIÓN DE USUARIOS

## USUARIOS COMUNUES Y SUPERUSUARIO

***

### Usuarios comunes

Los usuarios comunes:

- Administran sus propios datos en **/home**
- No tiene privilegios para hacer configuraciones globales en en sistema
- No puede gestionar dispositivos
- No puede instalar ni actualizar configuraciones
- No puede manipular archivos de otros usuarios

### Superusuario/root

El superusuario o usuario root

- Es el que puede hacer todo (o casi) dentro del sistema
- Puede gestionar aplicaciones y actualizaciones del sistema
- Puede gestionar dispositivos
- Puede gestionar los privilegios de otros usuarios

## Autentificación

- Cada usuario se autentifica en el sistema, mediante:
  - **Username** (nombre de usuario)
  - **Password** 
- Un usuario común puede "elevar privilegios" a **usuario root** para realizar ciertas tareas (naturalmente esto es así si el usuario ha dado permiso al usuario común para hacer esto)

***

## Su y sudo

### Ejecutando comandos como root

- Muchos comandos en linux requieren privilegios de root
- Instalación de aplicaciones y actualización del sistema
- Hay dos formas de elevar privilegios del terminal
  - **su**: permite cambiarse a la cuenta de root
  - **sudo**: permite ejecutar comandos como root pero sin necesidad de pasarnos a la cuenta de root

### Comando **su**

- Al ejecutar el comando **su** el sistema nos pedirá la password del usuario root
- Obtendremos un prompt del root (#)
- Para volver a nuestro prompt de usuario ejecutamos `exit`

### Comando **sudo**

- Con **sudo** podemos ejecutar comandos con privilegios de **root** pero sin serlo
- Luego de la ejecución del comando volvemos a nuestro prompt de usuario común
- Para usar sudo es requisito haber sido previamente habilitado por el root para poder ejecutarlo
- **sudo** es más flexible y facilita la administración de linux
- Muchas distribuciones desactivan al usuario root de modo que la única forma de ejecutar comandos, es con **sudo**

### Comando **id**
- El comando **id** nos permite analizar este tipo de privilegios
- Sintaxis: id [usuario]

```
# Para el actual usuario
id
# Salida: uid=0(root) gid=0(root) grupos=0(root)

# Para otro usuario
id german

```

- Podemos ejecutar `sudo id` como usario común y vemos que el comando `id` se ejecuta como root

- Si nuestro usario no esta dentro del fichero **/etc/sudoers** la manera más sencilla de incluirlo es añadirlo al grupo **sudo** que si lo está

```
# Añade el usuario germán al grupo sudo
usermod -aG sudo german
```

***

## Usuarios y grupos

### Usuarios
Cuando definimos un usuario hemos de definir:

- Nombre de usuario
- Contraseña
- Id de usuario
  - El usuario root siempre tiene id = 0
  - En debian y sus derivados el resto de usuarios el id de usuario > 1000
  - Los usuarios que representan servicios del sistema id de usuario < 1000
- Grupo principal
- Información adicional (GECOS)
- Directorio de trabajo del usuario
- Interprete de comandos (shell)
  - Para usuarios comunes (/bin/bash)
  - Para usuarios que representan servicios (/bin/false)

### Grupos

Para los grupos hemos de definir: 

- Nombre de grupo
  - En debian y sus derivados cuando creamos un usuario también se crea un grupo con el mismo nombre
- Contraseña
- Id de grupo
  - 0 para root
- Lista de usuarios
  - Un usuario tiene un único grupo principal y 0, 1 o varios grupos secundarios, en esta lista aparecerá los usuarios que pertencen a ese grupo como grupo secundario
  
### Comando groups

- El comando `groups` permite ver información de los grupos de un usuario
- Este comando no requiere privilegios de administrador
- Sintaxis: groups [usuario]
- Ejemplo

```bash
### Si lo ejecuto como root
-> groups # Muestra el grupo root
-> groups german # Muestra german : german cdrom floppy sudo audio dip video plugdev netdev bluetooth
```

***

## Creación de usuarios

### Comando useradd

- Añade un nuevo usuario
- Este comando requiere privilegios de administrador
- Sintaxis: useradd [opciones] [nombre_usuario]
- Opciones:
  - -c comentario => Comentarios (GECOS)
  - -d directorio => Directorio
  - -D => Muestra valores por defecto
  - -e yyyy-mm-dd => Indicamos la fecha de expiración de un usuario
  - - g grupo_primario => Indicamos el grupo primario al que queremos que pertenezca el usuario
  - - G grupos_secudarios => Indicamos los grupos secundarios a los que queremos que pretenezca el usuario, separados por comas
  - - m => Indica que cree el directorio personal del usuario ya que por defecto este comando no lo crea
  - - s interprete => Indicamos la shell del usuario
  - - u uid => Indicamos el uid del usuario 

- Ejemplo:

```bash
# Muestra valores por defecto para la creación de usuarios
-> useradd -D

# Crear un usuario con comentarios, creando su directorio personal y un uid específico
-> useradd -c "Usuario de prueba" -m -u 1200 prueba
# Mostramos el resultado con id
-> id prueba # gid=1200(prueba) grupos=1200(prueba)
# Comprobamos que se ha creado su directorio
-> ls /home

# Crear un usuario con 
-> useradd -c "Usuario de prueba2" -g german -G operator,backup prueba2
# Mostramos el resultado con id
...
```

***

### Comando adduser

- Crea un usuario de forma interactiva
- Este comando requiere privilegios de administrador
- Sintaxis: adduser [opciones] \<nombre_usuario\>
- Opciones: 
  - --uid uid => Indicamos el uid del usuario
  - --shell interprete => Indicamos el interprete de comandos del usuario
  - --home directorio => Indicamos el directorio personal del usuario
  - --gid id => Indicamos el identificador del usuario
  - --gecos comentarios => Indicamos la descripción del usuario
- Ejemplos:

```bash
# Creación del usuario con un home determinado
-> adduser --home /users/prueba3 prueba3

# Creación de un usuario con comentarios
-> adduser --gecos "Nuevo usuario" prueba4
```

***

## Modificación de usuarios

### Comando usermod

- Modifica la cuenta de un usuario
- Este comando requiere privilegios de administrador
- Sintaxis: usermod [opciones] \<usuario\>
- Opciones
  - -c comentario => Establece un comentario
  - -d directorio => Establece el directorio personal (esta opción suele ejecutarse con -m)
  - -m => Mueve lo que estaba en el directorio antiguo al nuevo
  - -e yyyy-mm-dd => Establece fecha de espiración
  - -g grupo => Establece el grupo principal
  - -G grupos => Establece grupos secundarios (esta opción suele ejecutarse con -a)
  - -a => Añade grupos secundarios a los que ya tiene
  - -s interprete => Establece interprete de comandos
  - -u uid => Establece el identificador del usuario. Si cambiamos el uid de un usuario debemos cambiar el propietario del directorio/fichero del usuario
  - -U => Desbloquea el usario
  - -L => Bloquea un usuario
- Ejemplos

```bash
# Si se ejecuta sin parámetros muestra una ayuda resumida
-> usermod
# Establece los grupos secundarios
-> usermod -G users prueba2
# Añade grupos secundarios
-> usermod -aG games prueba2
```

***

## Eliminación de usuarios

### Comando userdel

- Elimna un usuario del sistema
- Este comando requiere privilegios de administrador
- Sintaxis: userdel [opciones] \<usuario\>
- Opciones:
  - -f => Fuerza la eliminación del usuario incluso si el usuario esta en sesión
  - -r => Elimina también su directorio personal
- Ejemplos:

```bash
# Elimina el usuario y su directorio personal
-> userdel -r 
```

***

## Ficheros /etc/passwd y /etc/shadow

- Toda la infromación de los usuarios en linux se aloja en ficheros

### Fichero /etc/passwd

- Contiene información del usuario
- Campos
  - Nombre del usuario
  - Contraseña
    - Si contiene una x indica que la contraseña se encuetra en el fichero /etc/shadow
    - Si conteene una ! indica que el usuario está bloqueado
  - id del usuario
  - id del grupo principal
  - Descripcion (GECOS)
  - Directorio del usuario
  - Interprete de comandos
- Ejemplo

```bash
$ cat /etc/passwd | egrep "prueba2"
# Muestra: prueba2:x:1201:1000:Usuario de prueba2:/home/prueba2:/bin/sh
```

### Fichero /etc/shadow

- Contiene información de las contraseñas
- Campos:
  - Nombre de usuario
  - Contraseña cifrada
  - Fecha del último cambio de contraseña. Se indica a través de días a partir de 1/1/1970
  - Días mínimos de validad de clave. Indica que si yo cambio la contraseña hoy voy a tener que esperar los días indicados para volver a cambiar la contraseña, si se indica 0 puedo volver a cambiar la contraseña tantas veces como quiera
  - Días máximos de validez de clave. Indica cuantos días tiene validez la clave que yo he cambiado, pasados esos días tendré que cambiar la contraseña para poder seguir utilizando la cuenta
  - Periodo de aviso de clave. Indica los días de antelación con los que el sistema me avisará antes de que caduque la contraseña
  - Periodo de inactividad de clave. Indica el número de días después de la expiración de la contraseña para que nuestra contraseña se bloquee
  - Fecha de espiración de la cuenta
  - Campo reservado para posibles nuevos campos


- Ejemplos:

```bash
$ cat /etc/shadow | egrep "prueba2"
# Muestra: prueba2:!:19041:0:99999:7::
$ cat /etc/shadow | egrep "german"
# Muestra: german:$y$j9T$dXaZvsIavSPL3sttE52aM1$E/Y8uCj16o0Ogc6.uR/mxpYnBTwGauymA.8fidJR9g8:18900:0:99999:7:::

# Modificamos la fecha de expiración
-> usermod -e 2022-02-28 prueba2
# Mostramos el cambio
$ cat /etc/shadow | egrep "prueba2"
```

### Comando chage

- Cambia la fecha de expiración de un usuario o su contraseña
- Este comando requiere privilegios de administrador
- Sintaxis: chage [opciones] [usuario]
- Opciones:
  - -l => Lista información
  - -d fecha => Establece último cambio de clave
  - -E fecha => Establece fecha de expiración de la cuenta
  - -I días => Establece días antes de inactividad
  - -m días => Establece mínimo de días de validez de la clave
  - -M días => Establece máximo de días de validez de la clave
  - -w días => Establece aviso antes de cadudidad
- Ejemplos:

```bash
-> chage -l prueba2
-> chage -m 1 prueba2
-> chage -M 30 prueba2
-> chage -l prueba2
```

### Comando getent

- Consulta una base de datos (usarios, contraseñas, grupos)
- Este comando requiere privilegios de administrador si se consulta información de contraseñas
- Podría conectarse a otras fuentes de usuarios, una base de datos mysql, un directorio ldap, etc.
- Sintaxis: getent base_de_datos [usuario]

- Ejemplos

```bash
$ getent passwd 
$ getent passwd german
$ getent shadow # Solo administrador
$ getent shadow german # Solo administrador
$ getent group
$ getent gshadow
```

### Comando passwd

- Para cambiar la contraseña y parámetros relacionados con la cuenta
- Este comando requiere privilegios de administrador para establecer cambios en otras cuentas que no sean las del propio usuario
- Sintaxis: passwd [opciones] \<usuario\>
- Opciones: 
  - -l => Bloquea la cuenta
  - -u => Desbloquea la cuenta
  - -e fecha => Establece expiración de la contraseña
  - -n días => Establece el mínimo de días de validez de la contraseña
  - -x días => Establece el máximo de días de validez de la contraseña
  - -w días => Establece el número de días del preaviso para cambiar la contraseña
  - -i días => Establece el número de días después del cual la cuenta quedará inhabilitada
  - -S => Muesta información
- Ejemplos:

```sh
passwd -S
passwd -l prueba
passwd -u prueba
```

***

## Ficheros /etc/group y /etc/gshadow

### Fichero /etc/group

- Contiene información sobre los grupos
- Campos
  - Nombre del grupo
  - Contraseña (Normalmente "*")
  - Id del grupo
  - Lista de usuarios
- Ejemplos:

```sh
$ getent group users
# Muestra: users:x:100:prueba2
```

### Fichero /etc/gshadow

- Contiene información sobre contraseña y administradores
- Campos
  - Nombre del grupo
  - Contraseña cifrada, utilizada para usuarios que pueden acceder al grupo temporalmente para realizar algunas tareas
  - Administradores
    - Pueden cambiar la contraseña
    - Pueden elimnar o añadir a miembros
  - Lista de usuarios
- Ejemplos:

```sh
-> getent gshadow users
# Muestra: users:*::prueba2
```

***

## Comandos para grupos

### Comando groupadd 

- Crea un grupo
- Sintaxis: groupadd [opciones] \<nombre_grupo\>

- Como administradores podemos cambiar la contraseña del un grupo mediante el comando `gpasswd`


