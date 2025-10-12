# Instalación

Hoy en día existen múltiples opciones para trabajar con MongoDB, tanto a nivel de instalación como de desarrollo y administración.
Aquí tienes un resumen actualizado, organizado por categorías 👇

⚙️ **1. Opciones de instalación y despliegue**

| Opción | Descripción | Ideal para |
|--------|--------------|-------------|
| 🖥️ **MongoDB Community Server** | Versión gratuita que se instala localmente en Windows, Linux o macOS. | Prácticas locales, entornos educativos. |
| 🐳 **MongoDB en Docker** | Se ejecuta como contenedor con `docker-compose` o comandos `docker run`. | Entornos de desarrollo rápidos y reproducibles. |
| ☁️ **MongoDB Atlas** | Servicio en la nube oficial de MongoDB. Permite crear clústeres gratuitos o de pago, gestionados por Mongo. | Proyectos web, microservicios, despliegues reales. |
| 🧩 **MongoDB Local + Atlas Sync** | Permite sincronizar datos locales con una base remota en Atlas. | Aplicaciones con modo offline/online. |

💻 **2. Herramientas de administración y visualización**

| Herramienta | Tipo | Descripción |
|--------------|------|--------------|
| 🧭 **MongoDB Compass** | GUI oficial | Interfaz gráfica para consultar, insertar y analizar datos. |
| 🐘 **DBeaver** | GUI universal | Permite conectarse a Mongo y a otras bases de datos (SQL y NoSQL). |
| 🧑‍💻 **Robo 3T** *(antiguo Robomongo)* | GUI ligera | Muy utilizada para tareas básicas de exploración. |
| 📜 **mongosh** | Consola oficial | Shell de comandos moderno (sustituye a `mongo`). |


Podremos instalar MongoDB en cualquier plataforma, e incluso sin tener permisos de administrador, como veremos en el caso de Ubuntu.

También existe la posibilidad de crear un servidor en la nube, incluso de forma gratuita.
Es la opción que MongoDB recomienda por defecto, aunque nosotros no la utilizaremos.


## 🐧 Instalación en Linux

Para realizar la instalación más básica, podemos hacerlo sin permisos de administrador.
Si los tenemos, todo será más cómodo, pero si no, también es posible, como veremos y destacaremos a continuación.

⚙️ **Instalación del servidor (Linux)**{.azul}

