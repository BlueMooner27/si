# COMANDO IP UTILIZADOS PARA CONFIGURACION DE RED

## Problemas de los servicios de red en linux
En debian, si tenemos la interfaz gráfica instalada tenemos dos servicios de red:

  1. NetworkManager (de la interfaza gráfica)
  2. networking

Debemos desabilitar y parar uno de ellos en este caso desabilitaremos el de la interfaz gráfica (NetworkManager)

```bash
# Esto para el servicio
systemctl stop NetworkManager
```

```bash
# Esto arranca el servicio
systemctl start NetworkManager
```

```bash
# Esto deshabilita el servicio
systemctl disable NetworkManager
```

```bash
# Esto habilita el servicio
systemctl enable NetworkManager
```

```bash
# Esto muestra el estado del servicio/s
systemctl status NetworkManager
systemctl status networking
```

## Fichero de configuarción de red

El fichero para grabar la configuración de red permanentemente es /etc/network/interfaces



## Ver interfaces de red
- Sintaxis: ip addr show dev [interfaz]
```bash
$ ip addr show dev enp0s3
```