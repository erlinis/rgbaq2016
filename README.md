# Rails Girls Barranquilla 2016

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


## 2. Crear Posts

Vamos a usar el comando `scaffold` para generar un punto de partida que nos permita listar, agregar, eliminar, editar, y ver posts, para esto ejecuta el siguient comando en la terminal:

```sh
rails generate scaffold post caption:text picture:string
```
El comando `scaffold` crea nuevos archivos en el directorio del proyecto, los más importantes este momento, debido a que estaremos trabajando en ellos son:

| Archivo / Carpeta                   | Desripción                                                 |
|:----------------------------------: |:----------------------------------------------------------:|
| app/models/post.rb                  | Clase que representa el concepto _Post_                    |
| app/controller/posts_controller.rb  | Maneja todas las solicitudes de los _Posts_                |
| db/migrate/2016..._create_posts.rb  | Definición para crear la tabla _Posts_ en la base de datos |
| app/views/...                       | Contiene las interfaces para crear, listar, editar posts   |

Para crear la tabla en la base de datos, ejecutamos:  

```sh
rake db:migrate
```

 Iniciamos nuevamente el servidor con:
```sh
rails server
```
Prueba lo que se ha generado hasta el momento ingresando a [http://localhost:3000/posts](http://localhost:3000/posts) en tu navegador.
Crea,  edita y mira la información de los _posts_.


## 3. Guardando imagenes

Para subir imagenes a nuestro servidor necesitamos instalar una pieza de software, llamada `gema` en Rails.
Usando tu editor texto abre el archivo `Gemfile` que se encuentra en la raíz del directorio  y debajo de la linea

```
gem 'sqlite3'
```
Agrega

```
# Use Carrierwave to upload images on the server
gem 'carrierwave'
```
En la terminal ejecuta:

```sh
bundle
```

Ahora podemos generar el código para el manejar la subida de imagenes, para eso en la terminal ejecuta:

```sh
rails generate uploader Picture
```

Si el servidor está corriendo. Presione `Ctrl` + `C` para cerrarlo y luego ejecuta `rails server` para iniciarlo.
Es necesario reiniciar el proceso del servidor Rails para que la aplicación pueda cargar el código generado por la librería recién agregada.

Para usar el codigo generado previamente abrimos el archivo `app/models/post.rb` y debajo de la línea

```
class Post < ApplicationRecord
```
agregamos

```
  mount_uploader :picture, PictureUploader
```

Luego abre `app/views/posts/_form.html.erb` y cambia
```
<%= f.text_field :picture %>
```

por
```
<%= f.file_field :picture %>
````

Ingresa a [http://localhost:3000/posts](http://localhost:3000/posts) en tu navegador y agrega un nuevo _post_ con una imagen. Notaras cuando cargas una imagen que esta no se ve, esto es porque te muestra sólo la ruta del archivo, para arreglar eso abre el archivo `app/views/post/show.html.erb` y cambia

```
<%= @post.picture %>
```
por

```sh
<%= image_tag(@post.picture_url, :width => 400) if @post.picture.present? %>
````

Ahora refresca tu navegador para ver los cambios.


## 4. Añadiendo navegación

La aplicación no se ve muy bien aún. Usaremos el proyecto [Bootstrap](http://getbootstrap.com/getting-started/) para darle un mejor estilo.

Abre `app/views/layouts/application.html.erb` en tu editor de código y encima de la linea:

 ```sh
 <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
 ```

Incluye las siguientes lineas:

```sh
<link rel="stylesheet" href="http://railsgirls.com/assets/bootstrap.css">
<link rel="stylesheet" href="http://railsgirls.com/assets/bootstrap-theme.css">
```

y reemplaza esta linea
```
<%= yield %>
```
por

```sh
<div class="container">
  <%= yield %>
</div>
```

También agregaremos una barra de navegación y un pie de página, para esto pega el siguiente código debajo la etiqueta `<body>`

```
    <nav class="navbar navbar-default navbar-fixed-top">
      <div class="container">
        <div class="navbar-header">
          <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
            <span class="sr-only">Toggle navigation</span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
            <span class="icon-bar"></span>
          </button>
          <a class="navbar-brand" href="/"> Rails Girls BAQ</a>
        </div>
        <div id="navbar" class="navbar-collapse collapse">
          <ul class="nav navbar-nav">
            <li><%= link_to "Posts", posts_path %></li>
            <li><%= link_to "New Post", new_post_path %></li>
          </ul>
        </div>
      </div>
    </nav>
```

y este otro bloque encima de la etiquta `</body>`

```
  <footer>
    <div class="container">
      <p>Rails Girls Barranquilla 2016</p>
    </div>
  </footer>
  <script src="//railsgirls.com/assets/bootstrap.js"></script>
```

Mira la nueva barra de navegación en [http://localhost:3000/posts](http://localhost:3000/posts)


## 5. Agregando comentarios

Seamos más sociales permitiendo que se puedan añadir comentarios sobre nuestros _posts_, para esto presionamos `Ctrl` + `C` para detener el servidor y ejecutamos el siguiente comando en la terminal:

```sh
rails g model comments content:text post:references
```

Se generaran unos archivos, uno de los más importatntes es la migración, pues contiene la definición de la tabla donde se guardaran los comentarios.

Ejecutamos la migración para crear la tabla en la base de datos con el comando:  

```sh
rake db:migrate
```

 Iniciamos nuevamente el servidor con:
```sh
rails server
```

Creemos la relación entre _post_ y _comments_. Abrimos el archivo `app/models/post.rb` y debajo de la línea

```
  mount_uploader :picture, PictureUploader
```

agregamos

```
 has_many :comments, :dependent => :destroy
 accepts_nested_attributes_for :comments,  :reject_if => lambda { |a| a[:content].blank? }, :allow_destroy => true
```

Ahora necesitamos una formulario para escribir los comentarios, entonces abrimos `app/views/posts/show.html.erb` y encima de la linea

```
<%= link_to 'Edit', edit_post_path(@post) %> |
```

agrega

```
<%= form_for(@post) do |f| %>
  <%= f.fields_for :comments, @post.comments.build do |builder| %>
    <div class="field">
      <%= builder.label :content, 'Write a comment' %>
      <%= builder.text_area :content %>
    </div>
  <% end %>

  <div class="actions">
   <%= f.submit 'comment' %>
  </div>
<% end %>
```

Luego abre el archivo `app/controllers/posts_controller.rb` y dentro del método `post_params` cambia la linea
```
params.require(:post).permit(:caption, :picture)
```

por 

```
params.require(:post).permit(:caption, :picture, comments_attributes: [ :content,  :_destroy, :id ] )
```

Entra a [http://localhost:3000/posts](http://localhost:3000/posts) y abre un _post_ creado anteriorment y agrega un comentario.

Nótaras que no se pueden ver los comentario agregados, para poder visualizarlos entra al archivo `app/views/posts/show.html.erb` y encima de la linea

```
<%= form_for(@post) do |f| %>
```

agrega
```
<p>
  <strong>Comments</strong>
  <br>
  <% @post.comments.each do |comment| %>
    <span class="text-muted"><%= comment.created_at.to_formatted_s(:short)  %></span> | <%= comment.content %>
    <br>
  <% end %>
</p>
```

Ahora refresca tu navegador para ver los comentarios.


## 6. Mejorando el diseño

Demosle un poco de estilo a la aplicación usando algunos de los componentes de bootstrap.

Abre el archivo `app/views/posts/index.html.erb` y cambia su contenido por

```
<p id="notice"><%= notice %></p>
<% @posts.reverse.each do |post| %>
  <div class="row">
    <div class="col-md-8 col-sm-4">
      <div class="thumbnail">
        <%= image_tag(post.picture_url, :width => 200) if post.picture.present? %>
        <div class="caption">
          <h4><%= link_to post.caption, post %></h4>
          <p>
            <%= post.created_at.to_formatted_s(:short) %> |
            <%= link_to "Comentarios #{post.comments.count}", post %> |
            <%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' } %> 
          </p>
        </div>
      </div>
    </div>
  </div>
<% end %>

```

Mira el nueva aspecto de la aplicación en [http://localhost:3000/posts](http://localhost:3000/posts)
