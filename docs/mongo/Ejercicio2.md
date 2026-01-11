# 📝 Ejercicio: Mi base de datos MongoDB con **referencias** y **agregaciones** (tema libre)

## 🛠️ Requisitos

### 1. Base de Datos

En este ejercicio vas a crear tu propia base de datos MongoDB, cuyo nombre debe seguir el formato: **bd_tunombreprimerapellido**.

Por ejemplo, si te llamas Maria Pérez, tu base de datos se llamará bd_mariaperez.

---

### 2. Modelo con varias colecciones (tema libre)

Debes crear **al menos 3 colecciones** relacionadas mediante **referencias**. La temática es **libre**.

#### Requisitos del modelo:
- Debe haber **relaciones 1:N** y/o **N:M** usando **referencias** (no embedding masivo).  
- Puedes usar **IDs lógicos** (ej.: `"prod-laptop"`) o **ObjectId**.  
- Cada colección debe cargarse desde su **archivo JSON**.

---

### 3. Operaciones en Kotlin

Trabajarás con **Kotlin + KMongo** (y Jackson para leer JSON) para implementar:

- Insertar varios documentos en una colección.
- Mostrar todos los documentos almacenados.
- Actualizar algún campo de un documento.
- Buscar documentos según un criterio.
- Eliminar uno o varios documentos según condición.
- **Cargar datos desde JSON** para todas tus colecciones.
- Verificar datos en `mongosh`.

---

### 4. Búsquedas avanzadas con `aggregate()`

Debes crear **dos consultas de agregación**:

---

#### 🔹 A) Join multinivel con referencias

Usa **`$lookup`** y **`$unwind`** para combinar colecciones.


---

#### 🔹 B) Agregación en una sola colección

Ejemplos:
- Estadísticas por categoría  
- Documentos por año  
- Media de duración/precio/puntuación por grupo  

Debe contener al menos un `\$group` y un `\$sort`.

---

## 📩 Entrega

Sube un archivo **.zip** con:

- El proyecto completo de **IntelliJ**  
- Archivos JSON  
- Archivo **README.md** donde expliques:  

    - Nombre de tu base de datos  
    - Temática elegida  
    - Colecciones y sus referencias  
    - Qué operaciones CRUD has implementado  
    - Qué hace cada pipeline de agregación.
    - Ejemplos de resultados en consola  
    - Capturas de verificación desde `mongosh`

---

## ✅ Rúbrica de evaluación

| Criterio | Puntos |
|---------|-------:|
| **Base de datos y modelo (3 colecciones con referencias)** | 2 |
| **Carga desde JSON** | 1 |
| **Operaciones CRUD en Kotlin** | 2 |
| **Agregaciones con referencias (`lookup + unwind`)** | 3 |
| **Agregación en una sola colección** | 1 |
| **Código y documentación (README, claridad, salidas)** | 1 |
| **Total** | **10 puntos** |

---

