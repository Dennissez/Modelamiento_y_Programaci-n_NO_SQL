# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## MANUAL DE ESTUDIO 3: CONSUMIENDO INFORMACIÓN DE UNA BASE DE DATOS NOSQL O DOCUMENTAL (ME_3)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 2:** Consumiendo Información de una Base de Datos NoSQL o Documental (Semana 3)
* **Objetivo:** Reconocer y aplicar las principales sentencias de consulta y filtros condicionales en MongoDB mediante el comando `find()`, dominando los operadores relacionales (`$eq`, `$lt`, `$lte`, `$gt`, `$gte`, `$ne`, `$in`, `$nin`) y los operadores lógicos (`$and`, `$or`, `$not`) tanto en interfaz gráfica (MongoDB Compass) como en consola de comandos (`mongosh`).

---

## 🧭 Tabla de Contenidos

1. [Consultas NoSQL para Resolver Problemas Complejos](#1-consultas-nosql-para-resolver-problemas-complejos)
2. [Entornos de Ejecución: MongoDB Compass vs. mongosh](#2-entornos-de-ejecución-mongodb-compass-vs-mongosh)
3. [El Comando Fundamental: `find()`](#3-el-comando-fundamental-find)
4. [Dataset de Referencia (`tienda.productos`)](#4-dataset-de-referencia-tiendaproductos)
5. [Operadores Relacionales de Comparación](#5-operadores-relacionales-de-comparación)
   * [5.1 Operador de Igualdad: `$eq`](#51-operador-de-igualdad-eq)
   * [5.2 Operador Menor Que: `$lt`](#52-operador-menor-que-lt)
   * [5.3 Operador Menor o Igual Que: `$lte`](#53-operador-menor-o-igual-que-lte)
   * [5.4 Operador Mayor Que: `$gt`](#54-operador-mayor-que-gt)
   * [5.5 Operador Mayor o Igual Que: `$gte`](#55-operador-mayor-o-igual-que-gte)
   * [5.6 Operador Distinto: `$ne`](#56-operador-distinto-ne)
   * [5.7 Operador Pertenencia: `$in`](#57-operador-pertenencia-in)
   * [5.8 Operador No Pertenencia: `$nin`](#58-operador-no-pertenencia-nin)
6. [Operadores Lógicos de Combinación](#6-operadores-lógicos-de-combinación)
   * [6.1 Operador Conjunción: `$and`](#61-operador-conjunción-and)
   * [6.2 Operador Disyunción: `$or`](#62-operador-disyunción-or)
   * [6.3 Operador Negación: `$not`](#63-operador-negación-not)
7. [Tabla Resumen de Sintaxis y Operadores](#7-tabla-resumen-de-sintaxis-y-operadores)
8. [Preguntas de Reflexión y Autoevaluación](#8-preguntas-de-reflexión-y-autoevaluación)

---

## 1. Consultas NoSQL para Resolver Problemas Complejos

El paradigma NoSQL documental de MongoDB se apoya en el formato binario **BSON** (*Binary JSON*). A diferencia del lenguaje estructurado SQL que requiere declaraciones complejas con múltiples cláusulas y bloqueos transaccionales en tablas normalizadas, MongoDB utiliza el **MongoDB Query API**, un lenguaje de consultas expresado directamente mediante objetos y pares clave-valor.

### Ventajas en Entornos de Alta Demanda:
* **Esquemas Dinámicos:** Permite filtrar documentos heterogéneos sin que la ausencia de un campo en algunos registros provoque errores de ejecución.
* **Escalabilidad Distribuida:** Los filtros se resuelven de forma eficiente a lo largo de nodos particionados (*shards*).
* **Indexación Secundaria:** Soporte para índices directos sobre campos anidados y arreglos.

---

## 2. Entornos de Ejecución: MongoDB Compass vs. mongosh

Para consumir y analizar datos se disponen de dos herramientas complementarias:

### a) MongoDB Compass (Interfaz Gráfica)
* Permite explorar colecciones mediante una barra de filtrado visual denominada **Filter**.
* Los filtros se ingresan directamente como objetos JSON: `{ campo: { $operador: valor } }`.
* Permite alternar vistas de resultados en formato lista documental, vista JSON cruda o vista tabular/cuadrícula.

### b) MongoDB Shell (`mongosh`)
* Interfaz de consola de comandos en modo texto.
* Requiere seleccionar primero el contexto de la base de datos de trabajo mediante:
  ```javascript
  use tienda
  ```
* El identificador contextual `db` representa la base de datos activa sobre la cual se invoca la colección y el método:
  ```javascript
  db.productos.find(...)
  ```
* Para validar en cualquier momento sobre qué base de datos se está operando, se ejecuta:
  ```javascript
  db
  ```

---

## 3. El Comando Fundamental: `find()`

El método `find()` recupera documentos desde una colección que cumplan con los criterios de búsqueda especificados en un objeto de filtro.

### Sintaxis General en Consola
```javascript
db.coleccion.find(filtro, proyeccion)
```

* **`filtro` (Opcional):** Objeto JSON con los criterios y operadores. Si se deja vacío (`{}`) o sin argumentos, retorna todos los documentos de la colección.
* **Retorno:** Devuelve un **cursor** que apunta al conjunto de documentos resultantes.
* En `mongosh`, la consulta base para traer toda la información de productos es:
  ```javascript
  db.productos.find()
  ```

---

## 4. Dataset de Referencia (`tienda.productos`)

Para todos los ejemplos prácticos de este manual, se utiliza la colección `productos` de la base de datos `tienda`, compuesta por 15 documentos:

| Código | Nombre | Marca | Precio | Stock |
| :--- | :--- | :--- | :--- | :--- |
| `PROD001` | Apple Watch SE | Apple | 219990 | 100 |
| `PROD002` | Bose Portable Home Speaker | Bose | 379990 | 98 |
| `PROD003` | Disco Externo 1TB 2.5" | WD | 46990 | 90 |
| `PROD004` | Multifuncional Tinta Continua | HP | 139990 | 50 |
| `PROD005` | AirPods (3° generación) | Apple | 169990 | 65 |
| `PROD006` | Unidad SSD 480GB Sata3 2.5" | WD | 24990 | 56 |
| `PROD007` | Notebook Pavilion 15.6" FHD Ryzen 7 | HP | 679990 | 100 |
| `PROD008` | MacBook Pro 16" | Apple | 2749990 | 100 |
| `PROD009` | Unidad SSD 480GB Sata3 2.5" | Kingston | 28990 | 90 |
| `PROD010` | Monitor Gamer 32" Curvo Odyssey | Samsung | 569990 | 30 |
| `PROD011` | Monitor Studio Display 27" | Apple | 1999990 | 60 |
| `PROD012` | RAM Notebook 16GB SoDimm DDR4 | Kingston | 41990 | 30 |
| `PROD013` | Notebook Galaxy Book3 15.6" | Samsung | 579990 | 70 |
| `PROD014` | Audífono Noise Cancelling 700 Negro | Bose | 360000 | 70 |
| `PROD015` | Cartucho de Tinta Original HP 122 | Cannon | 22990 | 100 |

---

## 5. Operadores Relacionales de Comparación

Los operadores de comparación equivalen a las condiciones relacionales de una cláusula `WHERE` en SQL.

### 5.1 Operador de Igualdad: `$eq` (*Equal*)
Filtra documentos donde el valor de un campo es idéntico al valor indicado.

* **Forma implícita:**
  * **Compass Filter:** `{ marca: "HP" }`
  * **mongosh:** `db.productos.find({ marca: "HP" })`
* **Forma explícita (`$eq`):**
  * **Compass Filter:** `{ marca: { $eq: "HP" } }`
  * **mongosh:** `db.productos.find({ marca: { $eq: "HP" } })`
* **Resultado:** 2 documentos correspondientes a `PROD004` y `PROD007`.

> [!NOTE]
> Los valores de tipo texto van entre comillas dobles (`"HP"`), mientras que los valores numéricos van sin comillas.

---

### 5.2 Operador Menor Que: `$lt` (*Less Than*)
Selecciona documentos cuyo valor numérico sea estrictamente inferior al parámetro indicado.

* **Requerimiento:** Productos con stock estrictamente menor a 50 unidades.
* **Compass Filter:** `{ stock: { $lt: 50 } }`
* **mongosh:**
  ```javascript
  db.productos.find({ stock: { $lt: 50 } })
  ```
* **Resultado:** 2 registros (`PROD010` con stock 30 y `PROD012` con stock 30).

---

### 5.3 Operador Menor o Igual Que: `$lte` (*Less Than or Equal*)
Selecciona documentos cuyo valor sea menor o igual al umbral especificado.

* **Requerimiento:** Productos con stock menor o igual a 56 unidades.
* **Compass Filter:** `{ stock: { $lte: 56 } }`
* **mongosh:**
  ```javascript
  db.productos.find({ stock: { $lte: 56 } })
  ```
* **Resultado:** 4 registros (`PROD004` con 50, `PROD006` con 56, `PROD010` con 30 y `PROD012` con 30).

---

### 5.4 Operador Mayor Que: `$gt` (*Greater Than*)
Selecciona documentos cuyo valor sea estrictamente superior al número indicado.

* **Requerimiento:** Productos con stock superior a 98 unidades.
* **Compass Filter:** `{ stock: { $gt: 98 } }`
* **mongosh:**
  ```javascript
  db.productos.find({ stock: { $gt: 98 } })
  ```
* **Resultado:** 4 registros con stock igual a 100 (`PROD001`, `PROD007`, `PROD008` y `PROD015`). El producto con stock 98 (`PROD002`) queda excluido.

---

### 5.5 Operador Mayor o Igual Que: `$gte` (*Greater Than or Equal*)
Incluye los valores iguales y superiores al umbral.

* **Requerimiento:** Productos con stock igual o superior a 98 unidades.
* **Compass Filter:** `{ stock: { $gte: 98 } }`
* **mongosh:**
  ```javascript
  db.productos.find({ stock: { $gte: 98 } })
  ```
* **Resultado:** 5 registros (`PROD001`, `PROD002`, `PROD007`, `PROD008` y `PROD015`).

---

### 5.6 Operador Distinto: `$ne` (*Not Equal*)
Recupera documentos donde el campo tiene un valor diferente al especificado o donde dicho campo no existe.

* **Requerimiento:** Productos cuya marca sea distinta de `"Apple"`.
* **Compass Filter:** `{ marca: { $ne: "Apple" } }`
* **mongosh:**
  ```javascript
  db.productos.find({ marca: { $ne: "Apple" } })
  ```
* **Resultado:** 11 documentos de marcas Bose, WD, HP, Kingston, Samsung y Cannon.

---

### 5.7 Operador Pertenencia: `$in` (*In*)
Verifica si el valor del campo coincide con cualquiera de los elementos listados dentro de un arreglo (`[...]`).

* **Requerimiento:** Productos cuya marca sea Samsung, WD o HP.
* **Compass Filter:** `{ marca: { $in: ["Samsung", "WD", "HP"] } }`
* **mongosh:**
  ```javascript
  db.productos.find({ marca: { $in: ["Samsung", "WD", "HP"] } })
  ```
* **Resultado:** 6 registros coincidentes.

---

### 5.8 Operador No Pertenencia: `$nin` (*Not In*)
Verifica que el valor del campo no coincida con ninguno de los elementos listados en el arreglo.

* **Requerimiento:** Productos cuya marca NO sea Samsung, WD ni HP.
* **Compass Filter:** `{ marca: { $nin: ["Samsung", "WD", "HP"] } }`
* **mongosh:**
  ```javascript
  db.productos.find({ marca: { $nin: ["Samsung", "WD", "HP"] } })
  ```
* **Resultado:** 9 registros correspondientes a las marcas restantes (Apple, Bose, Kingston, Cannon).

---

## 6. Operadores Lógicos de Combinación

Permiten conectar múltiples expresiones condicionales para formar filtros complejos.

### 6.1 Operador Conjunción: `$and`
Evalúa que **todas** las cláusulas del arreglo sean verdaderas simultáneamente.

* **Requerimiento:** Productos con stock mayor o igual a 100 **Y** cuya marca sea `"HP"`.
* **Sintaxis estándar:**
  * **Compass Filter:** `{ $and: [ { stock: { $gte: 100 } }, { marca: "HP" } ] }`
  * **mongosh:**
    ```javascript
    db.productos.find({ $and: [ { stock: { $gte: 100 } }, { marca: "HP" } ] })
    ```
* **Sintaxis implícita (abreviada):**
  MongoDB aplica automáticamente un `$and` cuando se separan múltiples condiciones por comas dentro de un mismo documento:
  ```javascript
  db.productos.find({ stock: { $gte: 100 }, marca: "HP" })
  ```
* **Resultado:** 1 registro (`PROD007` - Notebook Pavilion).

---

### 6.2 Operador Disyunción: `$or`
Evalúa que **al menos una** de las cláusulas del arreglo sea verdadera.

* **Requerimiento:** Productos cuya marca sea `"Samsung"` **O** `"HP"`.
* **Sintaxis:**
  * **Compass Filter:** `{ $or: [ { marca: "Samsung" }, { marca: "HP" } ] }`
  * **mongosh:**
    ```javascript
    db.productos.find({ $or: [ { marca: "Samsung" }, { marca: "HP" } ] })
    ```
* **Resultado:** 4 registros (`PROD004`, `PROD007`, `PROD010` y `PROD013`).

---

### 6.3 Operador Negación: `$not`
Invierte el valor de verdad del operador o expresión que envuelve.

* **Requerimiento:** Consultar productos cuyo precio sea mayor o igual a 1.000.000 negando la condición de ser menor a 1.000.000.
* **Sintaxis:**
  * **Compass Filter:** `{ precio: { $not: { $lt: 1000000 } } }`
  * **mongosh:**
    ```javascript
    db.productos.find({ precio: { $not: { $lt: 1000000 } } })
    ```
* **Resultado:** 2 registros (`PROD008` MacBook Pro a $2.749.990 y `PROD011` Monitor Studio Display a $1.999.990).

---

## 7. Tabla Resumen de Sintaxis y Operadores

| Tipo | Operador | Significado | Ejemplo de Filtro en MongoDB |
| :--- | :--- | :--- | :--- |
| **Búsqueda** | `find()` | Recuperar documentos de la colección | `db.productos.find({})` |
| **Relacional** | `$eq` | Igual a | `{ precio: { $eq: 46990 } }` |
| **Relacional** | `$lt` | Menor que ($<$) | `{ stock: { $lt: 50 } }` |
| **Relacional** | `$lte` | Menor o igual que ($\le$) | `{ stock: { $lte: 56 } }` |
| **Relacional** | `$gt` | Mayor que ($>$) | `{ stock: { $gt: 98 } }` |
| **Relacional** | `$gte` | Mayor o igual que ($\ge$) | `{ stock: { $gte: 98 } }` |
| **Relacional** | `$ne` | Distinto de ($\ne$) | `{ marca: { $ne: "Apple" } }` |
| **Relacional** | `$in` | Pertenencia a lista | `{ marca: { $in: ["WD", "HP"] } }` |
| **Relacional** | `$nin` | No pertenencia a lista | `{ marca: { $nin: ["Apple", "Bose"] } }` |
| **Lógico** | `$and` | Conjunción lógica (Y) | `{ $and: [ { stock: { $gte: 100 } }, { marca: "HP" } ] }` |
| **Lógico** | `$or` | Disyunción lógica (O) | `{ $or: [ { marca: "Samsung" }, { marca: "HP" } ] }` |
| **Lógico** | `$not` | Negación lógica (NO) | `{ precio: { $not: { $lt: 1000000 } } }` |

---

## 8. Preguntas de Reflexión y Autoevaluación

1. **¿Qué diferencia operativa existe entre ejecutar un filtro en la barra de MongoDB Compass y redactar la consulta en `mongosh`?**
   * En Compass únicamente se escribe el objeto JSON del criterio de filtro (lo que va dentro de los paréntesis de `find()`), mientras que en `mongosh` se debe especificar la ruta completa del método sobre el contexto de la base de datos (`db.coleccion.find(...)`).
2. **¿Por qué la consulta `{ campo1: val1, campo2: val2 }` equivale a un operador `$and`?**
   * Porque el analizador sintáctico de MongoDB concatena por defecto con lógica `AND` todas las claves separadas por coma en el nivel superior del documento de filtro.
3. **¿Cuándo es imprescindible el uso explícito del operador `$and`?**
   * Cuando se requiere aplicar múltiples condiciones sobre una **misma clave** utilizando operadores repetidos o cuando se combinan expresiones lógicas anidadas junto a operadores `$or`.
