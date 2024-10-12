---
title: instalacion de termux y faq
categories: [termux]
comments: true
---


## Que es termux
Termux es un emulador de terminal para android, el principal uso que le doy es editar y ejecutar codigo, es útil para editar codigo "on-the-fly" de repositorios tanto en git como en syncthing.

![screenshot](/assets/img/posts/screenshot-termux.png){: width="250"}

### Como instalar termux
Está disponible en [Fdroid](https://f-droid.org/es/packages/com.termux/) en formato .apk o en [Github](https://github.com/termux/termux-app#github) (recomendado)

Una vez instalado, Lo primero que hago es seleccionar un grupo de mirrors para mantener los repositorios
```bash
termux-change-repo
```
Esto invoca una interfaz gráfica que permite seleccionar un mirror, seleccionemos **mirror groups** y luego **Mirrors in South America**.

![cambiar mirror-group](/assets/img/posts/scr-termux-change-repo.jpg)

```bash
pkg update
```
Ahora otorgamos acceso al almacenamiento del dispositivo para lectura/escritura de archivos.
```bash
termux-setup-storage
```
Esto nos va a redirigir a la pantalla de configuracion del teléfono, activamos el slider para conceder acceso a todos los archivos:
![termux-setup-storage](/assets/img/posts/termux-grant-access.jpg   )

Ahora deberíamos tener acceso a todos los archivos del dispositivo.

Listo! ya tenemos termux listo para editar y ejecutar código

---
Algunas preguntas que me surgieron al utilizar termux son:

### Que es un emulador de terminal?
Una [terminal](https://es.wikipedia.org/wiki/Terminal_(inform%C3%A1tica)) solía ser (es, supongo) un dispositivo físico (un monitor y un teclado) el cual se usaba para comunicarse directamente con una computadora, ver su información en lineas de texto y darle instrucciones mediante una interfaz de comandos.

![terminal en fallout 4](/assets/img/posts/terminal.webp) ~*Una terminal en fallout 4*

Ahora las terminales son software, conocidas como emuladores de terminal. Imitan la misma funcionalidad pero sin necesitar hardware especializado.
Termux emula una terminal en Android aprovechando el kernel de linux y completandola con un entorno que contiene el software básico y los comandos que vienen con todas las distribuciones modernas de linux. 

### Termux es linux?
>The environment setup in Termux is similar to that of a modern Linux distribution. However, running on Android implies several important differences.

Ver [Differences from Linux](https://wiki.termux.com/wiki/Differences_from_Linux) de la wiki de termux.

Las limitaciones de Android hacen que termux no sea compatible con software nativo de linux, las diferencias mas importantes son que termux no usa el sistema de archivos de linux, y ademas usa Bionic libc: la libreria standard de C escrita por Google para Android. Lo que rompe la compatibilidad con el software escrito para linux. 

Mas contenido en la [guía principal]({% post_url 2024-09-28-termuxmenu %})