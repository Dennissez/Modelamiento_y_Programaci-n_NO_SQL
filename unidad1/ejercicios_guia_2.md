# MODELAMIENTO Y PROGRAMACIÓN NO-SQL

## GUÍA PRÁCTICA 2: INSTALACIÓN Y GESTIÓN DE BASES DE DATOS EN MONGODB (R_2 / EJ_2)

* **Asignatura:** Modelamiento y Programación No-SQL
* **Unidad N° 1:** Diseño e Implementación de una Base de Datos NoSQL o Documental (Semana 2 / Actividad N°1)
* **Objetivo:** Implementar un entorno de base de datos NoSQL mediante la instalación y configuración de MongoDB Community Server, MongoDB Compass y MongoDB Shell (`mongosh`), estructurando la base de datos `tienda`, la colección `productos` y realizando la carga masiva de datos desde archivos delimitados (`CSV`).

---

## 🧭 Tabla de Contenidos

1. [Arquitectura del Entorno de Trabajo](#1-arquitectura-del-entorno-de-trabajo)
2. [Paso 1: Instalación de MongoDB Community Server](#2-paso-1-instalación-de-mongodb-community-server)
3. [Paso 2: Instalación y Configuración de MongoDB Compass](#3-paso-2-instalación-y-configuración-de-mongodb-compass)
4. [Paso 3: Instalación de la Consola de Comandos (mongosh)](#4-paso-3-instalación-de-la-consola-de-comandos-mongosh)
5. [Paso 4: Creación de la Base de Datos `tienda` y Colección `productos`](#5-paso-4-creación-de-la-base-de-datos-tienda-y-colección-productos)
   * [5.1 Vía Gráfica (MongoDB Compass)](#51-vía-gráfica-mongodb-compass)
   * [5.2 Vía Consola Interactiva (`mongosh`)](#52-vía-consola-interactiva-mongosh)
6. [Paso 5: Importación Masiva de Datos (`productos.csv`)](#6-paso-5-importación-masiva-de-datos-productoscsv)
   * [6.1 Mapeo de Tipos en Compass (Delimitador Semicolon)](#61-mapeo-de-tipos-en-compass-delimitador-semicolon)
   * [6.2 Estructura Documental Resultante (BSON)](#62-estructura-documental-resultante-bson)
   * [6.3 Alternativa por Terminal (`mongoimport`)](#63-alternativa-por-terminal-mongoimport)
7. [Alternativa Profesional: Despliegue Rápido con Docker](#7-alternativa-profesional-despliegue-rápido-con-docker)
8. [Consultas Básicas de Verificación en `mongosh`](#8-consultas-básicas-de-verificación-en-mongosh)

---

## 1. Arquitectura del Entorno de Trabajo

Para operar de forma completa con MongoDB en el entorno académico y profesional se articulan tres componentes clave:

```
┌──────────────────────────────────────────────────────────────┐
│                    Herramientas de Cliente                   │
│                                                              │
│   ┌───────────────────────────┐  ┌───────────────────────┐   │
│   │  MongoDB Compass (GUI)    │  │  mongosh (Terminal)   │   │
│   │  Puerto predeterminado    │  │  Línea de comandos    │   │
│   └─────────────┬─────────────┘  └───────────┬───────────┘   │
└─────────────────┼────────────────────────────┼───────────────┘
                  │   URI: mongodb://localhost:27017/          
                  ▼                            ▼                
┌──────────────────────────────────────────────────────────────┐
│             MongoDB Community Server (Servicio)              │
│  - Motor de almacenamiento (WiredTiger)                      │
│  - Formato binario en disco: BSON                            │
│  - Puerto estándar de escucha: 27017                         │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. Paso 1: Instalación de MongoDB Community Server

1. **Descarga del Instalador:**
   * Ingresar a la sección oficial de descargas: [MongoDB Community Server Download](https://www.mongodb.com/try/download/community).
   * Parámetros de selección recomendados para entorno Windows:
     * **Version:** `7.0.1 (Current)` o superior.
     * **Platform:** `Windows x64`.
     * **Package:** `msi`.
   * Archivo obtenido: `mongodb-windows-x86_64-7.0.1-signed.msi`.

2. **Ejecución del Asistente:**
   * Ejecutar el archivo `.msi` con permisos de administrador.
   * En la pantalla de bienvenida pulsar **Next**.
   * Aceptar la licencia de usuario final (*Server Side Public License - SSPL*) marcando el checkbox **"I accept the terms in the License Agreement"** y pulsar **Next**.

3. **Tipo de Instalación:**
   * Seleccionar **Complete** para asegurar que todos los binarios y librerías del motor queden instalados.

4. **Configuración del Servicio (*Service Configuration*):**
   * Mantener activa la opción predeterminada **"Install MongoDB as a Service"**.
   * Parámetros por defecto:
     * **Service Name:** `MongoDB`.
     * **Data Directory:** `C:\Program Files\MongoDB\Server\7.0\data\`.
     * **Log Directory:** `C:\Program Files\MongoDB\Server\7.0\log\`.
   * Hacer clic en **Next**.

---

## 3. Paso 2: Instalación y Configuración de MongoDB Compass

1. En la pantalla siguiente del instalador, asegurarse de mantener marcado el casillero **"Install MongoDB Compass"**.
2. Hacer clic en **Install** para iniciar el despliegue de archivos en disco.
3. Una vez finalizado el proceso, pulsar **Finish**.
4. Al abrirse MongoDB Compass:
   * En la pantalla de bienvenida (*Welcome to Compass*), hacer clic en **Start**.
   * En la pantalla de conexión principal (*New Connection*), la cadena de conexión por defecto es:
     ```text
     mongodb://localhost:27017/
     ```
   * Hacer clic en el botón verde **Connect**.
   * Compass mostrará las tres bases de datos administrativas del sistema: `admin`, `config` y `local`.

---

## 4. Paso 3: Instalación de la Consola de Comandos (mongosh)

Compass cuenta con una consola interactiva embebida en su barra inferior (`>_MONGOSH`). Si se requiere tener la consola disponible de forma independiente desde cualquier terminal de Windows (CMD o PowerShell):

1. **Descarga:**
   * Acceder a [MongoDB Shell Download](https://www.mongodb.com/try/download/shell).
   * Seleccionar la versión `Windows 64-bit (MSI)` y descargar `mongosh-1.10.6-x64.msi`.
2. **Instalación:**
   * Ejecutar el instalador, aceptar las opciones por defecto o definir la ruta:
     ```text
     C:\Program Files\MongoDB\Server\7.0\mongosh\
     ```
3. **Comprobación:**
   * Abrir CMD / PowerShell y ejecutar:
     ```bash
     mongosh
     ```
   * El indicador cambiará a `test>`, confirmando la conexión al nodo local `127.0.0.1:27017`.

---

## 5. Paso 4: Creación de la Base de Datos `tienda` y Colección `productos`

### 5.1 Vía Gráfica (MongoDB Compass)

1. En la barra lateral izquierda, dentro de la sección **Databases**, hacer clic en el botón **`+`** (*Create database*).
2. Completar los campos del modal:
   * **Database Name:** `tienda`
   * **Collection Name:** `productos`
3. Dejar desmarcado *Time-Series* y pulsar **Create Database**.
4. En el panel izquierdo aparecerá la base de datos `tienda` y su colección hija `productos`.

### 5.2 Vía Consola Interactiva (`mongosh`)

Para replicar el proceso vía comandos en la terminal o la consola integrada de Compass:

```javascript
// 1. Conmutar al contexto de la base de datos 'tienda'
use tienda

// 2. Crear formalmente la colección 'productos'
db.createCollection("productos")

// 3. Verificar las colecciones creadas
show collections
```

> [!NOTE]
> En MongoDB, el objeto `db` actúa como un comodín o puntero dinámico que hace referencia a la base de datos actualmente seleccionada por el comando `use`.

---

## 6. Paso 5: Importación Masiva de Datos (`productos.csv`)

### 6.1 Mapeo de Tipos en Compass (Delimitador Semicolon)

1. Seleccionar la colección `tienda.productos` en Compass.
2. Hacer clic en el botón verde **ADD DATA** $\rightarrow$ **Import JSON or CSV file**.
3. Seleccionar el archivo local `productos.csv`.
4. Configurar las opciones de carga en el modal:
   * **Select delimiter:** `Semicolon` (punto y coma `;`).
   * **Ignore empty strings:** Activado.
5. **Tipificación de Campos:**
   * `codigo`: `String`
   * `nombre`: `String`
   * `marca`: `String`
   * `precio`: `Int32`
   * `stock`: `Int32`
6. Hacer clic en el botón **Import**. Compass notificará la carga de los 15 documentos.

### 6.2 Estructura Documental Resultante (BSON)

Cada fila del archivo plano se almacena como un documento independiente provisto de un identificador único generado por MongoDB:

```json
{
  "_id": ObjectId("650dbf528045172aad43e33d"),
  "codigo": "PROD001",
  "nombre": "Apple Watch SE",
  "marca": "Apple",
  "precio": 219990,
  "stock": 100
}
```

### 6.3 Alternativa por Terminal (`mongoimport`)

Si se prefiere realizar la carga sin interfaz gráfica utilizando las herramientas de terminal (*Database Tools*):

```bash
mongoimport --db tienda --collection productos --type csv --delimiter ";" --headerline --file "productos.csv"
```

---

## 7. Alternativa Profesional: Despliegue Rápido con Docker

Para desplegar el entorno completo sin realizar instalaciones directas en el sistema operativo host, se puede utilizar el siguiente comando de Docker:

```bash
docker run -d \
  --name mongodb-iplacex \
  -p 27017:27017 \
  -v mongo_data:/data/db \
  mongo:7.0
```

Para conectarse a este contenedor desde MongoDB Compass, simplemente se utiliza la misma URI local:
```text
mongodb://localhost:27017
```

---

## 8. Consultas Básicas de Verificación en `mongosh`

Una vez cargados los datos, ejecutar los siguientes comandos para validar el correcto almacenamiento:

```javascript
// Usar la base de datos
use tienda

// Contar los documentos importados (debe retornar 15)
db.productos.countDocuments()

// Consultar los tres primeros productos formateados
db.productos.find().limit(3).pretty()

// Filtrar productos por marca
db.productos.find({ marca: "Apple" })

// Filtrar productos con stock mayor a 80 unidades
db.productos.find({ stock: { $gt: 80 } })
```
