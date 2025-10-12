# 🟢 Comandos de MongoDB y su utilización

---

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

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `show collections` | Lista todas las colecciones de la base de datos. | `show collections` |
| `db.createCollection("nombre")` | Crea una colección vacía. | `db.createCollection("alumnos")` |
| `db.coleccion.drop()` | Elimina una colección completa. | `db.alumnos.drop()` |
| `db.coleccion.renameCollection("nuevoNombre")` | Cambia el nombre de una colección. | `db.alumnos.renameCollection("estudiantes")` |

---

## 🔹 Operaciones básicas

### 🔹Inserción

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `insertOne()` | Inserta un solo documento. | `db.alumnos.insertOne({nombre:"Ana", nota:8})` |
| `insertMany()` | Inserta varios documentos a la vez. | `db.alumnos.insertMany([{nombre:"Luis", nota:7},{nombre:"Marta", nota:9}])` |

> 📌 Si la colección no existe, MongoDB la **creará automáticamente** en el momento de la inserción.

---

### 🔹Búsqueda

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `find()` | Devuelve todos los documentos de la colección. | `db.alumnos.find()` |
| `findOne()` | Devuelve el primer documento que cumple una condición. | `db.alumnos.findOne({nombre:"Ana"})` |
| `find(criterio, proyección)` | Permite filtrar y mostrar solo algunos campos. | `db.alumnos.find({nota:{$gte:8}}, {nombre:1, _id:0})` |

📘 **Operadores comunes**:  
`$eq` (igual), `$ne` (distinto), `$gt` (mayor que), `$lt` (menor que), `$gte` (mayor o igual), `$lte` (menor o igual), `$in`, `$and`, `$or`.

---

### 🔹 Actualización

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `updateOne(filtro, cambios)` | Actualiza el primer documento que cumpla la condición. | `db.alumnos.updateOne({nombre:"Ana"}, {$set:{nota:9}})` |
| `updateMany(filtro, cambios)` | Actualiza todos los documentos que cumplan la condición. | `db.alumnos.updateMany({nota:{$lt:5}}, {$set:{aprobado:false}})` |
| `replaceOne(filtro, nuevoDoc)` | Sustituye el documento completo. | `db.alumnos.replaceOne({nombre:"Ana"}, {nombre:"Ana", nota:10})` |

> ⚠️ Usa `$set` para modificar solo algunos campos y **no perder el resto**.

---

### 🔹 Eliminación

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `deleteOne()` | Elimina el primer documento que cumpla la condición. | `db.alumnos.deleteOne({nombre:"Luis"})` |
| `deleteMany()` | Elimina todos los documentos que cumplan la condición. | `db.alumnos.deleteMany({nota:{$lt:5}})` |

---

## 🔹 Consultas avanzadas y ordenación

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `sort()` | Ordena los resultados. `1` ascendente, `-1` descendente. | `db.alumnos.find().sort({nota:-1})` |
| `limit()` | Limita el número de resultados. | `db.alumnos.find().limit(3)` |
| `countDocuments()` | Devuelve el número de documentos que cumplen un filtro. | `db.alumnos.countDocuments({nota:{$gte:5}})` |

---

## 🔹 Índices

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `createIndex({campo:1})` | Crea un índice ascendente. | `db.alumnos.createIndex({nombre:1})` |
| `getIndexes()` | Muestra los índices existentes. | `db.alumnos.getIndexes()` |
| `dropIndex("nombre_1")` | Elimina un índice. | `db.alumnos.dropIndex("nombre_1")` |

---

## 🔹 Información útil del entorno

| Comando | Descripción | Ejemplo |
|----------|--------------|----------|
| `db.stats()` | Muestra estadísticas sobre la base de datos. | `db.stats()` |
| `db.coleccion.stats()` | Muestra estadísticas sobre una colección. | `db.alumnos.stats()` |
| `db.version()` | Devuelve la versión de MongoDB. | `db.version()` |

---

## 🔹 Ejemplo completo


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


| Etapa | Descripción | Ejemplo |
|--------|--------------|----------|
| `$match` | Filtra documentos (equivalente a `WHERE`). | `{ $match: { ciudad: "Valencia" } }` |
| `$project` | Selecciona campos específicos o crea nuevos. | `{ $project: { _id:0, nombre:1, nota:1 } }` |
| `$sort` | Ordena los resultados. | `{ $sort: { nota: -1 } }` |
| `$limit` | Limita el número de resultados. | `{ $limit: 5 }` |
| `$skip` | Omite un número de documentos. | `{ $skip: 10 }` |
| `$group` | Agrupa los documentos por un campo y calcula valores agregados (como `COUNT`, `SUM`, `AVG`). | `{ $group: { _id: "$curso", media: { $avg: "$nota" } } }` |
| `$count` | Devuelve el número total de documentos resultantes. | `{ $count: "total" }` |
| `$lookup` | Realiza una unión entre colecciones (similar a `JOIN`). | `{ $lookup: { from: "profesores", localField: "idProfesor", foreignField: "_id", as: "infoProfesor" } }` |
| `$unwind` | Descompone arrays en múltiples documentos. | `{ $unwind: "$aficiones" }` |


## ✅ Resumen visual

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

