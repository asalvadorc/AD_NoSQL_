# JSON

**JSON** (JavaScript Object Notation) es un formato de texto ligero utilizado para almacenar e intercambiar información estructurada entre aplicaciones.
Aunque su sintaxis proviene de JavaScript, hoy en día es independiente del lenguaje y se usa ampliamente en entornos como Kotlin, Java, Python, Node.js, bases de datos NoSQL, APIs REST, etc.

Un fichero JSON está compuesto por **pares clave–valor**, donde:

- Las **claves** siempre van entre comillas dobles " ".
- Los **valores** pueden ser:

    - Cadenas de texto ("texto")
    - Números (42)
    - Booleanos (true o false)
    - Objetos (otro conjunto de pares clave-valor { ... })
    - Arrays o listas ([ ... ])
    - Valor nulo (null)

**Ejemplos de estructuras JSON**{.azul}

🔹 **Objeto simple**: Representa un único elemento con propiedades básicas.

    {
    "nombre": "Ana",
    "edad": 25,
    "ciudad": "Valencia"
    }

🔹 **Objeto con array**(lista de valores): Incluye un campo que contiene una lista.

    {
    "nombre": "Pedro",
    "aficiones": ["cine", "fútbol", "viajar"]
    }

🔹 **Objeto con otro objeto anidado**: Un campo puede contener a su vez otro objeto JSON.

    {
    "nombre": "Lucía",
    "edad": 30,
    "direccion": {
        "calle": "Gran Vía",
        "ciudad": "Madrid",
        "codigo_postal": 28013
    }
    }

🔹 **Array de objetos**: Cuando necesitamos almacenar varios elementos similares (por ejemplo, una lista de productos o alumnos).

    {
    "alumnos": [
        { "nombre": "Carlos", "nota": 8.5 },
        { "nombre": "María", "nota": 9.2 },
        { "nombre": "Laura", "nota": 7.8 }
    ]
    }

🔹 **Combinación compleja (objetos + arrays + anidamientos)**: Para representar datos estructurados, como los de una tienda online.

    {
    "pedido": {
        "id": 101,
        "fecha": "2025-10-11",
        "cliente": {
        "nombre": "Javier López",
        "email": "jlopez@example.com"
        },
        "productos": [
        { "nombre": "Ratón inalámbrico", "precio": 19.99, "cantidad": 1 },
        { "nombre": "Teclado mecánico", "precio": 49.95, "cantidad": 1 }
        ],
        "total": 69.94
    }
    }


🔹 **Array de objetos principales**: También se puede usar un array como estructura raíz, por ejemplo, para representar varios registros en un mismo fichero:

    [
        { "nombre": "Ana", "edad": 25 },
        { "nombre": "Pedro", "edad": 31 },
        { "nombre": "Lucía", "edad": 28 }
    ]
