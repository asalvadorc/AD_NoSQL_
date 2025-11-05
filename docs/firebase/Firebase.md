# Introducción

Firebase es un servicio de backend, es decir, del lado del servidor, que ofrece la posibilidad de guardar los datos de aplicaciones web y de dispositivos móviles, con la particularidad de que los datos modificados por uno de los clientes conectados se pueden actualizar en tiempo real en todos los demás que estén conectados.

Ofrece una serie de servicios, entre los cuales podemos destacar:

- **Firebase Auth** – Autenticación de usuarios: permite gestionar la validación de usuarios de forma cómoda. Incluye autenticación mediante Facebook, GitHub, Twitter y Google, así como validación utilizando una cuenta de correo, guardando la contraseña en Firebase, lo que nos libera de tener que almacenarlas en nuestro propio dispositivo.

- **Realtime Database** – Base de Datos en Tiempo Real: permite guardar datos en la nube que estarán sincronizados entre todas las aplicaciones (clientes) conectadas. En realidad, no es una base de datos propiamente dicha, sino un documento JSON.

- **Cloud Firestore**: es una evolución de la anterior que sí permite guardar colecciones de documentos JSON, por lo que ya puede considerarse una base de datos completa.

- **Firebase Storage**: permite almacenar archivos para nuestras aplicaciones, como pueden ser audios, vídeos, imágenes, etc.

En este tema nos centraremos sobre todo en el segundo y el tercer servicio, los de bases de datos. Debemos recordar que **Realtime Database** no es una base de datos completa: únicamente podemos guardar un documento JSON, aunque puede ser tan extenso como queramos. Se trata, por tanto, de una base de datos NoSQL, o más bien una mini base de datos NoSQL. Eso sí, permite sincronizar los datos en tiempo real en todos los dispositivos conectados. En cambio, **Cloud Firestore** sí es una base de datos completa.