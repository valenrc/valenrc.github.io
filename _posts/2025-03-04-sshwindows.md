---
title: conexión SSH desde Windows a Termux
categories: [termux]
comments: false
---

## Configuración de un servidor SSH en Termux y conección desde Windows

En este post voy a configurar un servidor SSH en Termux y conectarnos a él desde Windows usando Powershell.
Esto es útil para acceder a Termux desde Windows sin tener que usar la terminal de Termux desde el telefono.
![screenshot](/assets/img/posts/powershell-termux.jpg)

### SSH server en Termux

Instalar el paquete ```openssh``` en Termux, esto instala y configura el servidor SSH automaticamente.
La configuración y las claves se encuentran en ```/data/data/com.termux/files/usr/etc/ssh/```.

```bash
pkg upgrade && pkg install openssh
```

Nos conectamos al servidor con ```USUARIO@IP``` en el puerto por defecto ```8022```.

Para obtener el nombre de usuario:

```bash
whoami
```

Para obtener la dirección IP del host:
(Buscar la etiqueta wlan0)

```bash
pkg install iproute2 && ip addr
```

iniciar el servidor SSH. Opcionalmente con el flag ```-d``` para ver mensajes de debug.

```bash
sshd -d
```

Mas detalles en [Termux Wiki - Remote Access](https://wiki.termux.com/wiki/Remote_Access)

### Conectarse al servidor desde Powershell

Usamos el cliente SSH de Windows que viene instalado por defecto en *optional features*
Nos va a pedir una *passphrase* si es que la configuramos.

```bash
ssh <USUARIO>@IP -p 8022
```

Mas información en [SSH in Windows Terminal](https://learn.microsoft.com/es-mx/windows/terminal/tutorials/ssh#access-windows-ssh-client-and-ssh-server)

---

## Autentificación
La autentificación por defecto es mediante la contraseña del usuario en Termux, pero se puede configurar para que sea mediante una clave pública (mucho mas seguro).

### Contraseña
Para cambiar la contraseña de Termux:
```bash
passwd
```
Cada vez que nos conectemos al servidor SSH nos va a pedir la contraseña del usuario.

### Clave pública
Tenemos que generar claves en el cliente (Windows) y copiar la clave pública en el host (Termux)

Generar el par de claves en la dirección ```C:\Users\<USUARIO>\.ssh\```
(En powershell)
```bash
ssh-keygen -t rsa -b 2048 -f $env:USERPROFILE\.ssh\id_rsa
cat $env:USERPROFILE\.ssh\id_rsa.pub
```

el comando ```cat``` muestra el contenido del archivo, lo copiamos. Ahora hay que pegarlo en el archivo ```~/.ssh/authorized_keys``` en Termux.

Para eso nos conectamos por ssh (ver [Conectarse al servidor desde Powershell](#conectarse-al-servidor-desde-powershell)) y dentro del host (terminal de termux) copiamos el contenido del portapapeles dentro de ```~/.ssh/authorized_keys```.

![](/assets/img/posts/rsa_public.jpg)

Ahora queda deshabilitar la conexión por contraseña en el host:
Dentro de termux, ir a ```/data/data/com.termux/files/usr/etc/ssh/sshd_config```, descomentar la linea ```PasswordAuthentication yes``` y cambiarla por ```PasswordAuthentication no```. 

---

Listo, ya podemos empezar a usar Termux desde Windows.