# Práctica MongoDB
Puedes encontrar el enunciado [aquí](https://github.com/IESCampanillas/practica-MongoDB-DAW/blob/master/Practica_MongoDB.pdf).
He extraido información de [aquí](https://docs.mongodb.com/tutorials/install-mongodb-on-ubuntu/) y [aquí](https://github.com/LuisJoseSanchez/mongodb/blob/master/README.md).

## 1. Crear una base de datos.
Se utiliza el mismo comando para crear la base de datos y para usarla.
```console
> use timeline 
switched to db gestion
```
Si cometimos algún error y queremos borrar la base de datos:
```console
 > db.dropDatabase() 
 { "dropped" : "timeline", "ok" : 1 }
```
Para saber en qué base de datos estamos trabajando:
```console
> db
timeline
```
Para ver las bases de datos que hemos creado:
```console
> show dbs
admin   0.000GB
chat    0.000GB
config  0.000GB
local   0.000GB
```

## 2. Tener una colección.
Se pueden crear de dos formas, la primera sería:
```console
> db.createCollection("posts")
{ "ok" : 1 }
```
También se pueden crear insertando directamente un documento:
```console
> db.posts.insert({category: "bird", user: "djsk_01", likes: 100, title: "Upside-down duck"})
WriteResult({ "nInserted" : 1 })
```
## 3. Insertar, modificar y borrar documentos en la colección.
Para introducir un solo documento, lo hacemos como el apartado de arriba, pero para introducir varios, podemos hacerlo en forma de array:
```console
> var p1 = 
```
## 4. Crear un índice sobre un campo de la colección.
## 5. Realizar consultas en las que utilices igual, mayor y menor que.
## 6. Realizar una consulta en la que los documentos aparezcan ordenados y se limite el número de estos mostrados.
## 7. Realizar una consulta con agrupamiento y una función para mostrar la media, o suma, o la que tú decidas.
 
