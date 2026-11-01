# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## MANUAL DE ESTUDIO 1: DISEÑO E IMPLEMENTACIÓN DE UNA BASE DE DATOS NOSQL O DOCUMENTAL (ME_1)

* **Asignatura:** Modelamiento y Programación No-SQL  
* **Unidad N° 1:** Diseño e Implementación de una Base de Datos NoSQL o Documental (Semana 1)  
* **Objetivo:** Identificar conceptualmente una base de datos relacional y una base de datos NoSQL considerando las características propias de su funcionamiento, arquitectura y patrones de modelamiento.

---

## 🧭 Tabla de Contenidos

1. [Evolución Histórica y Categorías de Bases de Datos](#1-evolución-histórica-y-categorías-de-bases-de-datos)
   * [1.1 Bases de Datos Jerárquicas](#11-bases-de-datos-jerárquicas)
   * [1.2 Bases de Datos en Red](#12-bases-de-datos-en-red)
   * [1.3 Bases de Datos Relacionales (RDBMS / SQL)](#13-bases-de-datos-relacionales-rdbms--sql)
   * [1.4 Bases de Datos NoSQL / Documentales](#14-bases-de-datos-nosql--documentales)
2. [El Surgimiento del Movimiento NoSQL](#2-el-surgimiento-del-movimiento-nosql)
   * [2.1 El Impacto de la Web 2.0 y Redes Sociales](#21-el-impacto-de-la-web-20-y-redes-sociales)
   * [2.2 Limitaciones del Enfoque Tradicional](#22-limitaciones-del-enfoque-tradicional)
3. [Características y Ventajas del Modelo NoSQL](#3-características-y-ventajas-del-modelo-nosql)
   * [3.1 Escalabilidad Horizontal vs. Vertical](#31-escalabilidad-horizontal-vs-vertical)
   * [3.2 Desempeño y Manejo de Recursos](#32-desempeño-y-manejo-de-recursos)
   * [3.3 Eliminación de Cuellos de Botella](#33-eliminación-de-cuellos-de-botella)
4. [Comparativa Directa: SQL vs. NoSQL](#4-comparativa-directa-sql-vs-nosql)
5. [Conceptos de Cardinalidad y su Adaptación al Entorno NoSQL](#5-conceptos-de-cardinalidad-y-su-adaptación-al-entorno-nosql)
   * [5.1 Tipos Clásicos de Cardinalidad](#51-tipos-clásicos-de-cardinalidad)
   * [5.2 Adaptación a Collections y Documentos (MongoDB)](#52-adaptación-a-collections-y-documentos-mongodb)
6. [Preguntas de Reflexión y Autoevaluación](#6-preguntas-de-reflexión-y-autoevaluación)

---

## 1. Evolución Histórica y Categorías de Bases de Datos

El manejo de datos en las organizaciones transitó desde sistemas de archivos planos (años 60) hacia repositorios integrados. La industria de TI ha transitado por cuatro clasificaciones fundamentales:

```
  [Años 60]              [Años 70]             [Años 80 - 2000]           [2000+]
  Jerárquicas    ──►       En Red       ──►      Relacionales      ──►     NoSQL
(Árbol 1:N Padre)    (Grafos M:N Codasyl)     (Tablas Codd / SQL)     (Flexible/Distribuido)
```

### 1.1 Bases de Datos Jerárquicas
* **Estructura:** Organizan la información en forma de árbol invertido (relación padre/hijo).
* **Reglas:**
  * El nivel 0 representa el nodo raíz (*root*).
  * Un nodo padre puede tener un número ilimitado de hijos.
  * Un nodo hijo solo puede tener un único nodo padre.
  * Los nodos sin descendientes se denominan "hojas".
* **Ejemplo clásico:** **Adabas** (*Adaptable Database System*), que evolucionó añadiendo capas de consulta SQL y conectores para persistir en entornos empresariales.
* **Uso actual:** Muy reducido en la industria moderna debido a la rigidez de su esquema.

### 1.2 Bases de Datos en Red
* **Estructura:** Planteada para superar las limitaciones del modelo jerárquico mediante registros y enlaces.
* **Relaciones:** Permite que un nodo hijo tenga **más de un padre**, facilitando la representación directa de relaciones muchos a muchos ($M:N$).
* **Estándar:** Modelo **CODASYL** (fines de los 60 y años 70).
* **Uso actual:** Prácticamente en desuso operativo debido a la complejidad de mantenimiento de punteros de red.

### 1.3 Bases de Datos Relacionales (RDBMS / SQL)
Propuestas a principios de los años 70 por **Edgar F. Codd** (IBM). Constituyen el estándar corporativo dominante de las últimas décadas.
* **Componentes:**
  * **Tablas (Entidades):** Estructura columnar compuesta por atributos (columnas) y registros/tuplas (filas).
  * **Integridad Referencial:** Claves Primarias (**PK**) para unicidad y Claves Foráneas (**FK**) para vincular registros entre entidades.
  * **Lenguaje Estándar:** SQL (*Structured Query Language*).
  * **Propiedades ACID:** Garantizan consistencia estricta en transacciones financieras y comerciales.

### 1.4 Bases de Datos NoSQL / Documentales
Sistemas creados para almacenar, estructurar y consultar datos no estructurados o semi-estructurados sin requerir esquemas tabulares fijos:
* **Estructura:** Agrupan información en **Documentos** (generalmente JSON/BSON) que contienen todos los datos y arrays vinculados en un solo objeto.
* **Agrupación:** Los documentos se almacenan dentro de **Colecciones** (*Collections*).
* **Motor referencial:** **MongoDB**, con implementaciones locales (*On-Premise*) y plataformas en la nube (*MongoDB Atlas*).

---

## 2. El Surgimiento del Movimiento NoSQL

### 2.1 El Impacto de la Web 2.0 y Redes Sociales
Con el surgimiento de la Web 2.0, los usuarios dejaron de ser simples lectores de contenido y pasaron a ser generadores masivos de datos (publicaciones, multimedia, interacciones en tiempo real, registros de chats, eventos de telemetría).

Este crecimiento exponencial introdujo dos retos insalvables para el modelo relacional puro:
1. **Volumen de escala masiva (Terabytes y Petabytes diarios).**
2. **Heterogeneidad de datos no estructurados** (imposibles de encajar limpiamente en columnas predefinidas sin generar campos nulos masivos).

### 2.2 Limitaciones del Enfoque Tradicional
* **Escalamiento Vertical Costoso:** Los RDBMS tradicionales escalaban añadiendo más CPU, RAM y discos de alta gama al mismo servidor (*Scale-Up*), lo que genera techos de hardware y costes elevados.
* **Sobrecarga de Sentencias y Bloqueos:** El procesamiento de consultas complejas con múltiples uniones (`JOIN`) bajo alta concurrencia produce cuellos de botella en la contención de cerraduras (*locks*).

---

## 3. Características y Ventajas del Modelo NoSQL

El término **NoSQL** se interpreta formalmente como *"Not only SQL"* (No solo SQL):

```
       CARACTERÍSTICAS DEL MODELO NoSQL
   ┌────────────────────────────────────────┐
   │ 1. Modelado Flexible (Sin esquema rígido)│
   │ 2. Escalabilidad Horizontal (Nodos)    │
   │ 3. Escritura y Lectura Ultra Rápida    │
   │ 4. Arquitectura Distribuida (Sharding) │
   └────────────────────────────────────────┘
```

### 3.1 Escalabilidad Horizontal vs. Vertical
* **Escalabilidad Horizontal (*Scale-Out*):** Para aumentar la capacidad de procesamiento o almacenamiento, simplemente se agregan máquinas estándar (nodos) al clúster mediante particionado distribuido (*sharding*) y replicación.
* **Bajo coste de infraestructura:** No exige supercomputadores; opera sobre servidores estándar de menor coste.

### 3.2 Desempeño y Manejo de Recursos
* **Priorización de Disponibilidad y Escritura:** Diseñadas para escribir ráfagas masivas de datos sin esperar a resolver bloqueos relacionales complejos.
* **Consultas por Clave:** Emplean algoritmos indexados y tablas Hash distribuidas que resuelven lecturas en tiempos reducidos.

### 3.3 Eliminación de Cuellos de Botella
Al no requerir compilación y análisis sintáctico de sentencias relacionales con bloqueos de integridad transaccional inmediata en cada tabla secundaria, el sistema procesa transacciones concurrentes en paralelo sin saturar un punto único centralizado.

---

## 4. Comparativa Directa: SQL vs. NoSQL

| Criterio | Bases de Datos Relacionales (SQL) | Bases de Datos NoSQL (Documentales) |
| :--- | :--- | :--- |
| **Estructura básica** | Tablas, Filas (tuplas) y Columnas. | Colecciones (*Collections*) y Documentos (JSON/BSON). |
| **Esquema (*Schema*)** | Rígido, tipado y predefinido (`DDL`). | Dinámico, polimórfico y flexible. |
| **Identificadores** | Claves Primarias (`PK`) y Foráneas (`FK`). | Identificador único documental (`_id`), sin FKs automáticas. |
| **Operaciones JOIN** | Soportadas nativamente mediante SQL. | Evitadas habitualmente; se desnormaliza o se resuelve por software. |
| **Lenguaje de consulta** | Estándar SQL (`SELECT`, `INSERT`, etc.). | Específico por motor (MongoDB Query API/JSON, CQL en Cassandra). |
| **Escalamiento** | Principalmente Vertical (*Scale-Up*). | Horizontal (*Scale-Out* distribuido en clúster). |
| **Enfoque de diseño** | Normalización (eliminar redundancia). | Desnormalización (anidar datos para optimizar lecturas). |

---

## 5. Conceptos de Cardinalidad y su Adaptación al Entorno NoSQL

En el diseño relacional, la cardinalidad delimita el número de entidades que pueden asociarse entre dos conjuntos:

```
  Uno a Uno (1:1)       : [Automóvil] ───────── [Patente]
  Uno a Muchos (1:N)    : [Género Cinematográfico] ───────< [Películas]
  Muchos a Muchos (M:N) : [Compradores] >───────< [Productos]
  Recursiva             : [Empleado/Supervisor] ↺
```

### 5.1 Tipos Clásicos de Cardinalidad
* **Uno a Uno ($1:1$):** Una entidad A se asocia como máximo con una entidad B, y viceversa (ej. un automóvil y su placa patente).
* **Uno a Muchos ($1:N$):** Una entidad A se vincula a varias entidades B, pero una entidad B pertenece a una sola entidad A (ej. un género musical y sus álbumes).
* **Muchos a Muchos ($M:N$):** La entidad A se asocia con múltiples entidades B, y cada B se asocia con múltiples A (ej. clientes y productos adquiridos).
* **Recursiva:** Una entidad se relaciona consigo misma (ej. un empleado que supervisa a otros empleados del mismo equipo).

### 5.2 Adaptación a Collections y Documentos (MongoDB)
En NoSQL no existen las restricciones de clave foránea (*Foreign Key Constraints*) gestionadas por el motor de base de datos. Sin embargo, los conceptos de cardinalidad se resuelven mediante dos estrategias de diseño documental:

1. **Documentos Incrustados / Embebidos (*Embedding*):**
   * Se utiliza idealmente para relaciones $1:1$ o $1:N$ donde los elementos secundarios se consultan siempre junto con el documento principal.
   ```json
   {
     "_id": "CLI01",
     "nombre": "Ana Pérez",
     "direcciones": [
       { "ciudad": "Santiago", "tipo": "Casa" },
       { "ciudad": "Valparaíso", "tipo": "Oficina" }
     ]
   }
   ```
2. **Referencias Documentales (*Referencing*):**
   * Se utiliza en relaciones $M:N$ o $1:N$ masivas para evitar que un solo documento crezca indefinidamente sobrepasando el límite de tamaño de documento (ej. 16 MB en MongoDB):
   ```json
   {
     "_id": "PED100",
     "cliente_id": "CLI01",
     "total": 35000
   }
   ```

---

## 6. Preguntas de Reflexión y Autoevaluación

1. **¿Por qué las bases de datos NoSQL suelen desnormalizar los datos en lugar de usar `JOIN` masivos?**  
   * Porque en entornos con grandes volúmenes distribuidos en múltiples nodos de red, resolver un `JOIN` implica transferir datos a través de la red física entre servidores, penalizando severamente el tiempo de respuesta. Anidar la información en un solo documento permite recuperarla en una sola operación de lectura por clave.

2. **¿En qué casos sigue siendo preferible una base de datos relacional (SQL) frente a una NoSQL?**  
   * En sistemas donde la integridad transaccional estricta (cumplimiento ACID total) sea obligatoria y prioritaria sobre la velocidad de escritura masiva, como en sistemas contables, plataformas bancarias de transferencias o ERPs empresariales estructurados.

3. **¿Cómo se maneja la consistencia de datos en NoSQL si no existen claves foráneas automáticas?**  
   * La consistencia se traslada a la **capa de aplicación** (el código del backend en Python, Node.js, etc.), donde el desarrollador programa las validaciones de negocio previas a la persistencia.
