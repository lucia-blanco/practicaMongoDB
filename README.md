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
admin    0.000GB
timeline 0.000GB
config   0.000GB
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
#### Insertar
Para introducir un solo documento, lo hacemos como el apartado de arriba, pero para introducir varios, podemos hacerlo en forma de array:
```console
> var p1 = {category: "dog", user: "djsk_01", likes: 70, title: "Dog and its owner"}
> var p2 = {category: "dog", user: "poadka_02", likes: 500, title: "Lily with a sock"}
> var p3 = {category: "cat", user: "uaus_03", likes: 200, title: "Cat with moustache"}
> var p4 = {category: "cat", user: "uaus_03", likes: 300, title: "Cat with hat"}
> var p5 = {category: "parrot", user: "ebbe_04", likes: 18, title: "Ulises"}
> var p6 = {category: "dog", user: "poadka_02", likes: 0, title: "Sleepy Jere"}
> db.posts.insert([p1, p2, p3, p4, p5, p6])
BulkWriteResult({
	"writeErrors" : [ ],
	"writeConcernErrors" : [ ],
	"nInserted" : 6,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
```
Comprobamos que todo ha salido bien (usamos pretty() para verlo más bonito):
```console
> db.posts.find().pretty()
{
	"_id" : ObjectId("5afd2ac54ba327b0f5b4b8a9"),
	"category" : "bird",
	"user" : "djsk_01",
	"likes" : 100,
	"title" : "Upside-down duck"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8aa"),
	"category" : "dog",
	"user" : "djsk_01",
	"likes" : 70,
	"title" : "Dog and its owner"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ab"),
	"category" : "dog",
	"user" : "poadka_02",
	"likes" : 500,
	"title" : "Lily with a sock"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ac"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 200,
	"title" : "Cat with moustache"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ad"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 300,
	"title" : "Cat with hat"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae"),
	"category" : "parrot",
	"user" : "ebbe_04",
	"likes" : 18,
	"title" : "Ulises"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8af"),
	"category" : "dog",
	"user" : "poadka_02",
	"likes" : 0,
	"title" : "Sleepy Jere"
}

```
#### Modificar
Se puede modificar de varias maneras, una de ellas es con save():
```console
> db.posts.save({"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae"), "category":"bird", "user": "ebbe_04", "likes": 18, "title": "Ulises"})
WriteResult({
	"nMatched" : 0,
	"nUpserted" : 1,
	"nModified" : 0,
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae")
})

```
Comprobamos que salió bien (usamos findOne para que muestre solo el que hemos modificado:
```console
> db.posts.findOne({"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae")})
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae"),
	"category" : "bird",
	"user" : "ebbe_04",
	"likes" : 18,
	"title" : "Ulises"
}

```
También se puede hacer usando el método update:
```console
> q = db.posts.findOne({"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae")})
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae"),
	"category" : "bird",
	"user" : "ebbe_04",
	"likes" : 18,
	"title" : "Ulises"
}
> q.category = "bird"
bird
> db.posts.update({"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae")}, q)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 0 })
```
#### Borrar
Para borrar usaremos remove() poniendo entre los paréntesis la condición.
```console
db.posts.remove({"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8af")})
WriteResult({ "nRemoved" : 1 })
```

## 4. Crear un índice sobre un campo de la colección.
Para crear un índice, utilizamos ensureIndex(), junto con el campo sobre el que queremos crear el índice y 1 si queremos que se ascendiente, -1 si queremos que sea descendiente.
```console
> db.posts.ensureIndex({"user":1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
```
## 5. Realizar consultas en las que utilices igual, mayor y menor que.
Aquí se muestran los posts con menos de 50 likes:
```console
> db.posts.find({"likes":{$lt:50}}).pretty()
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ae"),
	"category" : "bird",
	"user" : "ebbe_04",
	"likes" : 18,
	"title" : "Ulises"
}
```
Aquí los que tienen más de 250:
```console
> db.posts.find({"likes":{$gt:250}}).pretty()
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ab"),
	"category" : "dog",
	"user" : "poadka_02",
	"likes" : 500,
	"title" : "Lily with a sock"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ad"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 300,
	"title" : "Cat with hat"
}
```
Y, por último, los que tienen exactamente 200:
```console
> db.posts.find({"likes":200}).pretty()
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ac"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 200,
	"title" : "Cat with moustache"
}
```
## 6. Realizar una consulta en la que los documentos aparezcan ordenados y se limite el número de estos mostrados.
Los 3 posts con más likes:
```console
> db.posts.find().sort({likes : -1} ).limit(3).pretty()
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ab"),
	"category" : "dog",
	"user" : "poadka_02",
	"likes" : 500,
	"title" : "Lily with a sock"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ad"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 300,
	"title" : "Cat with hat"
}
{
	"_id" : ObjectId("5afd2b1d4ba327b0f5b4b8ac"),
	"category" : "cat",
	"user" : "uaus_03",
	"likes" : 200,
	"title" : "Cat with moustache"
}
```
## 7. Realizar una consulta con agrupamiento y una función para mostrar la media, o suma, o la que tú decidas.
Agrupamos por usuario, y sumamos los likes:
```console
> db.posts.aggregate([{$group: {_id: "$user", totalLikes: {$sum: "$likes"}}}])
{ "_id" : "ebbe_04", "totalLikes" : 18 }
{ "_id" : "uaus_03", "totalLikes" : 500 }
{ "_id" : "poadka_02", "totalLikes" : 500 }
{ "_id" : "djsk_01", "totalLikes" : 170 }
```
Aquí agrupamos por categoría y la media de likes:
```console
> db.posts.aggregate([{$group: {_id: "$category", avgLikes: {$avg: "$likes"}}}])
{ "_id" : "cat", "avgLikes" : 250 }
{ "_id" : "dog", "avgLikes" : 285 }
{ "_id" : "bird", "avgLikes" : 59 }
```
 
