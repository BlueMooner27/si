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