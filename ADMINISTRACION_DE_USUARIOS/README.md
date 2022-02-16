# ADMINISTRACIÓN DE USUARIOS

## USUARIOS COMUNUES Y SUPERUSUARIO

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
- Sintaxis: id \[usuario\]

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

