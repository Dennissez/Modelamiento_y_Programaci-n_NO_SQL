# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## GUÍA PRÁCTICA 3: CONSULTAS Y FILTROS EN MONGODB - CASO DATASET TITANIC (R_3 / EJ_3)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 2:** Consumiendo Información de una Base de Datos NoSQL o Documental (Semana 3 / Actividad N°1)
* **Objetivo:** Aplicar sentencias de consulta y operadores de filtrado relacional (`$gt`, `$eq`) y lógico (`$and`, `$or`) en MongoDB, utilizando el dataset `titanic.csv` cargado en la base de datos `titanic` y colección `pasajeros`.

---

## 🧭 Tabla de Contenidos

1. [Configuración Previa del Entorno](#1-configuración-previa-del-entorno)
2. [Estructura del Dataset (`titanic.pasajeros`)](#2-estructura-del-dataset-titanicpasajeros)
3. [Consulta A: Pasajeros que Sobrevivieron](#3-consulta-a-pasajeros-que-sobrevivieron)
4. [Consulta B: Pasajeros con Tarifa de Boleto Superior a 50](#4-consulta-b-pasajeros-con-tarifa-de-boleto-superior-a-50)
5. [Consulta C: Mujeres Sobrevivientes de Primera Clase](#5-consulta-c-mujeres-sobrevivientes-de-primera-clase)
6. [Consulta D: Pasajeros con Edad de 22 o 31 Años](#6-consulta-d-pasajeros-con-edad-de-22-o-31-años)
7. [Consulta E: Mujeres de Tercera Clase que Fallecieron](#7-consulta-e-mujeres-de-tercera-clase-que-fallecieron)
8. [Matriz Comparativa de Sintaxis y Operadores](#8-matriz-comparativa-de-sintaxis-y-operadores)

---

## 1. Configuración Previa del Entorno

Antes de ejecutar las consultas, el dataset `titanic.csv` debe encontrarse importado en MongoDB:

1. **Base de Datos:** `titanic`
2. **Colección:** `pasajeros`
3. **Mapeo de Tipos Recomendado:**
   * `Survived`: `Int32` (1 = Sobrevivió, 0 = No sobrevivió)
   * `Pclass`: `Int32` (1 = Primera Clase, 2 = Segunda Clase, 3 = Tercera Clase)
   * `Name`: `String`
   * `Sex`: `String` (`"male"` / `"female"`)
   * `Age`: `Int32` o `Double`
   * `Fare`: `Double` o `Int32` (Tarifa pagada)

En la consola `mongosh`, se debe conmutar previamente a la base de datos:
```javascript
use titanic
```

---

## 2. Estructura del Dataset (`titanic.pasajeros`)

Ejemplo de documento tipo en formato BSON:

```json
{
  "_id": ObjectId("6512d89a6bd4861887e1a001"),
  "Survived": 1,
  "Pclass": 1,
  "Name": "Mrs. John Bradley (Florence Briggs Thayer) Cumings",
  "Sex": "female",
  "Age": 38,
  "Fare": 71.2833
}
```

---

## 3. Consulta A: Pasajeros que Sobrevivieron

### Requerimiento
Consultar todos los pasajeros que sobrevivieron al hundimiento del Titanic (`Survived` igual a `1`).

### Filtro en MongoDB Compass
```json
{ Survived: 1 }
```

### Comando en `mongosh`
```javascript
db.pasajeros.find({ Survived: 1 })
```

### Comprobación de Cantidad
```javascript
db.pasajeros.countDocuments({ Survived: 1 })
// Resultado oficial: 342 documentos
```

---

## 4. Consulta B: Pasajeros con Tarifa de Boleto Superior a 50

### Requerimiento
Consultar los pasajeros cuyo precio de ticket (`Fare`) haya sido estrictamente mayor a 50.

### Filtro en MongoDB Compass
```json
{ Fare: { $gt: 50 } }
```

### Comando en `mongosh`
```javascript
db.pasajeros.find({ Fare: { $gt: 50 } })
```

### Comprobación de Cantidad
```javascript
db.pasajeros.countDocuments({ Fare: { $gt: 50 } })
// Resultado oficial: 160 documentos
```

---

## 5. Consulta C: Mujeres Sobrevivientes de Primera Clase

### Requerimiento
Listar los pasajeros de Primera Clase (`Pclass: 1`) que sobrevivieron (`Survived: 1`) y cuyo sexo sea femenino (`Sex: 'female'`).

### Filtro en MongoDB Compass
```json
{ $and: [ { Survived: 1 }, { Pclass: 1 }, { Sex: "female" } ] }
```

*Nota: También es válida la sintaxis implícita:*
```json
{ Survived: 1, Pclass: 1, Sex: "female" }
```

### Comando en `mongosh`
```javascript
db.pasajeros.find({ $and: [ { Survived: 1 }, { Pclass: 1 }, { Sex: "female" } ] })
```

### Comprobación de Cantidad
```javascript
db.pasajeros.countDocuments({ $and: [ { Survived: 1 }, { Pclass: 1 }, { Sex: "female" } ] })
// Resultado oficial: 91 documentos
```

---

## 6. Consulta D: Pasajeros con Edad de 22 o 31 Años

### Requerimiento
Consultar los pasajeros del Titanic que tengan exactamente 22 o 31 años de edad.

### Filtro en MongoDB Compass
```json
{ $or: [ { Age: 22 }, { Age: 31 } ] }
```

*Nota: Alternativa con operador `$in`:*
```json
{ Age: { $in: [22, 31] } }
```

### Comando en `mongosh`
```javascript
db.pasajeros.find({ $or: [ { Age: 22 }, { Age: 31 } ] })
```

### Comprobación de Cantidad
```javascript
db.pasajeros.countDocuments({ $or: [ { Age: 22 }, { Age: 31 } ] })
// Resultado oficial: 58 documentos
```

---

## 7. Consulta E: Mujeres de Tercera Clase que Fallecieron

### Requerimiento
Listar todos los pasajeros que murieron (`Survived: 0`), de sexo femenino (`Sex: 'female'`) y que viajaban en Tercera Clase (`Pclass: 3`).

### Filtro en MongoDB Compass
```json
{ $and: [ { Survived: 0 }, { Sex: "female" }, { Pclass: 3 } ] }
```

*Nota: Sintaxis simplificada:*
```json
{ Survived: 0, Sex: "female", Pclass: 3 }
```

### Comando en `mongosh`
```javascript
db.pasajeros.find({ $and: [ { Survived: 0 }, { Sex: "female" }, { Pclass: 3 } ] })
```

### Comprobación de Cantidad
```javascript
db.pasajeros.countDocuments({ $and: [ { Survived: 0 }, { Sex: "female" }, { Pclass: 3 } ] })
// Resultado oficial: 72 documentos
```

---

## 8. Matriz Comparativa de Sintaxis y Operadores

| Ítem | Criterio de Búsqueda | Operadores Usados | Total Documentos | Sintaxis Compass |
| :--- | :--- | :--- | :---: | :--- |
| **A** | Sobrevivientes | Igualdad implícita | **342** | `{ Survived: 1 }` |
| **B** | Tarifa $> 50$ | Relacional `$gt` | **160** | `{ Fare: { $gt: 50 } }` |
| **C** | Sobrevivientes + Clase 1 + Mujeres | Lógico `$and` | **91** | `{ $and: [{ Survived: 1 }, { Pclass: 1 }, { Sex: "female" }] }` |
| **D** | Edad 22 o 31 años | Lógico `$or` | **58** | `{ $or: [{ Age: 22 }, { Age: 31 }] }` |
| **E** | No sobrevivientes + Mujeres + Clase 3 | Lógico `$and` | **72** | `{ $and: [{ Survived: 0 }, { Sex: "female" }, { Pclass: 3 }] }` |
