Rails Girls Barranquilla 2016

Taller gratuito para aprender a desarrollar aplicaciones web con Ruby on Rails.

Fecha del evento: 5 - 6 de Agosto 2016

## 1. Creando la aplicación

Primero, abrimos una terminal:

- Mac OS X: Abre Spotlight, digite Terminal y click en la applicación Terminal.
- Windows: Click en Inicio y busca Comando Prompt, luego click Comando Prompt con Ruby on Rails.
- Linux (Ubuntu): Buscar por Terminal en el dash y click Terminal.

Una vez en la terminal digitamos estos comandos:

Para crear una nueva carpeta:

```sh
mkdir proyectos
```

Ahora para ingresar al folder proyectos ejecuta el comando:

```sh
cd proyectos
```

Puedes validar que estás ahora en un directorio o folder vacío, ejecutando el comando de listado  `ls`.
Ahora crearemos una nueva aplicación rails:

```sh
rails new rgbaq2016 
``` 
 **rgbaq2016** es un nombre de ejemplo, puedes cambiarlo.

Esto creará una nueva aplicación en el folder rgbaq2016, así que nuevamente tenemos 
que cambiar el directorio para ingresar a nuestra app. Ejecuta:

```sh
cd rgbaq2016
```

Si ejecutas `ls` dentro del directorio, deberías ver algunas carpetas como _app_ y _config_ entre otros arhivos.
Puedes comenzar el servidor de rails, ejecutando:

```sh
rails server
```
Ingresa a [http://localhost:3000](http://localhost:3000) en tu navegador.  Debería verse una 
página “Welcome aboard”, lo que significa que la generación de tu nueva aplicación esta funcionando 
correctamente.


Presiona `Ctrl`+`C` en el terminal para salir del servidor.

