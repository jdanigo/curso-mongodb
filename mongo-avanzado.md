# MongoDB - Avanzado

## Trabajando con subdocumentos
Supongamos que tenemos una colección de pedidos, donde nos llegan los pedidos de una tienda online
```
db.pedidos.insertOne({
  cliente: {
    nombre: "Juan Perez",
    email: "juanperez@example.com"
  },
  productos: [
    { nombre: "iPhone", precio: 1000 },
    { nombre: "iPad", precio: 800 }
  ]
})
```

## Operadores para trabajar con subdocumentos

|Operador|Definicion
|-------|----------|
|$addToSet| Este operador se utiliza para agregar un elemento a un subdocumento siempre y cuando ese elemento no exista|
|$push|Este operador se utiliza para agregar elementos a una lista dentro de un subdocumento, no importa si existe o no|
|$elemMatch| Este operador se utiliza para buscar documentos que contengan un subdocumento específico que cumpla con ciertas condiciones|
|$pull|Este operador se utiliza para eliminar elementos de una lista dentro de un subdocumento|
|$pop| Este operador se utiliza para eliminar el primer o el último elemento de una lista dentro de un subdocumento. |
|$unset|Este operador se utiliza para eliminar un campo específico de un subdocumento|

## $addToSet
```
db.pedidos.updateOne({ "_id": ObjectId("643df9c389917b0a95d9cce7") }, { "$addToSet": { "productos": {nombre: 'Macbook Pro', precio: 2000} } })
```
## $push
```
db.pedidos.updateOne({ "_id": ObjectId("643df9c389917b0a95d9cce7") }, { "$push": { "productos": {nombre: 'Macbook Pro', precio: 2500} } })
```
## $elemMatch
```
db.pedidos.find({ "productos": {  "$elemMatch": { nombre: 'Macbook Pro'} } } )
```

## $pull
```
db.pedidos.updateOne({ "_id": ObjectId("643df9c389917b0a95d9cce7") }, { "$pull": { "productos" : { nombre: 'iPad'} } })
```

## $pop
```
db.pedidos.updateOne({ "_id": ObjectId("64407fb889917b0a95d9cce9") }, { "$pop": { "productos": 1 } } ) -> eliminará el último registro
db.pedidos.updateOne({ "_id": ObjectId("64407fb889917b0a95d9cce9") }, { "$pop": { "productos": -1 } } ) -> eliminará el primer registro

```

## $unset
```
db.pedidos.updateOne({ "_id": ObjectId("64407fb889917b0a95d9cce9") },{ $unset: { "productos.$[].precio" : "" } } ) -> eliminar el campo dentro de un subdocumento
db.pedidos.updateOne({ "_id": ObjectId("64407fb889917b0a95d9cce9") },{ $unset: { "productos" : "" } } ) -> eliminar el campo completo de un documento
```

## Operadores de consulta

|Operador|Definicion
|-------|----------|
|$eq| igual a|
|$ne| diferente a|
|$gt| mayor que|
|$gte| mayor o igual que|
|$lt| menor que|
|$lte| menor o igual que|
|$in| en una lista|
|$nin| diferente en la lista|
|$regex|coincide con una expresión regular|


## $lt y $lte
Primero insertamos los registros:
```
db.clientes.insertMany([
{
  nombres: "Julian",
  apellidos: "Sanchez"  ,
  telefono: "3122898647", 
  edad: 40  
},
{
  nombres: "Jorge",
  apellidos: "Rueda",
  telefono: "3122898647",
  edad: 20  
},
{
  nombres: "Stiven",
  apellidos: "Ortiz",
  telefono: "3122898647",
  edad: 28  
},
{
  nombres: "Esteban",
  apellidos: "Dido",
  telefono: "3122898647",
  edad: 46  
},
{
  nombres: "Luis",
  apellidos: "Fuentes",
  telefono: "3122898647",
  edad: 47
}
])
```
Luego ejecutamos la query
```
db.clientes.find({edad: {$lt: 28}})
db.clientes.find({edad: {$lte: 28}})
```
