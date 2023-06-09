# MongoDB - Avanzado

## Trabajando con subdocumentos
Supongamos que tenemos una colección de pedidos, donde nos llegan los pedidos de una tienda online
```
db.pedidos.insertOne({
  cliente: {
    nombre: "Juan Perez",
    email: "juanperez@example.com",
    direccion: "mz2 cs 1"
  },
  productos: [
    { _id: ObjectId(), nombre: "iPhone", precio : 1000 , categoria : "celulares" },
    { _id: ObjectId(),nombre: "iPad", precio : 800, categoria : "tablets" }
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

# Transacciones en mongo

```
var session = db.getMongo().startSession();
session.startTransaction({ readConcern: { level: 'snapshot'}, writeConcern: { w: 'majority' } });
 

try{

    var cliente = db.cuentas.findOne({ _id: ObjectId("64408b6889917b0a95d9ccf1") });
    var initialBalance = cliente.balance; 
    var updatedBalance = initialBalance + 100; 
    db.cuentas.updateOne( { _id: ObjectId("64408b6889917b0a95d9ccf1") }, { $set: {balance: updatedBalance } })

    var cliente2 = db.cuentas.findOne({ _id: ObjectId("64408b6889917b0a95d9ccf2") });
    var initialBalance2 = cliente2.balance; 
    var updatedBalance2 = initialBalance2 + 1000; 
    db.cuentas.updateOne( { _id: ObjectId("64408b6889917b0a95d9ccf2") }, { $set: {balance: updatedBalance2 } })

    session.commitTransaction(); 
    print("ok");
    db.cuentas.find({})    
}
catch (error){
    print("se presento un error", error);
    session.abortTransaction(); 
}
finally{
    session.endSession();
}
```
