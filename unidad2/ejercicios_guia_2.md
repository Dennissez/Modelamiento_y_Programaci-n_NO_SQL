# Ejercicio N° 2

---

## 🧭 Índice de Contenidos

* [1. Contexto y Aprendizajes Esperados](#1-contexto-y-aprendizajes-esperados)
* [2. Respuestas Esperadas](#2-respuestas-esperadas)
  * [Ejercicio 1 - Enunciado #1](#ejercicio-1---enunciado-1)
    * [A) Inserción de un nuevo producto LG con insertOne()](#a-utilizando-insertone-inserte-un-nuevo-producto-de-la-marca-lg)
    * [B) Inserción de 3 productos con insertMany() (Logitech, HP, Samsung)](#b-utilizando-insertmany-inserte-3-nuevos-productos-de-las-marcas-logitech-hp-y-samsung)
    * [C) Modificación de precios (+30%) con updateMany()](#c-modifique-el-precio-de-los-3-productos-ingresados-en-la-letra-b-de-esta-actividad-aumentando-su-precio-en-un-30)
    * [D) Eliminación de productos con precio menor a 100.000 con deleteMany()](#d-borre-todos-los-productos-cuyo-precio-sea-menor-a-100000-)
    * [E) Conteo final de registros en la colección productos](#e-cuántos-registros-quedan-en-la-collection)
* [3. Cuadro Resumen de Trazabilidad y Balance Documental](#3-cuadro-resumen-de-trazabilidad-y-balance-documental)

---

## 1. Contexto y Aprendizajes Esperados

* Reconoce las principales sentencias de acción y de consulta sobre una base de datos NoSQL de acuerdo con los requerimientos de información de una determinada organización.
* Implementa sentencias que permitan agregar, eliminar y actualizar datos en un modelo NoSQL de acuerdo con los requerimientos planteados.

---

## 2. Respuestas Esperadas

### Ejercicio 1 - Enunciado #1

Dado el conjunto de datos `productos.csv` y su importación limpia dentro de una base de datos llamada `tienda` y la collection `productos`, realice las acciones sobre dicha collection y los documentos:

```javascript
use tienda
```

---

#### A) Utilizando insertOne() inserte un nuevo producto de la marca LG.

**Sintaxis MongoDB Shell (`mongosh`):**

```javascript
db.productos.insertOne({
  "codigo": "PROD016",
  "nombre": "Monitor UltraGear 27'' IPS 144Hz",
  "marca": "LG",
  "precio": 189990,
  "stock": 25
})
```

**Respuesta de la consola:**

```json
{
  "acknowledged": true,
  "insertedId": ObjectId("6502a27482798861d9fd4d67")
}
```

> **Explicación:** Se añade un único documento con la marca `"LG"`. El total de registros en la colección pasa de 15 a 16.

---

#### B) Utilizando insertMany() inserte 3 nuevos productos de las marcas: Logitech, HP y Samsung.

**Sintaxis MongoDB Shell (`mongosh`):**

```javascript
db.productos.insertMany([
  {
    "codigo": "PROD017",
    "nombre": "Mouse Gamer Inalámbrico G Pro X",
    "marca": "Logitech",
    "precio": 89990,
    "stock": 40
  },
  {
    "codigo": "PROD018",
    "nombre": "Teclado Mecánico OMEN Sequencer",
    "marca": "HP",
    "precio": 79990,
    "stock": 15
  },
  {
    "codigo": "PROD019",
    "nombre": "Tablet Galaxy Tab S9 FE 128GB",
    "marca": "Samsung",
    "precio": 399990,
    "stock": 20
  }
])
```

**Respuesta de la consola:**

```json
{
  "acknowledged": true,
  "insertedIds": [
    ObjectId("6502ab9382798861d9fd4d68"),
    ObjectId("6502ab9382798861d9fd4d69"),
    ObjectId("6502ab9382798861d9fd4d6a")
  ]
}
```

> **Explicación:** Se ingresan tres documentos en un arreglo (`[...]`). El total de registros en la colección pasa de 16 a 19.

---

#### C) Modifique el precio de los 3 productos ingresados en la letra B) de esta actividad, aumentando su precio en un 30%.

**Sintaxis MongoDB Shell (`mongosh`):**

```javascript
db.productos.updateMany(
  { codigo: { $in: ["PROD017", "PROD018", "PROD019"] } },
  { $mul: { precio: 1.30 } }
)
```

*Alternativa con `$set` fijando los valores calculados ($Precio \times 1.30$):*

```javascript
db.productos.updateOne({ codigo: "PROD017" }, { $set: { precio: 116987 } })
db.productos.updateOne({ codigo: "PROD018" }, { $set: { precio: 103987 } })
db.productos.updateOne({ codigo: "PROD019" }, { $set: { precio: 519987 } })
```

**Respuesta de la consola:**

```json
{
  "acknowledged": true,
  "matchedCount": 3,
  "modifiedCount": 3
}
```

> **Explicación:** El operador `$mul` multiplica los precios por $1.30$, actualizando los 3 productos ingresados en la letra B sin alterar el total de documentos.

---

#### D) Borre todos los productos cuyo precio sea menor a 100.000.-

**Sintaxis MongoDB Shell (`mongosh`):**

```javascript
db.productos.deleteMany({ precio: { $lt: 100000 } })
```

**Respuesta de la consola:**

```json
{
  "acknowledged": true,
  "deletedCount": 5
}
```

> **Explicación:** El operador relacional `$lt: 100000` selecciona y elimina los 5 productos del archivo inicial que tenían precios inferiores a 100.000 (`PROD003`, `PROD006`, `PROD009`, `PROD012` y `PROD015`). Los productos de la letra B quedan protegidos al haberse incrementado en la letra C a \$116.987 y \$103.987.

---

#### E) ¿Cuántos registros quedan en la collection?

**Sintaxis MongoDB Shell (`mongosh`):**

```javascript
db.productos.countDocuments()
```

**Respuesta de la consola:**

```javascript
14
```

> **Explicación:** Quedan exactamente **14 registros** en la colección `tienda.productos`.

---

## 3. Cuadro Resumen de Trazabilidad y Balance Documental

| Paso / Literal | Operación | Método Utilizado | Impacto en Documentos | Total en Colección |
| :--- | :--- | :--- | :---: | :---: |
| **Inicio** | Carga inicial `productos.csv` | `mongoimport` / Compass | $+15$ | **15** |
| **A** | Insertar producto LG | `insertOne()` | $+1$ | **16** |
| **B** | Insertar Logitech, HP y Samsung | `insertMany()` | $+3$ | **19** |
| **C** | Aumentar precio en un 30% | `updateMany()` (`$mul` / `$set`) | $0$ | **19** |
| **D** | Eliminar productos con precio $< 100.000$ | `deleteMany()` (`$lt`) | $-5$ | **14** |
| **E** | Conteo total final | `countDocuments()` | — | **14** |
