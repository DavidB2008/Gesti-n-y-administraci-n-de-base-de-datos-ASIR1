# Transformación de los Atributos de Relaciones —> Ejemplo Universidad

Si la relación se transforma en una tabla, todos sus atributos pasan a ser columnas de la tabla.&#x20;

En el caso en que alguno de los atributos de la&#x20;relación sea clave primaria, deberá ser incluido como parte de la clave primaria en dicha tabla.

En el ejemplo de la Universidad, tenemos una relación 1:N y otra N:M:<br>

De la relación 1:N: propagación de clave → la clave primaria de Grado (cardinalidad máxima 1) pasa como atributo y clave ajena&#x20;a la tabla Asignatura.<br>

De la relación N:M: se obtiene una nueva tabla con el atributo Nota y la clave primaria formada por la unión de las claves&#x20;primarias de Asignatura (Código) y Alumno (DNI).

#### 01 Identificar entidades

Convertir cada una en una tabla.

#### 02 Identificar relaciones N:M

Crear una nueva tabla por cada una.

#### 03 Identificar relaciones 1:N

Propagar la clave primaria (o crear tabla si hay&#x20;nulos/atributos propios).

#### 04 Identificar relaciones 1:1

Aplicar la regla según cardinalidades.

#### 05 Transformar atributos de relaciones

Añadirlos como columnas a la tabla correspondiente.
