# MongoDB - Comandos básicos en la shell

## Mostrar todas las bases de datos

```
show dbs
```

## Mostrar la base de datos actual
```
db
```
## Cambiarse de base de datos

```
use testdatabase
```

## Borrar base de datos
```
db.dropDatabase()
```
## Crear una colección de datos
```
db.createCollection('personas')
```
## Borrar una colección de datos
```
db.nombreColeccion.drop()
```

## Mostrar las colecciones de datos
```
show collections
```
