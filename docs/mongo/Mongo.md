# MongoDB

MongoDB es un sistema de gestión de bases de datos NoSQL **orientado a documentos**.
A diferencia de las bases de datos relacionales, que almacenan la información en tablas con filas y columnas, MongoDB guarda los datos en **colecciones** formadas por documentos en formato **BSON** (una representación binaria de JSON).

Cada documento es una **estructura flexible**, parecida a un objeto de programación, donde los datos se organizan en pares **clave–valor**.
Esta flexibilidad permite que cada **documento** tenga una estructura diferente, lo que hace que MongoDB se adapte fácilmente a los cambios en los datos sin necesidad de modificar esquemas.

📂 **Estructura básica**

| Concepto       | Equivalente en BD relacional | Descripción                                      |
|----------------|------------------------------|--------------------------------------------------|
| **Base de datos** | Base de datos                | Conjunto de colecciones.                         |
| **Colección**     | Tabla                       | Agrupación de documentos relacionados.           |
| **Documento**     | Fila (registro)             | Unidad básica de almacenamiento. Es un objeto JSON. |
| **Campo**         | Columna                    | Atributo dentro del documento.                   |

!!!Tip "Ejemplo de estructura flexible"
    Veamos algunos **ejemplos** de documentos JSON para guardar la información de **libros y autores**.

Dependiendo de cómo se deba acceder a la información, podemos plantearnos guardar los libros con sus autores, o guardar los autores con sus libros.
Incluso podríamos guardar ambas versiones, para poder acceder a los datos de todas las formas posibles, aunque eso implique duplicar la información.

De la primera manera, guardando los libros junto con su autor, podríamos tener documentos con esta estructura, que se podrían guardar en una colección llamada **Libros**:


    {  
        _id:101,  
        titol:"El secret de Khadrell",  
        autor: {  
        nom:"Pep",  
        cognoms:"Castellano Puchol",  
        any_naixement:1960  
        },  
        isbn:"84-95620-72-3"  
    },  
    {  
        _id:102,  
        titol:"L'Ombra del Vent",  
        autor: {  
        nom:"Carlos",  
        cognoms:"Ruiz Zafon",  
        pais:"Espanya"  
        },  
        pagines:490,  
        editorial:"Planeta"  
    }

Observa cómo los objetos no tienen por qué tener la misma estructura.
La forma de acceder al nombre de un autor sería la siguiente:
**objeto.autor.nombre**

Una manera alternativa de guardar la información, como habíamos comentado antes, sería organizarla por autores, junto con sus libros.
De este modo, podríamos ir completando la colección **Autores** con uno o más documentos de este estilo:   

    {  
        _id: 201,  
        nom:"Pep",  
        cognoms:"Castellano Puchol",  
        any_naixement:1960,  
        llibres: [  
        {  
        titol:"El secret de Khadrell",  
        isbn:"84-95620-72-3"  
        },  
        {  
        titol:"Habitació 502",  
        editorial:"Tabarca"  
        }  
    ]  
    },  
    {  
        _id:202,  
        nom:"Carlos",  
        cognoms:"Ruiz Zafon",  
        pais:"Espanya",  
        llibres: [  
        {  
            titol:"L'Ombra del Vent",  
            pagines:490,  
            editorial:"Planeta"  
        }  
    ]  
    }

Observa cómo, para un autor, ahora tenemos un array (los corchetes: [ ]) con sus libros.

!!!Tip "¿Cuál de las dos formas es mejor para guardar la información?"
    Pues depende del tipo de acceso que se vaya a realizar a los datos.
    La mejor opción será probablemente aquella que, según las consultas que se necesiten hacer, devuelva la información de forma más rápida.    