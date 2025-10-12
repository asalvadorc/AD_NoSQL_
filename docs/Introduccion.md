# 🔹Introducción

Las bases de datos documentales nativas (como MongoDB, Redis o Firebase) almacenan información en forma de documentos, usualmente codificados en JSON, BSON o XML, en lugar de filas y columnas como en las bases de datos relacionales.

Cada documento puede tener una estructura diferente, lo que permite mayor flexibilidad y agilidad en el desarrollo.

Sin embargo, si el dominio de la aplicación tiene muchas relaciones fuertes entre entidades y se necesita garantizar una integridad referencial estricta, una base de datos relacional puede ser más adecuada.

**Ventajas**{.azul}

| Ventaja                        | Descripción                                                                 |
|-------------------------------|-----------------------------------------------------------------------------|
| Flexibilidad del esquema      | No es necesario definir un esquema fijo antes de insertar datos. Ideal para estructuras dinámicas. |
| Escalabilidad horizontal      | Se adaptan bien al escalado distribuyendo los datos en múltiples servidores (sharding). |
| Rendimiento en lectura y escritura | Muy eficiente en operaciones de lectura y escritura sobre documentos completos. |
| Modelo cercano a objetos      | Almacenan los datos de manera similar a como se manejan en el código (objetos serializados como JSON). |
| Facilidad de integración con APIs REST | Los documentos JSON pueden ser enviados y recibidos fácilmente a través de APIs. |
| Ideal para datos semiestructurados | Útiles para trabajar con datos que no se ajustan a una estructura tabular, como respuestas de formularios, logs, etc. |


 **Inconvenientes**{.azul}

 | Inconveniente                         | Descripción                                                                 |
|--------------------------------------|-----------------------------------------------------------------------------|
| Falta de integridad referencial      | No hay claves foráneas como en las bases de datos relacionales, lo que puede causar inconsistencias si no se gestiona adecuadamente desde la aplicación. |
| Redundancia de datos                 | Se repite información entre documentos al no haber normalización; esto puede generar más uso de espacio. |
| Curva de aprendizaje                 | Requiere aprender nuevos conceptos como agregaciones, operadores específicos y estructuras de documentos. |
| Menor soporte para transacciones complejas | Aunque existen transacciones en algunas bases (como MongoDB), su uso es más limitado que en sistemas relacionales. |
| Consultas menos optimizadas en relaciones complejas | No es la mejor opción cuando los datos necesitan muchas relaciones y joins complejos. |
