# Master of Table's

## Master of Tables

### &#x20;Manual de Referencia y Diseño Estructurado DDL en SQL

### 1. La Biblioteca de las Tablas

\
La creación de tablas en SQL sigue una estructura jerárquica que define la organización de los datos. La sintaxis `CREATE TABLE` es la base para estructurar cualquier almacenamiento de información en bases de datos relacionales. La opción `TEMPORARY` crea tablas que existen solo durante la sesión actual, ideales para cálculos intermedios. `IF NOT EXISTS` previene errores en scripts de despliegue, mientras que las opciones de tabla permiten optimizar el almacenamiento según el uso específico.

#### Metáfora Visual<br>

Imagina que crear una tabla es como construir una biblioteca. `CREATE TABLE` sería el terreno y la estructura principal, las columnas son los diferentes tipos de estanterías (para libros grandes, pequeños, revistas), las restricciones son las normas de la biblioteca (no puedes poner dos libros con el mismo código), y las opciones de tabla son decisiones sobre iluminación, distribución del espacio y sistemas de seguridad.

```
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] nombre_tabla (
    definicion_columna,
    definicion_columna,
    ...
    [restricciones_tabla]
) [OPCIONES_TABLA];
```

### 2. Tipos de Contenedores (Datos)

#### &#x20;2.1 Contenedores Numéricos

Los tipos numéricos en MySQL se dividen en exactos y aproximados. Los enteros (`TINYINT`, `SMALLINT`, `MEDIUMINT`, `INT`, `BIGINT`) ofrecen precisión absoluta. `UNSIGNED` duplica el rango positivo eliminando los valores negativos.&#x20;

Los decimales exactos (`DECIMAL`, `NUMERIC`) mantienen precisión perfecta para cálculos financieros, mientras que los flotantes (`FLOAT`, `DOUBLE`) sacrifican precisión por rango y eficiencia.<br>

Metáfora

Los tipos enteros son contenedores de diferentes tamaños: `TINYINT` son cajas pequeñas (255 ítems), `INT` son contenedores medianos (4 billones), `BIGINT` son almacenes gigantescos. `DECIMAL` es una caja fuerte para dinero, y `FLOAT` mide líquidos donde pequeñas variaciones son aceptables.

```
CREATE TABLE ejemplo_enteros (
    id TINYINT UNSIGNED,  -- Rango: 0 a 255
    edad SMALLINT,        -- Edad humana: -32,768 a 32,767
    poblacion INT UNSIGNED,-- Población: 0 a 4 billones
    cantidad_big BIGINT    -- Transacciones globales
);

CREATE TABLE ejemplo_decimales (
    precio DECIMAL(10,2),  -- Dinero exacto: $99999999.99
    impuesto DECIMAL(5,4), -- Tasas precisas: 0.9999
    ratio NUMERIC(3,2)     -- Ratios: 9.99 máximo
);
```

#### 2.2 Fecha y Hora

\
MySQL ofrece cinco tipos temporales: `DATE` (solo fechas), `TIME` (solo horas, rango de -838 a 838 horas), `DATETIME` (combina fecha y hora), `TIMESTAMP` (con soporte de zona horaria, rango 1970-2038) y `YEAR` (optimizado para años).

Metáfora

`DATE` es un calendario de pared, `TIME` un cronómetro, `DATETIME` un reloj inteligente, `TIMESTAMP` un reloj mundial sincronizado y `YEAR` un almanaque anual.

```
CREATE TABLE ejemplo_fechas (
    fecha_nacimiento DATE,
    hora_entrada TIME,
    fecha_hora_creacion DATETIME,
    ultima_modificacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    año_fundacion YEAR
);
```

#### 2.3 Cadenas

\
Se dividen en binarias (`BINARY`, `VARBINARY`) y de caracteres (`CHAR`, `VARCHAR`). `CHAR` ocupa espacio fijo, `VARCHAR` optimiza espacio. `TEXT`/`BLOB` manejan grandes volúmenes de datos. `ENUM` define opciones fijas y `SET` permite selección múltiple.

Metáfora

