# Conexión

## Desde Kotlin

Opción 1: Driver oficial de MongoDB (para Java/Kotlin)

Gradle

    implementation("org.mongodb:mongodb-driver-sync:5.2.0")


    import com.mongodb.client.MongoClients
    import com.mongodb.client.MongoCollection
    import com.mongodb.client.MongoDatabase
    import org.bson.Document

    fun main() {
        // 1️⃣ Conexión al servidor local
        val uri = "mongodb://localhost:27017"
        val client = MongoClients.create(uri)

        // 2️⃣ Seleccionar base de datos
        val database: MongoDatabase = client.getDatabase("biblioteca")

        // 3️⃣ Seleccionar colección
        val coleccion: MongoCollection<Document> = database.getCollection("libros")

        // 4️⃣ Insertar un documento
        val doc = Document("titulo", "1984")
            .append("autor", "George Orwell")
            .append("año", 1949)

        coleccion.insertOne(doc)
        println("📚 Documento insertado correctamente.")

        // 5️⃣ Leer documentos
        for (libro in coleccion.find()) {
            println(libro.toJson())
        }

        client.close()
    }


Opción 2: KMongo (simplifica mucho la sintaxis y aprovecha las data class de Kotlin.)

Gradle

    implementation("org.litote.kmongo:kmongo:5.1.0")


        import org.litote.kmongo.*
        import org.litote.kmongo.client.MongoClient

        data class Libro(val titulo: String, val autor: String, val año: Int)

        fun main() {
            // 1️⃣ Crear conexión
            val client = KMongo.createClient() // Por defecto: mongodb://localhost:27017
            val database = client.getDatabase("biblioteca")
            val coleccion = database.getCollection<Libro>()

            // 2️⃣ Insertar datos
            val libro = Libro("Fahrenheit 451", "Ray Bradbury", 1953)
            coleccion.insertOne(libro)
            println("🔥 Libro insertado correctamente")

            // 3️⃣ Consultar datos
            val resultados = coleccion.find()
            resultados.forEach { println(it) }

            // 4️⃣ Filtrar datos
            val buscado = coleccion.findOne(Libro::autor eq "Ray Bradbury")
            println("🔎 Encontrado: $buscado")

            client.close()
        }

**Ejemplo de CRUD completo con KMongo**

        import org.litote.kmongo.*

        data class Alumno(val nombre: String, val nota: Double)

        fun main() {
            val client = KMongo.createClient()
            val db = client.getDatabase("instituto")
            val alumnos = db.getCollection<Alumno>()

            // Insertar
            alumnos.insertOne(Alumno("Ana", 8.5))
            alumnos.insertOne(Alumno("Luis", 6.7))

            // Listar
            println("📋 Todos los alumnos:")
            alumnos.find().forEach { println(it) }

            // Actualizar
            alumnos.updateOne(Alumno::nombre eq "Luis", setValue(Alumno::nota, 7.5))

            // Eliminar
            alumnos.deleteOne(Alumno::nombre eq "Ana")

            client.close()
        }


## Conexión a MongoDB Atlas (nube)

Solo cambia la URI de conexión.

Copia la URL que te ofrece Atlas en la sección Connect → Drivers.

    val uri = "mongodb+srv://usuario:password@cluster0.mongodb.net/?retryWrites=true&w=majority"
    val client = MongoClients.create(uri)
    val database = client.getDatabase("biblioteca")


⚠️ Asegúrate de:

- Añadir tu IP a la lista blanca de Atlas.
- Usar un usuario con permisos de lectura/escritura.
- Especificar correctamente el nombre de la base.


