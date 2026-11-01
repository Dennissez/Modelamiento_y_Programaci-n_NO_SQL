# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## GUÍA PRÁCTICA 1: CONCEPTOS FUNDAMENTALES SQL VS. NOSQL (R_1 / EJ_1)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 1:** Diseño e Implementación de una Base de Datos NoSQL o Documental (Semana 1 / Actividad N°1)
* **Objetivo:** Identificar conceptualmente una base de datos relacional y una base de datos NoSQL considerando las características propias de su funcionamiento.

---

## 🧭 Tabla de Contenidos
1. [Tabla Rápida de Respuestas Correctas](#1-tabla-rápida-de-respuestas-correctas)
2. [Cuestionario Completo con Preguntas, Alternativas y Justificación](#2-cuestionario-completo-con-preguntas-alternativas-y-justificación)
3. [Resumen Conceptual para Exámenes Tipo Test](#3-resumen-conceptual-para-exámenes-tipo-test)

---

## 1. Tabla Rápida de Respuestas Correctas

| N° Enunciado | Alternativa Correcta | Concepto Clave |
| :---: | :---: | :--- |
| **Enunciado #1** | **b** | Conjunto de registros en modelo relacional: **Tabla**. |
| **Enunciado #2** | **d** | Identificador unívoco que evita duplicados: **Claves Primarias (PK)**. |
| **Enunciado #3** | **c** | Detonante de NoSQL: **La Web 2.0**. |
| **Enunciado #4** | **b** | Motor donde NO se utiliza SQL de forma nativa: **Base de Datos NoSQL**. |
| **Enunciado #5** | **d** | Operación NO disponible nativamente en NoSQL: **JOIN**. |
| **Enunciado #6** | **c** | Estructura similar a jerárquica con más de un padre: **Base de Datos en Red**. |
| **Enunciado #7** | **b** | Relación A con muchos B y viceversa: **Muchos a Muchos**. |
| **Enunciado #8** | **d** | Lenguaje que NoSQL evita o usa solo de apoyo: **SQL**. |
| **Enunciado #9** | **a** | Cardinalidad entre Producto y Tipo Producto: **Uno a Muchos**. |
| **Enunciado #10** | **b** | Chat de soporte con datos no estructurados: **Base de Datos NoSQL**. |

---

## 2. Cuestionario Completo con Preguntas, Alternativas y Justificación

### Enunciado #1
En un modelo relacional ¿Cómo se llama al conjunto de registros asociados a los atributos o campos de una entidad?:

* a.- Índice.
* **b.- Tabla.** *(Correcta)*
* c.- Collection.
* d.- Ninguna de las Anteriores.

> **Justificación Técnica:**  
> En el modelo relacional propuesto por E. F. Codd, la **Tabla** es la estructura columnar bidimensional que agrupa los registros (filas o tuplas) que comparten los mismos atributos (columnas). En NoSQL documental ese concepto equivale a una *Collection*.

---

### Enunciado #2
*“…identifican unívocamente el registro o fila e impiden que este se repita dentro de la tabla y están basados en un campo con datos únicos”*, el concepto corresponde a:

* a.- Índices.
* b.- CONSTRAINTS.
* c.- Claves Foráneas
* **d.- Claves Primarias.** *(Correcta)*

> **Justificación Técnica:**  
> La **Clave Primaria** (*Primary Key* o PK) garantiza la unicidad e integridad de la entidad relacional, asegurando que ninguna fila tenga un identificador repetido ni nulo.

---

### Enunciado #3
¿Qué es lo que gatilla el surgimiento de las bases de datos NoSQL?:

* a.- La Web 3.0
* b.- Html.
* **c.- La Web 2.0** *(Correcta)*
* d.- La Web 4.0.

> **Justificación Técnica:**  
> La llegada de la **Web 2.0** transformó a los usuarios en creadores masivos de contenido en redes sociales y plataformas web, generando un volumen masivo de datos no estructurados que superó las capacidades de escala y concurrencia de los RDBMS tradicionales.

---

### Enunciado #4
En qué tipo de base de datos NO se utiliza Lenguaje SQL:

* a.- Base de Datos Jerárquica
* **b.- Base de Datos NoSQL.** *(Correcta)*
* c.- Base de Datos en Red.
* d.- Base de datos relacional.

> **Justificación Técnica:**  
> Las bases de datos **NoSQL** prescinden del estándar SQL como lenguaje de consulta principal, reemplazándolo por interfaces basadas en documentos JSON/BSON (como en MongoDB) o dialectos propios (como CQL en Cassandra o GQL en BigTable).

---

### Enunciado #5
¿Qué operaciones (o su equivalente) NO se pueden realizar en una base de datos NoSQL y si se pueden realizar en las bases de datos relacionales?

* a.- SELECT.
* b.- DELETE.
* c.- INSERT.
* **d.- JOIN.** *(Correcta)*

> **Justificación Técnica:**  
> En sistemas distribuidos NoSQL no existen las operaciones **JOIN** nativas entre colecciones. Hacer un JOIN a través de múltiples servidores físicos en red genera cuellos de botella severos, por lo que se recurre a la **desnormalización** (anidar documentos) o se resuelve en la capa de software de la aplicación.

---

### Enunciado #6
*“…son parecidas a las jerárquicas sólo que en ellas puede haber más de un padre”*, el concepto corresponde a:

* a.- Base de Datos Jerárquica
* b.- Base de Datos NoSQL.
* **c.- Base de Datos en Red.** *(Correcta)*
* d.- Base de datos relacional.

> **Justificación Técnica:**  
> En el modelo de **Base de Datos en Red** (estándar CODASYL), los registros se enlazan permitiendo relaciones de pertenencia múltiple (un nodo hijo puede tener varios nodos padres), superando la limitación de árbol estricto del modelo jerárquico.

---

### Enunciado #7
*“Una entidad A puede relacionarse con muchas entidades B y viceversa.”*, el concepto corresponde a la cardinalidad de:

* a.- Uno a Muchos.
* **b.- Muchos a Muchos.** *(Correcta)*
* c.- Uno a Uno.
* d.- Ninguna de las Anteriores.

> **Justificación Técnica:**  
> La cardinalidad **Muchos a Muchos ($M:N$)** se produce cuando una ocurrencia de la entidad A se asocia con varias de B, y al mismo tiempo una ocurrencia de B se relaciona con múltiples ocurrencias de A (por ejemplo: Compradores y Productos).

---

### Enunciado #8
*“La mayoría de las bases de datos NoSQL evitan utilizar este tipo de lenguaje o lo utilizan como un lenguaje de apoyo”*, el lenguaje al que se refieren corresponde a:

* a.- JSON.
* b.- XML.
* c.- GQL
* **d.- SQL.** *(Correcta)*

> **Justificación Técnica:**  
> El movimiento **NoSQL** (*Not only SQL*) nació precisamente para alejarse del rígido estándar **SQL**, sustituyéndolo por APIs nativas de clave-valor, documentos o grafos.

---

### Enunciado #9
Se le solicita modelar la relación entre la entidad Producto y la entidad Tipo Producto, frente a qué tipo de cardinalidad estaríamos presentes:

* **a.- Uno a Muchos.** *(Correcta)*
* b.- Muchos a Muchos.
* c.- Uno a Uno.
* d.- Ninguna de las Anteriores.

> **Justificación Técnica:**  
> Un Tipo de Producto (ej. "Electrónica") agrupa a **muchos** productos concretos (televisor, radio, tablet), pero cada producto pertenece a **un único** tipo de producto en el catálogo base. La cardinalidad es $1:N$ (*Uno a Muchos*).

---

### Enunciado #10
*“La empresa de mejoramiento del hogar Fast Home S. A. necesita analizar la percepción de sus clientes respecto del Servicio de Post-Venta. Para esto el Gerente de Servicio al Cliente se apoyará en mensajes intercambiados con el Bot de Soporte Vía Web que se encuentra en el sitio oficial para ser utilizado por sus clientes que necesitan asesoría post-venta 24/7. Para ello lo ha citado a Ud a que le recomiende cuál de las siguientes sería la base de datos que más se ajusta los requerimientos gestión de la información que se suelen utilizar en esos canales de chat”.* Dado lo expuesto, su recomendación sería:

* a.- Base de Datos Jerárquica
* **b.- Base de Datos NoSQL.** *(Correcta)*
* c.- Base de Datos en Red.
* d.- Base de datos relacional.

> **Justificación Técnica:**  
> Los historiales de mensajería y chatbots contienen **datos no estructurados y de longitud variable** (texto libre, emojis, adjuntos, metadatos variables). Una base de datos **NoSQL** (documental como MongoDB) maneja estos registros en documentos JSON flexibles sin necesidad de esquemas columnares rígidos.

---

## 3. Resumen Conceptual para Exámenes Tipo Test

```
┌─────────────────────────────────┬──────────────────────────────────┐
│   Bases de Datos Relacionales   │       Bases de Datos NoSQL       │
├─────────────────────────────────┼──────────────────────────────────┤
│ Esquema tabular rígido (DDL)    │ Esquema dinámico/flexible (JSON) │
│ Escalabilidad Vertical (CPU/RAM)│ Escalabilidad Horizontal (Nodos) │
│ Soporta operaciones complejas   │ Evita JOINs; desnormaliza datos  │
│ Integridad mediante PK y FK     │ Consistencia en capa de software │
│ Consultas estandarizadas SQL    │ APIs por motor (BSON, CQL, GQL)  │
└─────────────────────────────────┴──────────────────────────────────┘
```