`CHAR` es una estantería de tamaño fijo; `VARCHAR` es ajustable; `TEXT` es una sección de libros grandes; `BLOB` guarda archivos multimedia; `ENUM` es un menú cerrado y `SET` un buffet libre.

```
CREATE TABLE ejemplo_cadenas (
    codigo CHAR(10),
    nombre VARCHAR(100),
    hash_password BINARY(32),
    descripcion_corta TINYTEXT,
    contenido_articulo TEXT,
    estado ENUM('activo','inactivo'),
    permisos SET('lectura','escritura')
);
```

#### 2.4 JSON y Espaciales

`JSON` (MySQL 5.7.8+) permite almacenar documentos validados. Los tipos espaciales (`POINT`, `POLYGON`, etc.) siguen el estándar OGC para realizar consultas geométricas (distancia, intersección).<br>

Metáfora

`JSON` es una carpeta de documentos flexibles. Los tipos espaciales son un sistema GPS integrado en la base de datos para manejar mapas y rutas.

```
CREATE TABLE ejemplo_json_espacial (
    configuracion JSON,
    ubicacion POINT,
    area_cobertura POLYGON
);
```

### 3. Definición de Columnas

\
Cada columna es una unidad de datos con atributos que controlan su comportamiento: `NOT NULL` (obligatorio), `DEFAULT` (valor por defecto), `AUTO_INCREMENT` (secuencia), `COMMENT` (documentación), `STORAGE` (optimización) y `CHECK` (validación).

Metáfora

Definir columnas es como diseñar un formulario: `NOT NULL` es un campo obligatorio (\*), `DEFAULT` son valores pre-llenados, `AUTO_INCREMENT` es un numerador automático y `CHECK` son las reglas de validación.

```
CREATE TABLE ejemplo_columnas_completo (
    id_auto INT PRIMARY KEY AUTO_INCREMENT COMMENT 'ID único',
    nombre VARCHAR(100) NOT NULL DEFAULT 'Anónimo',
    email VARCHAR(255) UNIQUE NOT NULL,
    cache_data VARCHAR(500) STORAGE MEMORY,
    creado_en TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    edad TINYINT UNSIGNED CHECK (edad >= 18 AND edad <= 120),
    estado ENUM('activo','inactivo') DEFAULT 'activo'
);
```

### 4. Las Restricciones

\
Las restricciones garantizan la integridad de los datos. `PRIMARY KEY` identifica filas unívocamente, `FOREIGN KEY` mantiene las relaciones, `UNIQUE` previene duplicados y `CHECK` aplica reglas de negocio.

Metáfora

Las restricciones son las leyes de una ciudad: `PRIMARY KEY` es el DNI único, `FOREIGN KEY` representa la relación familiar, `UNIQUE` son los números de teléfono e impedir duplicados, y `CHECK` son normas explícitas (p. ej. edad mínima).

```
CREATE TABLE sistema_ventas (
    venta_id INT,
    cliente_id INT,
    producto_id INT,
    cantidad INT,
    fecha_inicio DATE,
    fecha_fin DATE,
    
    PRIMARY KEY (venta_id, producto_id),
    UNIQUE KEY uk_venta_diaria (cliente_id, producto_id, fecha_inicio),
    CONSTRAINT fk_venta_cliente FOREIGN KEY (cliente_id) REFERENCES clientes(id) ON DELETE CASCADE,
    CONSTRAINT chk_fechas CHECK (fecha_inicio <= fecha_fin)
);
```

### 5. Índices

\
Aceleran las consultas a costa de ralentizar escrituras. Existen índices regulares (`INDEX`), de texto completo (`FULLTEXT`) y espaciales (`SPATIAL`).

Metáfora

Es el índice al final de un libro. En lugar de leer página por página (full table scan), vas directamente a la página exacta señalada por el término.

```
CREATE TABLE biblioteca_digital (
    libro_id INT PRIMARY KEY AUTO_INCREMENT,
    isbn VARCHAR(20),
    titulo VARCHAR(200),
    autor VARCHAR(100),
    contenido LONGTEXT,
    
    UNIQUE KEY uk_isbn (isbn),
    INDEX idx_autor_fecha (autor),
    FULLTEXT ft_busqueda (titulo, contenido)
) ENGINE=InnoDB;
```

