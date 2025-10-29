# 🔹Conexión


Para conectar una aplicación escrita en **Kotlin** con una base de datos **MongoDB**, existen dos opciones principales, según el objetivo y el nivel de control que se necesite sobre las operaciones:

1️⃣ Conexión con el controlador oficial de MongoDB (**MongoDB Driver**)

Es la opción más directa y de bajo nivel. Permite trabajar con las clases del paquete com.mongodb o org.bson para realizar operaciones CRUD (crear, leer, actualizar y eliminar) sobre las colecciones.
Se utiliza cuando se quiere tener control total sobre las consultas, la conexión y la conversión de documentos BSON a objetos Kotlin.

2️⃣ Conexión mediante una librería de mapeo (ORM/ODM), como **KMongo**

KMongo es una capa de abstracción sobre el driver oficial, diseñada específicamente para Kotlin.
Permite trabajar con data classes y consultas tipadas, simplificando el código y haciéndolo más idiomático.
Es la opción más recomendada para proyectos educativos o empresariales donde se busca claridad, seguridad de tipos y menor código repetitivo.

A continuación veremos un ejemplo práctico de cada una de las dos formas de conectar Kotlin con MongoDB. Ambos ejemplos se ejecutarán sobre MongoDB Community Server en local y se crearán con Gradle desde IntelliJ.

## 🔹Kotlin + Gradle + Driver oficial

**1. Crea un nuevo proyecto**{.azul}

Al nuevo proyecto lo llamaremos, por ejemplo, BDNoSQL.

![alt text](../img/mongo1.png)

**2. Añadir las dependencias en Gradle**{.azul}

En el archivo **build.gradle.kts**, dentro del bloque dependencies, añadimos:

        dependencies {
            implementation("org.mongodb:mongodb-driver-sync:5.2.0")
        }

**3. Ejemplo sobre la colección Libros**{.azul}
    
Crea un archivo **Main.kt** en un paquete nuevo llamado **mongo** con el siguiente código:


![alt text](../img/mongo2.png)


    import com.mongodb.client.MongoClients
    import com.mongodb.client.MongoCollection
    import com.mongodb.client.MongoDatabase
    import org.bson.Document

    fun main() {
        // 1Conexión al servidor local
        val uri = "mongodb://localhost:27017"
        val client = MongoClients.create(uri)

        // Seleccionar base de datos
        val database: MongoDatabase = client.getDatabase("biblioteca")

        // Seleccionar colección
        val coleccion: MongoCollection<Document> = database.getCollection("libros")

        // Insertar un documento
        val doc = Document("titulo", "1984")
            .append("autor", "George Orwell")
            .append("año", 1949)

        coleccion.insertOne(doc)
        println("Documento insertado correctamente.")

        // Leer documentos
        for (libro in coleccion.find()) {
            println(libro.toJson())
        }

        client.close()
    }


!!!Warning "Advertencia"
    Al ejecutar el programa os aparecerá el siguiene mensaje:

    ![alt text](../img/mongo3_msg.png) 

    Esto no es un error, solo una advertencia. MongoDB intenta usar **SLF4J** (Simple Logging Facade for Java) para mostrar mensajes de registro (logs) sobre la conexión, operaciones, etc.  
    Como tu proyecto no incluye ninguna librería de logging, te avisa de que no podrá mostrar esos logs internos, pero el programa sigue funcionando perfectamente.
   


## 🔹Kotlin + Gradle + KMongo

**1. Añadir las dependencias en Gradle**{.azul}

En el **archivo build.gradle.kts**, dentro del bloque dependencies, añadimos:

        dependencies {
            implementation("org.litote.kmongo:kmongo:5.1.0")
        }

**2. Ejemplo sobre la colección Libros**{.azul}

   
Crea un archivo **Main.kt** en un paquete nuevo llamado **Kmongo** con el siguiente código:

![alt text](../img/mongo4.png)

        import org.litote.kmongo.*

        data class Libros(val titulo: String, val autor: String, val año: Int)

        fun main() {
            // Crear conexión
            val client = KMongo.createClient() // Por defecto: mongodb://localhost:27017
            val database = client.getDatabase("biblioteca")
            val coleccion = database.getCollection<Libros>()

            // Insertar datos
            val libro = Libros("Fahrenheit 451", "Ray Bradbury", 1953)
            coleccion.insertOne(libro)
            println("Libro insertado correctamente")

            // Consultar datos
            val resultados = coleccion.find()
            resultados.forEach { println(it) }
        
            client.close()
        }


!!!Note "Nota"
    Aparecerán los dos libros insertados, tanto en el ejemplo anterior como en este, ya que la colección Libros es la misma.

    ![alt text](../img/mongo4_msg.png)


## 🔹Control de errores 

Cuando un programa se conecta a una base de datos como MongoDB, siempre existe la posibilidad de que ocurran errores: la base de datos puede no estar disponible, los datos pueden ser incorrectos o una operación puede fallar por un conflicto o por formato inválido.

Por eso, es importante incluir control de errores en el código, especialmente en las operaciones de conexión y en las operaciones CRUD (insertar, leer, actualizar, eliminar).

El control de errores se hace con bloques **try-catch**. Así puedes capturar las excepciones y evitar que el programa se detenga bruscamente.

**Buenas prácticas:**

- Verificar que el servidor MongoDB está en ejecución antes de conectar.
- Usar **try-catch** en las secciones críticas del programa (especialmente conexión e inserción).
- Mostrar mensajes claros al usuario para facilitar el diagnóstico.
- Cerrar el cliente con client.close() en un bloque **finally** si se usa una conexión persistente.

        try {
            val client = KMongo.createClient() // Por defecto: mongodb://localhost:27017
            val database = client.getDatabase("biblioteca")
            val coleccion = database.getCollection<Libros>()

            // Insertar datos
            val libro = Libros("Fahrenheit 451", "Ray Bradbury", 1953)
            coleccion.insertOne(libro)
            println("Libro insertado correctamente")

            // Consultar datos
            val resultados = coleccion.find()
            resultados.forEach { println(it) }
        
            client.close()

        } catch (e: Exception) {
            println("⚠️ Error al acceder a MongoDB: ${e.message}")
        
        } finally { //si se usa una conexión persistente.
            // Cierre seguro del cliente
            client?.close()
            println("🔒 Conexión cerrada.")
        }    
    


 <!--  

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
-->
<!--
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

-->