Desde la página oficial de MongoDB: [https://www.mongodb.com/try/download/community
](https://www.mongodb.com/try/download/community) vamos al menú **Products → Community Edition → Community Server** y descargamos la versión apropiada para nuestro sistema operativo.

Observa que, en el caso de Linux, hay muchas versiones para distintas distribuciones.
Es recomendable elegir el paquete **.tgz**, ya que simplemente descomprimiendo el archivo se completa la instalación básica.

Por ejemplo, para Ubuntu 22.04 de 64 bits, el archivo sería: [https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu2204-8.0.5.tgz](https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu2204-8.0.5.tgz)

!!!Warning ""
    Recuerda comprobar siempre que la versión coincide con la de tu sistema operativo.

Sencillamente, descomprimiremos este archivo en el lugar que queramos, y con eso ya tendremos la instalación básica lista.

Por defecto, el directorio de la base de datos es:

    /data/db

El único problema que podríamos tener, si no somos administradores, es no tener permiso para crear ese directorio.
En ese caso, crearemos otro directorio alternativo y, en el momento de arrancar el servidor, le indicaremos esa ubicación.

▶️ La forma de arrancar el servidor será:

    <directori arrel MongoDB>./bin/mongod

Opcionalmente, podemos indicarle a MongoDB dónde se encuentra la base de datos (si no lo especificamos, asumirá por defecto que está en /data/db):

    <directorio raíz de MongoDB>/bin/mongod --dbpath <directorio_de_la_BD>

Resumiendo, y situados en el directorio donde hemos descomprimido MongoDB:

📁 Crear el directorio de datos

    mkdir /data  
    mkdir /data/db

▶️ Si no somos administradores, arrancamos el servidor así:    

    ./bin/mongod --dbpath ./data/db

🔐 Si somos administradores, simplemente lo iniciamos con:

    ./bin/mongod

La siguiente imagen ilustra esta segunda opción.

Corresponde a una versión anterior de MongoDB, pero el procedimiento es totalmente equivalente.

![alt text](../img/mongod.png)

!!!Note "Nota"
    Una vez que el servidor está en marcha, no debemos cerrar esa terminal, ya que al hacerlo detendríamos el servidor.



⚙️ **Instalación del cliente MongoShell (Linux)**{.azul}

Desde la página de MongoDB [https://www.mongodb.com/try/download/shell
](https://www.mongodb.com/try/download/shell) vamos al menú
**Products → Tools → MongoDB Shell**, y descargamos la versión apropiada para nuestro sistema operativo.

Observa que, en el caso de Linux, existen muchas versiones para diferentes distribuciones.
Es recomendable elegir el paquete **.tgz**, ya que simplemente descomprimiendo el archivo se completa la instalación.

En el caso de Ubuntu 22.04 de 64 bits, seleccionaremos la opción genérica “Linux 64”, ya que es la que ofrece el paquete .tgz.

El archivo correspondiente es: [https://downloads.mongodb.com/compass/mongosh-2.4.0-linux-x64.tgz](https://downloads.mongodb.com/compass/mongosh-2.4.0-linux-x64.tgz)

!!!Warning ""
    Recuerda asegurarte siempre de que la versión sea la correcta para tu sistema.

Sencillamente descomprimiremos este archivo en el lugar que queramos, y con eso ya tendremos hecha la instalación básica.

    <directori arrel Mongosh>/bin/mongosh

    ./bin/mongosh

![alt text](../img/mongosh.png)    


## 🪟 Instalación en Windows

⚙️ **Instalación del servidor (Windows)**{.azul}

La instalación no presenta ninguna dificultad.
Descargamos la versión correspondiente de MongoDB para Windows, que se distribuye como un archivo .msi ejecutable.

En el momento de redactar estos apuntes, la versión de 64 bits más reciente es la 8.0.5:
👉 https://fastdl.mongodb.org/windows/mongodb-windows-x86_64-8.0.5-signed.msi

Al igual que en el caso de Linux, antes de ejecutar el servidor debemos tener creado el directorio de datos.
Por defecto, este directorio será:

    \data\db

📁 Creación del directorio y arranque del servidor

Ejecuta los siguientes comandos en el Símbolo del sistema (cmd) o PowerShell:

    mkdir \data\db
    "C:\Program Files\MongoDB\Server\8.0\bin\mongod.exe"


Si todo es correcto, debería aparecer una ventana o consola indicando que el servidor está en ejecución, como en la siguiente imagen:

![alt text](../img/mongodW.png)


!!!Note "Nota"
    Si instalaste MongoDB utilizando el instalador MSI de MongoDB, normalmente el servicio ya quedará instalado automáticamente, por lo que no será necesario ejecutarlo manualmente.


💻 **Instalación del cliente Mongo Shell (Windows)**{.azul}

Para conectarnos como clientes, debemos hacerlo desde otra terminal, utilizando mongosh.exe, que es la interfaz de línea de comandos (CLI) oficial de MongoDB.

Esta herramienta permite interactuar con la base de datos mediante comandos en JavaScript.

🔽 Descarga

Descargamos la versión correspondiente de MongoDB Shell para Windows desde la página oficial:[https://www.mongodb.com/try/download/shell](https://www.mongodb.com/try/download/shell)

!!!Tip "Mongo Compass"
    También podéis descargar la versión MongoDB Compass, que es la herramienta gráfica oficial de MongoDB, la cual permite visualizar, explorar y administrar bases de datos MongoDB sin necesidad de utilizar la línea de comandos.

    [https://downloads.mongodb.com/compass/mongodb-compass-1.45.3-win32-x64.exe](https://downloads.mongodb.com/compass/mongodb-compass-1.45.3-win32-x64.exe)

**Probar el funcionament**{.azul}

Para probar su funcionamiento, vamos a ejecutar un par de comandos: uno para guardar un documento y otro para recuperarlo.

En cualquier operación, debemos escribir db seguido del nombre de la colección, y después la operación que queremos realizar.
Con el siguiente comando:

    db.ejemplo.insertOne({ msg: "Hola, ¿qué tal?" })


Nos responderá con:

    WriteResult({ "nInserted" : 1 })


Indicando que se ha insertado un documento en la colección ejemplo (si no existía, la creará automáticamente).

Y con el siguiente comando recuperamos la información:

    db.ejemplo.find()


Lo que nos devolverá algo como:

    { "_id" : ObjectId("56cc1acd73b559230de8f71b"), "msg" : "Hola, ¿qué tal?" }


Todo esto se realiza en la misma terminal, y cada uno de nosotros obtendrá un número diferente en el campo ObjectId.
En la siguiente imagen pueden verse las dos operaciones.

![alt text](../img/mongo_opW.png)

En realidad, estamos conectados a una base de datos llamada test.
Podemos crear y utilizar más de una base de datos, pero en este curso será más que suficiente trabajar con esta.

Para comprobarlo, podemos ejecutar la siguiente instrucción, que nos devuelve el nombre de la base de datos actual:

    db.getName()