# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## MANUAL DE ESTUDIO 4: CONSUMIENDO INFORMACIÓN DE UNA BASE DE DATOS NOSQL: OPERACIONES DE ACCIÓN (ME_4)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 2:** Consumiendo Información de una Base de Datos NoSQL o Documental (Semana 4)
* **Objetivo:** Implementar sentencias de manipulación y mutación de datos en MongoDB que permitan agregar (`insertOne`, `insertMany`), eliminar (`deleteOne`, `deleteMany`) y actualizar (`updateOne`, `updateMany`, `update`, `$set`, `$inc`) documentos según los requerimientos operativos de la organización.

---

## 🧭 Tabla de Contenidos

1. [Evolución de las Operaciones CRUD en MongoDB](#1-evolución-de-las-operaciones-crud-en-mongodb)
2. [Operaciones de Inserción (INSERT)](#2-operaciones-de-inserción-insert)
   - [2.1 Inserción Unitaria: `insertOne()`](#21-inserción-unitaria-insertone)
   - [2.2 Flexibilidad de Esquema al Insertar](#22-flexibilidad-de-esquema-al-insertar)
   - [2.3 Inserción Gráfica en MongoDB Compass](#23-inserción-gráfica-en-mongodb-compass)
   - [2.4 Inserción Masiva: `insertMany()`](#24-inserción-masiva-insertmany)
3. [Operaciones de Eliminación (DELETE / REMOVE)](#3-operaciones-de-eliminación-delete--remove)
   - [3.1 Eliminación Unitaria: `deleteOne()`](#31-eliminación-unitaria-deleteone)
   - [3.2 Eliminación Masiva: `deleteMany()`](#32-eliminación-masiva-deletemany)
4. [Operaciones de Actualización (UPDATE)](#4-operaciones-de-actualización-update)
   - [4.1 Actualización Unitaria: `updateOne()` y Operador `$set`](#41-actualización-unitaria-updateone-y-operador-set)
   - [4.2 Actualización Masiva: `updateMany()`](#42-actualización-masiva-updatemany)
   - [4.3 Actualizaciones Complejas con `$inc` y `$set`](#43-actualizaciones-complejas-con-inc-y-set)
5. [Matriz Comparativa de Métodos y Respuestas del Motor](#5-matriz-comparativa-de-métodos-y-respuestas-del-motor)
6. [Preguntas de Reflexión y Autoevaluación](#6-preguntas-de-reflexión-y-autoevaluación)

---

## 1. Evolución de las Operaciones CRUD en MongoDB

Históricamente, MongoDB utilizaba los comandos genéricos `db.collection.insert()`, `db.collection.remove()` y `db.collection.update()`. A partir de MongoDB 3.2 (y consolidado en las versiones modernas 5.x y 7.x), se reemplazaron formalmente por métodos especializados:

```
    MÉTODOS OBSOLETOS                     MÉTODOS MODERNOS ESTÁNDAR
┌───────────────────────┐            ┌──────────────────────────────────┐
│ insert(...)           │    ───►    │ insertOne(...)  / insertMany(...)│
│ remove(...)           │    ───►    │ deleteOne(...)  / deleteMany(...)│
│ update(...)           │    ───►    │ updateOne(...)  / updateMany(...)│
└───────────────────────┘            └──────────────────────────────────┘
```

Esta separación semántica optimiza la predictibilidad del código, el rendimiento de escritura por lotes y previene modificaciones o eliminaciones masivas no intencionales.

---

## 2. Operaciones de Inserción (INSERT)

Contexto de trabajo en consola `mongosh`:
```javascript
use tienda
```

### 2.1 Inserción Unitaria: `insertOne()`

Permite registrar exactamente un documento dentro de una colección. Si el documento no incluye el atributo `_id`, MongoDB genera automáticamente un `ObjectId` de 12 bytes.

#### Sintaxis y Ejemplo
```javascript
db.productos.insertOne({
  "codigo": "PROD016",
  "nombre": "Cable de poder Monitor",
  "marca": "Tecnomati",
  "precio": 2350,
  "stock": 40
})
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "insertedId": ObjectId("6502a27482798861d9fd4d67")
}
```
* **`acknowledged: true`**: Confirma que el nodo de base de datos persistió la escritura en memoria/journal.
* **`insertedId`**: Retorna el identificador asignado a la nueva tupla documental.

### 2.2 Flexibilidad de Esquema al Insertar

Dado que MongoDB tiene esquema dinámico (*schemaless* a nivel de colección), un nuevo documento no requiere contener los mismos campos que los ya existentes:

```javascript
db.productos.insertOne({
  "codigo": "PROD026",
  "nombre": "Cable Coaxial 1 Mt",
  "precio": 1990,
  "stock": 10
})
```
*En este ejemplo, se omite el atributo `marca` sin que el motor lance un error de validación estructural.*

### 2.3 Inserción Gráfica en MongoDB Compass

1. Ingresar a la base de datos `tienda` y seleccionar la colección `productos`.
2. Presionar el botón verde **ADD DATA** $\rightarrow$ **Insert document**.
3. Seleccionar la vista JSON (`{}`) y pegar el contenido documental.
4. Presionar el botón **Insert**.

### 2.4 Inserción Masiva: `insertMany()`

Permite enviar un arreglo de documentos (`[ { ... }, { ... } ]`) en un único viaje de red (*round-trip*), optimizando el tiempo de carga masiva.

```javascript
db.productos.insertMany([
  {
    "codigo": "PROD020",
    "nombre": "Teclado KB-500 Number Pad",
    "marca": "Spektra",
    "precio": 7490,
    "stock": 5
  },
  {
    "codigo": "PROD021",
    "nombre": "Teclado KB-700W Multimedia",
    "marca": "Spektra",
    "precio": 7590,
    "stock": 10
  }
])
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "insertedIds": [
    ObjectId("6502ab9382798861d9fd4d68"),
    ObjectId("6502ab9382798861d9fd4d69")
  ]
}
```

---

## 3. Operaciones de Eliminación (DELETE / REMOVE)

### 3.1 Eliminación Unitaria: `deleteOne()`

Elimina el **primer documento** que cumpla con el criterio de filtro provisto, incluso si existen más registros coincidentes en la colección.

```javascript
db.productos.deleteOne({ codigo: "PROD016" })
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "deletedCount": 1
}
```
* **`deletedCount`**: Informa el total exacto de documentos suprimidos (1 si existía, 0 si ningún registro coincidió).

### 3.2 Eliminación Masiva: `deleteMany()`

Elimina **todos** los documentos que satisfagan la condición de búsqueda.

```javascript
db.productos.deleteMany({ marca: "Tecnomati" })
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "deletedCount": 2
}
```

> [!CAUTION]
> Ejecutar `db.productos.deleteMany({})` sin especificar ningún filtro eliminará la totalidad de los documentos de la colección de manera irreversible.

---

## 4. Operaciones de Actualización (UPDATE)

A diferencia del comando SQL `UPDATE tabla SET ...`, en MongoDB las modificaciones sobre documentos se coordinan mediante **operadores de actualización atómicos**. Si se omite el operador (como `$set`), versiones antiguas de `update()` sobreescribían el documento completo.

### 4.1 Actualización Unitaria: `updateOne()` y Operador `$set`

Modifica los atributos del primer documento coincidente con el filtro.

* **Operador `$set`**: Reemplaza el valor de un campo existente o crea el campo si no figuraba previamente en el documento.

```javascript
db.productos.updateOne(
  { nombre: "Cable de poder Monitor", marca: "Tecnomati" },
  { $set: { precio: 2150 } }
)
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "matchedCount": 1,
  "modifiedCount": 1
}
```
* **`matchedCount`**: Cantidad de documentos que cumplieron con la condición de búsqueda.
* **`modifiedCount`**: Cantidad real de documentos cuyo valor cambió (si el precio ya era 2150, `matchedCount` será 1 pero `modifiedCount` será 0).

### 4.2 Actualización Masiva: `updateMany()`

Aplica la modificación sobre todos los registros que hagan match con el criterio.

```javascript
db.productos.updateMany(
  { marca: "Spektra" },
  { $set: { precio: 6990 } }
)
```

#### Respuesta del Motor
```json
{
  "acknowledged": true,
  "matchedCount": 2,
  "modifiedCount": 2
}
```

### 4.3 Actualizaciones Complejas con `$inc` y `$set`

Los operadores de mutación pueden combinarse dentro de la misma instrucción para realizar cambios cuantitativos y textuales simultáneamente:

* **`$inc`**: Incrementa o disminuye el valor numérico de un campo en la magnitud indicada (ej. `{ $inc: { stock: 5 } }` suma 5; `{ $inc: { stock: -2 } }` resta 2).

```javascript
db.productos.update(
  { codigo: "PROD020" },
  {
    $inc: { stock: 5 },
    $set: { nombre: "Teclado KB-500 Pad Numérico" }
  }
)
```

#### Estado Antes de la Modificación
```json
{
  "_id": ObjectId("6502b95f82798861d9fd4d6b"),
  "codigo": "PROD020",
  "nombre": "Teclado KB-500 Number Pad",
  "marca": "Spektra",
  "precio": 6990,
  "stock": 5
}
```

#### Estado Después de la Modificación
```json
{
  "_id": ObjectId("6502b95f82798861d9fd4d6b"),
  "codigo": "PROD020",
  "nombre": "Teclado KB-500 Pad Numérico",
  "marca": "Spektra",
  "precio": 6990,
  "stock": 10
}
```

---

## 5. Matriz Comparativa de Métodos y Respuestas del Motor

| Operación | Método Estándar | Argumentos Clave | Métricas de Retorno |
| :--- | :--- | :--- | :--- |
| **Insertar 1** | `insertOne(doc)` | Documento JSON individual | `insertedId` |
| **Insertar N** | `insertMany([docs])` | Arreglo de documentos JSON | `insertedIds` (array) |
| **Eliminar 1** | `deleteOne(filtro)` | Criterio de búsqueda | `deletedCount` (0 o 1) |
| **Eliminar N** | `deleteMany(filtro)` | Criterio de búsqueda | `deletedCount` ($\ge 0$) |
| **Actualizar 1**| `updateOne(filtro, update)` | Filtro + Operadores (`$set`, `$inc`) | `matchedCount`, `modifiedCount` |
| **Actualizar N**| `updateMany(filtro, update)`| Filtro + Operadores (`$set`, `$inc`) | `matchedCount`, `modifiedCount` |

---

## 6. Preguntas de Reflexión y Autoevaluación

1. **¿Por qué es una mala práctica utilizar `db.collection.update()` sin el operador `$set`?**
   * Porque en versiones tempranas y comportamientos por compatibilidad, pasar un documento literal sin operadores provoca el reemplazo absoluto del documento existente, perdiendo todos los campos que no se hayan vuelto a escribir.
2. **¿Qué ventaja técnica ofrece `insertMany()` frente a ejecutar múltiples llamadas a `insertOne()` dentro de un bucle?**
   * `insertMany()` empaqueta todos los registros en una única transacción de red hacia el servidor (escritura por lotes o *bulk write*), reduciendo drásticamente la latencia y la sobrecarga de conexiones I/O.
3. **¿Cuál es la diferencia entre `matchedCount` y `modifiedCount` al actualizar datos?**
   * `matchedCount` refleja los documentos que cumplieron con la condición del filtro, mientras que `modifiedCount` contabiliza únicamente aquellos donde el nuevo valor provocó una alteración real respecto a los datos almacenados.
