# 🔹 Comandos de MongoDB y su utilización

---

Los comandos básicos de MongoDB ya se trabajaron en 1.º de DAM, cuando se introdujo el modelo NoSQL y las operaciones fundamentales sobre colecciones y documentos.

En este tema no se pretende volver a explicarlos desde cero, sino ofrecer un resumen práctico y actualizado con los comandos más útiles para consultar, ordenar, filtrar y gestionar datos en MongoDB, que te servirá como recordatorio y punto de partida para los ejercicios de este módulo.

MongoDB utiliza su propia **shell interactiva**, llamada **`mongosh`**, que permite ejecutar comandos para administrar bases de datos, colecciones y documentos.  

Su sintaxis es **muy similar a JavaScript**, ya que cada comando se ejecuta sobre un **objeto base**:


    db.coleccion.operacion()

- db → representa la base de datos actual.
- coleccion → el nombre de la colección sobre la que actuamos.
- operacion() → el comando que deseamos ejecutar.


## 🔹 Sobre bases de datos

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `show dbs` | Muestra todas las bases de datos existentes. | `show dbs` |
| `use <nombre>` | Cambia a una base de datos (la crea si no existe). | `use biblioteca` |
| `db.getName()` | Muestra el nombre de la base de datos actual. | `db.getName()` |
| `db.dropDatabase()` | Elimina la base de datos actual. | `db.dropDatabase()` |

---

## 🔹 Sobre colecciones

| Comando | Descripción |
|----------|--------------|
| `show collections` | Lista todas las colecciones de la base de datos.<br>**Ejemplo:** `show collections` |
| `db.createCollection("nombre")` | Crea una colección vacía.<br>**Ejemplo:** `db.createCollection("alumnos")` |
| `db.coleccion.drop()` | Elimina una colección completa.<br>**Ejemplo:** `db.alumnos.drop()` |
| `db.coleccion.renameCollection("nuevoNombre")` | Cambia el nombre de una colección.<br>**Ejemplo:** `db.alumnos.renameCollection("estudiantes")` |

---

## 🔹 Operaciones básicas

### 🔹Inserción

| Comando | Descripción |
|----------|--------------|
| `insertOne()` | Inserta un solo documento.<br>**Ejemplo:** `db.alumnos.insertOne({nombre:"Ana", nota:8})` |
| `insertMany()` | Inserta varios documentos a la vez.<br>**Ejemplo:** `db.alumnos.insertMany([{nombre:"Luis", nota:7}, {nombre:"Marta", nota:9}])` |


> 📌 Si la colección no existe, MongoDB la **creará automáticamente** en el momento de la inserción.

---

### 🔹Búsqueda

| Comando | Descripción |
|----------|--------------|
| `find()` | Devuelve todos los documentos de la colección.<br>**Ejemplo:** `db.alumnos.find()` |
| `findOne()` | Devuelve el primer documento que cumple una condición.<br>**Ejemplo:** `db.alumnos.findOne({nombre:"Ana"})` |
| `find(criterio, proyección)` | Permite filtrar y mostrar solo algunos campos.<br>**Ejemplo:** `db.alumnos.find({nota:{$gte:8}}, {nombre:1, _id:0})` |


📘 **Operadores comunes**:  
`$eq` (igual), `$ne` (distinto), `$gt` (mayor que), `$lt` (menor que), `$gte` (mayor o igual), `$lte` (menor o igual), `$in`, `$and`, `$or`.

---

### 🔹 Actualización

| Comando | Descripción |
|----------|--------------|
| `updateOne(filtro, cambios)` | Actualiza el primer documento que cumpla la condición.<br>**Ejemplo:** `db.alumnos.updateOne({nombre:"Ana"}, {$set:{nota:9}})` |
| `updateMany(filtro, cambios)` | Actualiza todos los documentos que cumplan la condición.<br>**Ejemplo:** `db.alumnos.updateMany({nota:{$lt:5}}, {$set:{aprobado:false}})` |
| `replaceOne(filtro, nuevoDoc)` | Sustituye el documento completo.<br>**Ejemplo:** `db.alumnos.replaceOne({nombre:"Ana"}, {nombre:"Ana", nota:10})` |


> ⚠️ Usa `$set` para modificar solo algunos campos y **no perder el resto**.

---

### 🔹 Eliminación

| Comando | Descripción |
|----------|--------------|
| `deleteOne()` | Elimina el primer documento que cumpla la condición.<br>**Ejemplo:** `db.alumnos.deleteOne({nombre:"Luis"})` |
| `deleteMany()` | Elimina todos los documentos que cumplan la condición.<br>**Ejemplo:** `db.alumnos.deleteMany({nota:{$lt:5}})` |

---

**Ejemplo**{.azul}


    use biblioteca
    db.libros.insertMany([
    {titulo:"1984", autor:"Orwell", año:1949},
    {titulo:"Fahrenheit 451", autor:"Bradbury", año:1953}
    ])

    db.libros.find()
    db.libros.updateOne({titulo:"1984"}, {$set:{año:1950}})
    db.libros.find({año:{$gte:1950}})
    db.libros.deleteOne({titulo:"Fahrenheit 451"})


