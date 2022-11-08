# Active Storage

Active Storage es una herramienta de Rails que facilita el adjuntar archivos y almacenarlos ya sea de forma local (intranet) en tu ordenador o a plataformas en la nube como AWS, Google Cloud Storage, Microsoft Azure Storage, Digital Ocean, entre otros.
 
En este artículo se va a realizar un proyecto desde cero utilizando Scaffold que permite crear una estructura básica de Rails, teniendo como uno de sus atributos el adjuntar una imágen.
 
## Creando proyecto de libros
 
Vamos a crear el proyecto utilizando PostgreSql como base de datos, para lo cual ejecutamos desde un terminal el siguiente comando:
~~~ 
rails new books_storage - -database postgresql
~~~
## Utilizando Scaffold
 
Scaffold crea una estructura básica que permite administrar (mostrar, crear, eliminar y actualizar ) un libro, el mismo que tendrá los siguientes atributos: title, author, description, image, ejecutamos el siguiente comando:
~~~
rails new generate scaffold book title:string author:string description:string image:string
~~~

## Instalando Active Storage
Se lo puede instalar ejecutando el siguiente comando:
~~~
rails active_storage:install
~~~
Este comando va a generar una migración y creará una tabla donde se van a guardar las rutas de todos los archivos que se suban, no se guarda el archivo en si en la base de datos, este se va a almacenar en el disco duro si se trabaja de forma local, o en un almacenamiento en la nube si se lo lleva a producción.
 
Creamos la base de datos con:
~~~
rails db:create
~~~
y generamos las migraciones:
~~~
rails db:migrate
~~~
## Editando la clase Book
Se debe declarar los archivos que van a ir adjuntos al modelo  de book, para ello se agrega:
~~~ 
class Book < ApplicationRecord
	has_one_attached :image
end
~~~

_has_one_attached_ indica que el campo `image` del formulario tiene un archivo adjunto.

## Agregando el campo image en la vista
Abrimos el archivo `_form.html.erb` que se encuentra en el path: `app/views/books/_form.html.erb` se edita el campo correspondiente a la imagen cambiando del tipo `text_field` a `file_field` quedando:
~~~
<div>
	<%= form.label :image, style: “display:block” %>
	<%= form.file_field :image %>
</div>
~~~
## Cambiando la ruta raíz:
Se debe cambiar la ruta `root` con el fin de que se ejecute el la interacción de la clase libros que se acaba de crear para ellos se agrega en `config/route.rb`

~~~ 
root “books#index”
~~~
 
Donde books hace referencia al controlador e index es la acción a ejecutar. 
 
## Editando _book.html.erb
Para mostrar la imagen adjunta se puede hacer uso del helper `image_tag`, editando el campo correspondiente a la imagen quedaría de la siguiente manera:
~~~ 
<p>
	<strong>Image:</strong>
	<%= image_tag book.image, width: 200 if book.image.attached? %>
</p>
~~~
La imagen está lista para ser mostrada con un ancho de 200px, el condicional después del 200 es para verificar si existe alguna imagen adjunta, evitando que se rompa la aplicación al querer mostrar una imagen no existente.
Como dato adicional, se debe asegurar que dentro de `books_controller` en el método `book_params` se debe permitir el registro del campo `:image` con esto se generan los permisos para poder guardar la imágen.

## Probando la aplicación

Ejecutamos el servidor con:

rails server

y se dirige a la dirección: `https://localhost:3000/books/new` se llena los campos del formulario y adjuntamos un imagen del computador local, el registro del libro se debe realizar satisfactoriamente..

![image](https://user-images.githubusercontent.com/91301423/200691278-c1b2993c-7593-4566-adc0-85cd7c1e9bd2.png)

Once the form is submitted, the new book with its image should be shown.

![image](https://user-images.githubusercontent.com/91301423/200692236-baea56eb-fe49-4a95-beb1-430484b549a1.png)

 
 
## En resumen
 
Active Storage facilita y reduce la cantidad de código que se utiliza para administrar imágenes adjuntas.

En el artículo de hoy se ha realizado la configuración de Active Storage con Rails 7 y se lo probó utilizando un formulario básico.
 
