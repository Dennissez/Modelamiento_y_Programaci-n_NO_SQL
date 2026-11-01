# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## MANUAL DE ESTUDIO 2: TÉCNICAS DE MODELAMIENTO, CLAVE-VALOR Y EMBEBER EN NOSQL (ME_2)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 1:** Diseño e Implementación de una Base de Datos NoSQL o Documental (Semana 2)
* **Objetivo:** Reconocer las técnicas de modelado de datos NoSQL para satisfacer requerimientos de la industria, identificando la paridad clave-valor y el patrón de datos embebidos en motores documentales como MongoDB.

---

## 🧭 Tabla de Contenidos

1. [Introducción al Modelado Documental en MongoDB](#1-introducción-al-modelado-documental-en-mongodb)
2. [Estructura BSON y Paridad Clave-Valor](#2-estructura-bson-y-paridad-clave-valor)
3. [Técnicas de Modelamiento en MongoDB](#3-técnicas-de-modelamiento-en-mongodb)
   * [3.1 Modelo de Datos Integrado (Desnormalizado / Embebido)](#31-modelo-de-datos-integrado-desnormalizado--embebido)
   * [3.2 Modelo de Datos Normalizado (Referenciado)](#32-modelo-de-datos-normalizado-referenciado)
4. [Buenas Prácticas de la Industria en el Diseño NoSQL](#4-buenas-prácticas-de-la-industria-en-el-diseño-nosql)
   * [4.1 Desnormalizar vs. Normalizar (Decisión Arquitectónica)](#41-desnormalizar-vs-normalizar-decisión-arquitectónica)
   * [4.2 Preparar Datos para el Futuro](#42-preparar-datos-para-el-futuro)
   * [4.3 Incrustación de Campos Dependientes (Instantáneas de Datos)](#43-incrustación-de-campos-dependientes-instantáneas-de-datos)
   * [4.4 Principio de Documentos Autosuficientes](#44-principio-de-documentos-autosuficientes)
5. [El Patrón Embebido (*Embedded Documents*)](#5-el-patrón-embebido-embedded-documents)
   * [5.1 Estructura con Arrays de Subdocumentos](#51-estructura-con-arrays-de-subdocumentos)
   * [5.2 Ventajas y Consideraciones de Rendimiento](#52-ventajas-y-consideraciones-de-rendimiento)
6. [Cuadro Comparativo: Modelo Embebido vs. Modelo Referenciado](#6-cuadro-comparativo-modelo-embebido-vs-modelo-referenciado)
7. [Preguntas de Reflexión y Autoevaluación](#7-preguntas-de-reflexión-y-autoevaluación)

---

## 1. Introducción al Modelado Documental en MongoDB

En el paradigma relacional tradicional, el esquema de base de datos se traduce en un conjunto rígido de tablas, columnas, tipos estrictos y restricciones referenciales (claves foráneas).

En contraposición, **MongoDB** organiza los datos en **Colecciones** (*Collections*) y **Documentos** (*Documents*):

```
       MODELO RELACIONAL (RDBMS)                 MODELO DOCUMENTAL (MongoDB)
┌───────────────────────────────────────┐   ┌─────────────────────────────────────────┐
│ Base de Datos                         │   │ Base de Datos                           │
│  └── Tabla                            │   │  └── Colección (Collection)             │
│       └── Fila / Registro / Tupla     │   │       └── Documento (Document JSON/BSON)│
│            └── Columna / Atributo     │   │            └── Campo / Clave-Valor      │
└───────────────────────────────────────┘   └─────────────────────────────────────────┘
```

* **Esquema flexible (Polimórfico):** Los documentos que coexisten dentro de una misma colección no están forzados a poseer idéntica estructura ni los mismos tipos de datos.
* **Autocontención:** La información relacionada puede viajar integrada dentro del mismo documento, reduciendo drásticamente las consultas dispersas.

---

## 2. Estructura BSON y Paridad Clave-Valor

MongoDB persiste físicamente los documentos en disco utilizando **BSON** (*Binary JSON*), una codificación binaria que extiende el estándar JSON permitiendo tipos adicionales como `ObjectId`, fechas nativas (`Date`) y binarios.

### Sintaxis Base
```json
{
  "campo1": "valor1",
  "campo2": 500,
  "campo3": true
}
```

* **Clave (*Key*):** Cadena de caracteres que actúa como identificador del atributo.
* **Valor (*Value*):** Dato asociado; puede ser un escalar (número, string, booleano), un subdocumento anidado o un arreglo (`array`).
* Los valores numéricos se representan sin comillas (por ejemplo, `500`), a diferencia de los textos.

---

## 3. Técnicas de Modelamiento en MongoDB

### 3.1 Modelo de Datos Integrado (Desnormalizado / Embebido)

En este modelo, todos los datos vinculados a una entidad de negocio se almacenan dentro de **un único documento**. No se aplican las reglas clásicas de normalización (1FN, 2FN, 3FN).

#### Ejemplo de Documento Monolítico Integrado
```json
{
  "_id": ObjectId("5099803df3f4948bd2f98391"),
  "Emp_ID": "55555555-5",
  "Datos_generales": {
    "Nombre": "Julio",
    "Apellidos": "Moraga"
  },
  "Contacto": {
    "Fecha_nacimiento": "1995-09-26",
    "e-mail": "jmoraga@gmail.com",
    "telefono": "+56932108654"
  },
  "Direccion": {
    "Ciudad": "Santiago",
    "Comuna": "San Miguel",
    "País": "Chile",
    "Region": "RM"
  }
}
```
* **Ventaja:** Toda la información del colaborador se recupera con una única lectura en disco por `_id`.

---

### 3.2 Modelo de Datos Normalizado (Referenciado)

Los datos se separan en colecciones independientes y se enlazan mediante referencias documentales (almacenando el `_id` del documento maestro), simulando claves foráneas:

```
[ Colección: Empleados ]
{
  "_id": ObjectId("1001"),
  "Emp_ID": "55555555-5"
}

[ Colección: Datos_Generales ]
{
  "_id": ObjectId("1002"),
  "empDocID": ObjectId("1001"),
  "Nombre": "Julio",
  "Apellidos": "Moraga",
  "Fecha_nacimiento": "1995-09-26"
}

[ Colección: Contacto ]
{
  "_id": ObjectId("1003"),
  "empDocID": ObjectId("1001"),
  "e-mail": "jmoraga@gmail.com",
  "telefono": "+56932108654"
}

[ Colección: Direcciones ]
{
  "_id": ObjectId("1004"),
  "empDocID": ObjectId("1001"),
  "Ciudad": "Santiago",
  "Comuna": "San Miguel"
}
```

* **Ventaja:** Elimina la duplicidad y facilita la actualización atómica de un dato puntual.
* **Desventaja:** Requiere múltiples lecturas o el uso de la etapa `$lookup` en agregaciones para reconstruir la entidad completa.

---

## 4. Buenas Prácticas de la Industria en el Diseño NoSQL

### 4.1 Desnormalizar vs. Normalizar (Decisión Arquitectónica)

* **Priorizar lectura rápida:** Si la aplicación realiza miles de lecturas por segundo de perfiles completos y tolera eventuales retrasos breves en la consistencia, se **desnormaliza (embebido)**.
* **Priorizar consistencia estricta:** Si no es admisible tener datos divergentes ni por un segundo, se **normaliza (referenciado)**, asumiendo el coste en tiempo de múltiples consultas.

### 4.2 Preparar Datos para el Futuro
Cuando los datos serán consumidos por múltiples sistemas externos aún no desarrollados con patrones de consulta impredecibles, estructurar los datos de forma normalizada suele ofrecer mayor versatilidad evolutiva.

### 4.3 Incrustación de Campos Dependientes (Instantáneas de Datos)
En operaciones de compraventa o pedidos, la información de despacho o precios históricos **debe quedar incrustada como una instantánea (*snapshot*)**:
* Si un cliente modifica su domicilio en su perfil de usuario al día siguiente, el pedido despachado debe mantener la dirección original de entrega registrada al momento de la compra.

### 4.4 Principio de Documentos Autosuficientes
MongoDB está optimizado como un repositorio de alta velocidad para almacenar y despachar datos. Las consultas que obligan al motor a calcular valores en tiempo de ejecución (mediante funciones JavaScript) penalizan severamente el rendimiento.

* **Antipatrón (requiere cómputo de JavaScript en servidor):**
  ```json
  {
    "_id": 123456,
    "lapices": 10,
    "cuadernos": 5
  }
  ```
* **Patrón Recomendado (hacer explícito el total en el documento):**
  ```json
  {
    "_id": 123456,
    "lapices": 10,
    "cuadernos": 5,
    "total": 15
  }
  ```

---

## 5. El Patrón Embebido (*Embedded Documents*)

Consiste en incrustar objetos o arreglos de objetos directamente en las propiedades del documento contenedor.

### 5.1 Estructura con Arrays de Subdocumentos

En el modelo de una biblioteca virtual, los comentarios de un libro se incrustan como una lista dentro del documento del libro:

```json
{
  "_id": 1,
  "titulo": "Introducción a las Bases Datos NoSQL Usando MongoDB",
  "contenido": "En este manual lleva a cabo una introducción...",
  "autor": "Antonio Sarasa",
  "isbn": "978-84-9116-266-7",
  "comentarios": [
    {
      "autor": "Julio Moraga",
      "mail": "jmoraga@gmail.com",
      "contenido": "Me parece un excelente libro..."
    },
    {
      "autor": "Pedro Torres",
      "mail": "pedro.torres@gmail.com",
      "contenido": "Me permitió conocer de mejor..."
    },
    {
      "autor": "Sebastián Blanco",
      "mail": "blanco.s@outlook.com",
      "contenido": "Libro muy útil para..."
    }
  ]
}
```

### 5.2 Ventajas y Consideraciones de Rendimiento

* **Eficiencia de consulta:** Recupera el libro y todos sus comentarios en un solo acceso a disco sin uniones.
* **Límite de tamaño en BSON:** MongoDB impone un límite estricto de **16 MB por documento**. Si un arreglo crece sin control (por ejemplo, millones de comentarios), debe migrarse al modelo referenciado.
* **Costo de actualización:** Si un usuario cambia su correo, habría que actualizar múltiples subdocumentos dispersos en distintos libros.

---

## 6. Cuadro Comparativo: Modelo Embebido vs. Modelo Referenciado

| Característica | Modelo Embebido (Integrado) | Modelo Referenciado (Normalizado) |
| :--- | :--- | :--- |
| **Relación Cardinal** | Ideal para $1:1$ y $1:\text{Pocos}$ | Ideal para $1:\text{Muchos (masivos)}$ y $M:N$ |
| **Operación de Lectura** | Una sola lectura rápida (documento completo) | Múltiples lecturas o etapas `$lookup` |
| **Integridad de Datos** | Puede generar redundancia e inconsistencias | Centralizada y sin duplicidad |
| **Límite BSON** | Riesgo de alcanzar los 16 MB si crece sin tope | Seguro; los documentos hijos son independientes |
| **Casos Típicos** | Direcciones de contacto, snapshots de boletas | Catálogo de productos masivo, logs de auditoría |

---

## 7. Preguntas de Reflexión y Autoevaluación

1. **¿Por qué la paridad clave-valor en MongoDB permite lecturas tan eficientes?**
   * Porque cada atributo cuenta con una clave directa que el motor indexa y recupera mediante algoritmos hash, eliminando la necesidad de recorrer tablas cruzadas.

2. **¿En qué situación es un error de diseño utilizar subdocumentos embebidos?**
   * Cuando la cantidad de elementos secundarios es ilimitada (relaciones $1:\text{Infinitos}$, como el registro de logs de un servidor), ya que tarde o temprano se superará la cuota de 16 MB por documento en MongoDB.

3. **¿A qué se refiere el principio de "documento autosuficiente"?**
   * A que el documento contenga todos los datos y totales precalculados que el cliente necesita consumir, evitando forzar al motor de base de datos a ejecutar código de cómputo en cada consulta.