---

## 🔹 Consultas avanzadas y ordenación

| Comando | Descripción |
|----------|--------------|
| `sort()` | Ordena los resultados. `1` ascendente, `-1` descendente.<br>**Ejemplo:** `db.alumnos.find().sort({nota:-1})` |
| `limit()` | Limita el número de resultados.<br>**Ejemplo:** `db.alumnos.find().limit(3)` |
| `countDocuments()` | Devuelve el número de documentos que cumplen un filtro.<br>**Ejemplo:** `db.alumnos.countDocuments({nota:{$gte:5}})` |


---

## 🔹 Índices

| Comando | Descripción |
|----------|--------------|
| `createIndex({campo:1})` | Crea un índice ascendente.<br>**Ejemplo:** `db.alumnos.createIndex({nombre:1})` |
| `getIndexes()` | Muestra los índices existentes.<br>**Ejemplo:** `db.alumnos.getIndexes()` |
| `dropIndex("nombre_1")` | Elimina un índice.<br>**Ejemplo:** `db.alumnos.dropIndex("nombre_1")` |

---

## 🔹 Información útil del entorno

| Comando | Descripción |
|----------|--------------|
| `db.stats()` | Muestra estadísticas sobre la base de datos.<br>**Ejemplo:** `db.stats()` |
| `db.coleccion.stats()` | Muestra estadísticas sobre una colección.<br>**Ejemplo:** `db.alumnos.stats()` |
| `db.version()` | Devuelve la versión de MongoDB.<br>**Ejemplo:** `db.version()` |


---


## 🔹 Consultas avanzadas con `aggregate()`

El método **`aggregate()`** permite realizar **consultas complejas** y **procesamientos de datos** en varias etapas, similares a las funciones de **GROUP BY, JOIN o HAVING** en SQL.  
Cada etapa del *pipeline* (tubería) transforma los datos paso a paso.

---

**Estructura básica**

    db.coleccion.aggregate([
    { <etapa1> },
    { <etapa2> },
    ...
    ])

Cada etapa (stage) se representa mediante un objeto precedido por $, que indica la operación a realizar.


| Etapa | Descripción |
|--------|--------------|
| `$match` | Filtra documentos (equivalente a `WHERE`).<br>**Ejemplo:** `{ $match: { ciudad: "Valencia" } }` |
| `$project` | Selecciona campos específicos o crea nuevos.<br>**Ejemplo:** `{ $project: { _id:0, nombre:1, nota:1 } }` |
| `$sort` | Ordena los resultados.<br>**Ejemplo:** `{ $sort: { nota: -1 } }` |
| `$limit` | Limita el número de resultados.<br>**Ejemplo:** `{ $limit: 5 }` |
| `$skip` | Omite un número de documentos.<br>**Ejemplo:** `{ $skip: 10 }` |
| `$group` | Agrupa los documentos por un campo y calcula valores agregados (como `COUNT`, `SUM`, `AVG`).<br>**Ejemplo:** `{ $group: { _id: "$curso", media: { $avg: "$nota" } } }` |
| `$count` | Devuelve el número total de documentos resultantes.<br>**Ejemplo:** `{ $count: "total" }` |
| `$lookup` | Realiza una unión entre colecciones (similar a `JOIN`).<br>**Ejemplo:** `{ $lookup: { from: "profesores", localField: "idProfesor", foreignField: "_id", as: "infoProfesor" } }` |
| `$unwind` | Descompone arrays en múltiples documentos.<br>**Ejemplo:** `{ $unwind: "$aficiones" }` |



**Ejemplo**{.azul}

Supongamos que quieres agrupar los libros por autor y calcular el año promedio de publicación de sus obras:

    db.libros.aggregate([
    {
        $group: {
        _id: "$autor",              // agrupamos por autor
        promedioAño: { $avg: "$año" }, // promedio de años
        cantidadLibros: { $sum: 1 }    // número de libros por autor
        }
    }
    ])



## ✅ Resumen

| Categoría | Comandos clave |
|------------|----------------|
| 📚 **Base de datos** | `show dbs`, `use`, `db.getName()`, `db.dropDatabase()` |
| 📂 **Colecciones** | `show collections`, `db.createCollection()`, `db.coleccion.drop()` |
| ➕ **Inserción** | `db.coleccion.insertOne()`, `db.coleccion.insertMany()` |
| 🔍 **Consulta** | `db.coleccion.find()`, `db.coleccion.findOne()`, `.sort()`, `.limit()` |
| ✏️ **Actualización** | `db.coleccion.updateOne()`, `db.coleccion.updateMany()`, `$set` |
| ❌ **Eliminación** | `db.coleccion.deleteOne()`, `db.coleccion.deleteMany()` |
| 🧩 **Índices** | `db.coleccion.createIndex()`, `db.coleccion.getIndexes()`, `db.coleccion.dropIndex()` |
| 📈 **Estadísticas** | `db.stats()`, `db.coleccion.stats()`, `db.version()` |

