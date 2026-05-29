# Analizador de Tickets de Supermercado
## 🚀 Objetivo
Construir un historial real de precios basado en tickets propios y analizar su evolución.
Proyecto personal para almacenar, normalizar y analizar tickets de compras de distintas cadenas de supermercados utilizando **Excel** y **Power BI**.

## 🗂 Estructura de la Base de Datos

- **cadenas**: lista de cadenas (Vital, Carrefour, etc.)
- **tiendas**: sucursales asociadas a cada cadena
- **articulos**: artículos identificados por código del ticket
- **compras**: tabla de hechos con precios, cantidades y fechas
- **xref_articulos**: tabla manual para unificar códigos equivalentes entre cadenas

## 📊 Power BI

El modelo se conecta directamente a PostgreSQL y permite:

- comparar precios entre cadenas  
- ver evolución histórica por artículo  
- analizar variaciones por fecha  
- detectar ofertas reales  

## ⚠️ Disclaimer

Este proyecto es **personal** y se basa en tickets reales propios.  
Los datos cargados no representan precios oficiales ni información comercial de ninguna cadena.  
El repositorio se publica únicamente con fines educativos y de análisis personal.

## 🧩 ¿Querés usar este proyecto?

Si clonás este repositorio, **no vas a tener la base PostgreSQL** con mis datos.  
Para usar el Power BI o reproducir el modelo, tenés dos opciones:

### ✔ Opción A - Crear tu propia base desde cero
1. Crear una base PostgreSQL vacía  
2. Crear las tablas manualmente usando los scripts incluidos en el README (más abajo)  
3. Cargar tus propios artículos y compras (CSV o carga manual)  
4. Abrir el archivo `.pbix`  
5. Actualizar la conexión a tu servidor local  

### ✔ Opción B - Usar Power BI sin PostgreSQL
Si no querés usar PostgreSQL, podés:

- reemplazar las tablas por archivos CSV  
- usar Power BI en modo **Import**  
- mantener la misma estructura de columnas  

El modelo funciona igual mientras respetes los nombres de las tablas y campos.

## 🧱 Scripts SQL (para quien quiera recrear la base)

```sql
CREATE TABLE cadenas (
    id SERIAL PRIMARY KEY,
    nombre TEXT NOT NULL
);

CREATE TABLE tiendas (
    id SERIAL PRIMARY KEY,
    cadena_id INTEGER NOT NULL REFERENCES cadenas(id),
    nombre TEXT NOT NULL
);

CREATE TABLE articulos (
    codigo VARCHAR(20) PRIMARY KEY,
    descripcion TEXT NOT NULL
);

CREATE TABLE compras (
    id SERIAL PRIMARY KEY,
    tienda_id INTEGER NOT NULL REFERENCES tiendas(id),
    articulo_codigo VARCHAR(20) NOT NULL REFERENCES articulos(codigo),
    fecha DATE NOT NULL,
    cantidad INTEGER NOT NULL,
    precio_unit NUMERIC(12,2) NOT NULL
);

CREATE TABLE xref_articulos (
    codigo VARCHAR(20) PRIMARY KEY,
    codigo_a_mostrar VARCHAR(20) NOT NULL
);