### 6. Opciones de Tabla

Controlan el comportamiento a nivel de almacenamiento físico y lógico: `ENGINE` (InnoDB, MyISAM), `CHARACTER SET` / `COLLATE`, `ROW_FORMAT` y `AUTO_INCREMENT`.<br>

```
CREATE TABLE tabla_configurada (
    id BIGINT UNSIGNED PRIMARY KEY,
    datos TEXT
)
ENGINE=InnoDB
CHARACTER SET=utf8mb4 COLLATE=utf8mb4_unicode_ci
AUTO_INCREMENT=1000
ROW_FORMAT=COMPRESSED;
```

### 7. Tablas Temporales

\
Existen únicamente durante la sesión actual y se eliminan automáticamente al desconectarse.

```
CREATE TEMPORARY TABLE temp_ventas_diarias (
    producto_id INT,
    total_vendido DECIMAL(12,2)
) ENGINE=MEMORY;
```

### 8. Creación de Tablas con SELECT

Permite clonar o crear tablas basadas en una consulta con `CREATE TABLE ... AS SELECT` o copiar la estructura vacía con `CREATE TABLE ... LIKE`.

```
-- Copia datos y estructura
CREATE TABLE backup_clientes AS SELECT * FROM clientes;

-- Copia solo la estructura
CREATE TABLE clientes_test LIKE clientes;
```

### 9. ALTER TABLE

Comando DDL para modificar estructuras existentes (columnas, tipos, restricciones) sin perder datos.

```
ALTER TABLE usuarios_legacy
ADD COLUMN email VARCHAR(255) AFTER nombre,
MODIFY COLUMN nombre VARCHAR(100) NOT NULL,
ADD CONSTRAINT chk_email CHECK (email LIKE '%@%.%');
```

### 10. Particionamiento

Divide una tabla lógica en segmentos físicos (`RANGE`, `LIST`, `HASH`) para mejorar el rendimiento con datos voluminosos.

```
CREATE TABLE logs_sistema (
    id BIGINT UNSIGNED NOT NULL AUTO_INCREMENT,
    fecha_creacion DATETIME NOT NULL,
    PRIMARY KEY (id, fecha_creacion)
)
PARTITION BY RANGE (YEAR(fecha_creacion)) (
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p_actual VALUES LESS THAN MAXVALUE
);
```

### 11. Ejemplo Aplicado: E-Commerce

Integración completa de todos los conceptos anteriores estructurando una base de datos para comercio electrónico.

```
CREATE DATABASE IF NOT EXISTS ecommerce;
USE ecommerce;

CREATE TABLE clientes (
    cliente_id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    dni VARCHAR(20) NOT NULL UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    fecha_nacimiento DATE NOT NULL,
    CONSTRAINT chk_edad CHECK (fecha_nacimiento <= DATE_SUB(CURRENT_DATE, INTERVAL 18 YEAR))
) ENGINE=InnoDB;

CREATE TABLE productos (
    producto_id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    sku VARCHAR(50) NOT NULL UNIQUE,
    precio DECIMAL(10,2) UNSIGNED NOT NULL,
    stock INT UNSIGNED DEFAULT 0,
    CONSTRAINT chk_precio CHECK (precio > 0)
) ENGINE=InnoDB;
```

### Anexo: Profundización en Restricciones CHECK

Las restricciones `CHECK` son condiciones lógicas que validan los datos antes de permitir inserciones o actualizaciones. Se pueden definir a nivel de columna o a nivel de tabla.

#### Nivel de Columna vs Tabla

```
-- Nivel de Columna
CREATE TABLE emp (
    edad INT CHECK (edad >= 18)
);

-- Nivel de Tabla
CREATE TABLE emp (
    edad INT,
    salario DECIMAL,
    CONSTRAINT chk_emp CHECK (edad >= 18 AND salario > 0)
);
```

#### Metáfora: El Guardia de Seguridad

La columna es la puerta de la sala; el `CHECK` son los requisitos exigidos por el guardia para poder entrar.

```
CREATE TABLE club (
    nombre VARCHAR(100),
    edad INT CHECK (edad >= 18)
);
```
