# Manual de creación de Tablas en Mysql (scripting)

## Guía de Metodología DDL

Transformación de un Diagrama Relacional a un Script SQL de Creación de Tablas



## 1 Analizar el Diagrama y Ordenar las Tablas (Jerarquía)

Antes de escribir una sola línea de código, analiza las dependencias del diagrama relacional. Las tablas no se deben crear en orden aleatorio: **primero debes crear las tablas independientes (padres)** y después las **tablas dependientes (hijas)** que apuntan a ellas mediante Claves Foráneas (`FOREIGN KEY`).

#### Metáfora de Construcción

Si intentas poner los cimientos del segundo piso sin haber construido la planta baja, la obra colapsará. En SQL, si intentas hacer referencia a una tabla que aún no existe, el motor lanzará un error de clave foránea no encontrada.

#### Tablas Padres (Fuertes)

No tienen llaves foráneas apuntando a otras tablas del sistema (ej. `clientes`, `categorias`, `paises`).

#### Tablas Hijas (Débiles/Intermedias)

Poseen llaves foráneas que referencian a tablas padre (ej. `pedidos`, `detalle_pedidos`, `productos`).

### 2 Definir la Estructura Base y Motores de Almacenamiento

Prepara el encabezado de tu script e identifica los parámetros globales de la base de datos (juego de caracteres, motor relacional).

#### Buenas Prácticas

Utiliza siempre `IF NOT EXISTS` para evitar errores al reejecutar scripts y define explícitamente el motor `InnoDB` para garantizar el soporte de integridad referencial (claves foráneas) y transacciones.

```
-- 1. Creación y selección de la Base de Datos
CREATE DATABASE IF NOT EXISTS mi_sistema
CHARACTER SET = utf8mb4 
COLLATE = utf8mb4_unicode_ci;

USE mi_sistema;
```

### 3 Mapear Atributos a Tipos de Datos Adecuados

Traducción de cada atributo visual del diagrama a su tipo de dato nativo más eficiente según los requerimientos:

#### Identificadores (IDs)

Usa `INT UNSIGNED` o `BIGINT UNSIGNED` para llaves primarias numéricas correlativas.

#### Textos y Cadenas

Usa `VARCHAR(N)` para longitudes variables y `CHAR(N)` para códigos de tamaño fijo (DNI, SKU, ISOs).

#### Valores Monetarios

Nunca uses `FLOAT` o `DOUBLE` para dinero; utiliza `DECIMAL(M,D)` para evitar redondeos imperceptibles.

#### Fechas y Estados

Usa `DATE` o `DATETIME` para marcas de tiempo, y `ENUM` para estados con valores finitos y conocidos.

### 4 Aplicar Restricciones de Columna y Reglas de Negocio

Incorpora las reglas de integridad a nivel de atributo en la declaración de columnas:

* `NOT NULL`: Si el atributo es obligatorio en el diagrama.
* `AUTO_INCREMENT`: Para llaves primarias de generación automática.
* `DEFAULT`: Para valores predeterminados (ej. fecha actual, estados por defecto).
* `CHECK`: Para validaciones lógicas directas (ej. `precio > 0`, `edad >= 18`).

```
CREATE TABLE productos (
    producto_id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    codigo_sku CHAR(10) NOT NULL UNIQUE,
    nombre VARCHAR(120) NOT NULL,
    precio DECIMAL(10,2) UNSIGNED NOT NULL,
    activo ENUM('si', 'no') DEFAULT 'si',
    CONSTRAINT chk_precio_valido CHECK (precio > 0)
) ENGINE=InnoDB;
```

### 5 Establecer Claves Primarias y Foráneas (Relaciones)

Representa las cardinalidades del diagrama mediante llaves primarias y secundarias:

#### Relaciones 1 : N (Uno a Muchos)

La clave primaria de la tabla "Uno" (Padre) se coloca como clave foránea (`FOREIGN KEY`) en la tabla "Muchos" (Hija).

#### Relaciones N : M (Muchos a Muchos)

Crea una tabla intermedia que contenga las claves primarias de ambas tablas como llaves foráneas (formando una `PRIMARY KEY` compuesta).

#### Comportamiento en Cascadas

Define qué sucede al eliminar o actualizar un registro padre mediante `ON DELETE` y `ON UPDATE`:\
• `CASCADE`: Elimina/actualiza automáticamente los registros hijos.\
• `SET NULL`: Deja el campo en nulo en los hijos.\
• `RESTRICT / NO ACTION`: Bloquea la eliminación si existen datos vinculados.

```
-- Tabla Intermedia para Relación N:M entre Pedidos y Productos
CREATE TABLE detalles_pedido (
    pedido_id INT UNSIGNED NOT NULL,
    producto_id INT UNSIGNED NOT NULL,
    cantidad SMALLINT UNSIGNED NOT NULL DEFAULT 1,
    precio_unitario DECIMAL(10,2) NOT NULL,

    PRIMARY KEY (pedido_id, producto_id),
    
    CONSTRAINT fk_detalle_pedido 
        FOREIGN KEY (pedido_id) REFERENCES pedidos(pedido_id)
        ON DELETE CASCADE ON UPDATE CASCADE,
        
    CONSTRAINT fk_detalle_producto 
        FOREIGN KEY (producto_id) REFERENCES productos(producto_id)
        ON DELETE RESTRICT ON UPDATE CASCADE
) ENGINE=InnoDB;
```

### 6 Optimizar mediante Índices y Documentación

Añade índices adicionales (`INDEX`, `FULLTEXT`) en campos frecuentemente filtrados en consultas y utiliza `COMMENT` para documentar columnas complejas directamente en la estructura.

```
CREATE TABLE clientes (
    cliente_id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    apellido VARCHAR(100) NOT NULL,
    biografia TEXT,
    
    INDEX idx_busqueda_apellido (apellido),
    FULLTEXT ft_biografia (biografia)
) ENGINE=InnoDB COMMENT='Tabla principal de gestión de clientes';
```

### 7 Verificación y Prueba de Ejecución

Valida que el script completo se ejecute en el orden correcto sin inconsistencias de referencias cruzadas.

#### Estructura Final Recomendada del Script

1. Creación y selección de la base de datos (`CREATE DATABASE`, `USE`).
2. Creación de tablas independientes/padre (sin FKs).
3. Creación de tablas dependientes/hijas (con FKs).
4. Creación de tablas de unión/intermedias (N:M).
