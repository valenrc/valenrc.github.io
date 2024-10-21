---
title: como editar codigo en android - termux
categories: [termux]
comments: true
---

*Esta entrada pertenece a la [guia de termux]({% post_url 2024-09-28-termuxmenu %})*

Veamos que opciones tenemos para editar código en termux:

El editor que viene por defecto en Termux y en la mayoría de distribuciones de linux es ```nano```, el cual es bastante sencillo de usar, por ejemplo para abrir un archivo cualquiera o crearlo en el directorio actual si no existe:
```bash
nano nombre_archivo
```
Una vez dentro, se puede editar normalmente. Para guardar los cambios ``` Ctrl + o + Enter ``` y para salir ```Ctrl + x ```

Este editor es el mas ligero que se puede conseguir y ademas tiene las funciones básicas de un editor de texto, como syntax highlighting, copiar/pegar texto, reemplazar, configurar macros y mas funciones que se pueden consultar con ``` Ctrl + g ```

![screenshot del editor nano](/assets/img/posts/nano.png)
*screenshot de la [página de gnu-nano](https://www.nano-editor.org/)*

Ahora, si queremos algo mas parecido a un IDE con autocompletado, arbol de archivos, etc, etc...Tenemos la opcion de ``` nvim ```

```bash
pkg update && pkg install neovim
```
Con ```:``` podemos ingresar comandos y hay autocompletado con Tab , por ejemplo:

- ```:Tutor``` tutorial interactivo


personalizando nvim:

- ```:colorcheme <tema>``` cambia el tema (syntax highlighting y color de fondo)
- ```:syntax on``` habilita el syntax highlighting (habilitado por defecto)
- ```:syntax off```

### Instalando NvChad:
Prerequisitos:

Git:

```bash
pkg install git
```

NerdFont:
Instalemos la fuente FiraCode ([mas fuentes acá](https://www.nerdfonts.com/font-downloads))
``` bash
curl https://github.com/ryanoasis/nerd-fonts/releases/download/v3.2.1/FiraCode.zip
```
