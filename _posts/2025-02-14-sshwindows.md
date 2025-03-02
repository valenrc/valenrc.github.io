---
title: Conexion SSH desde Windows a Termux
categories: [termux]
comments: true
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

Listo, ya podemos empezar a usar Termux desde Windows.
