# Tipos de Claves en el Modelo Relacional

#### 🔑 Clave Candidata y Primaria

La clave candidata es un conjunto mínimo y&#x20;no vacío de atributos que identifica\
unívocamente cada registro.&#x20;

La clave primaria&#x20;es la clave candidata elegida por el usuario; es&#x20;compuesta cuando está formada por más de&#x20;un atributo.

#### 🔄 Clave Alternativa

Es cualquiera de las claves candidatas que no&#x20;han sido elegidas como clave primaria.&#x20;

Por&#x20;ejemplo, en la tabla EMPLEADO, si el atributo&#x20;DNI se elige como clave primaria, el atributo&#x20;NUM\_EMPLEADO pasa a ser clave&#x20;alternativa.

#### 🔗 Clave Ajena o Foránea

Es un conjunto no vacío de atributos cuyos&#x20;valores han de coincidir con los valores de la&#x20;clave primaria de otra relación.&#x20;

Puede&#x20;relacionar incluso una tabla consigo misma&#x20;(relaciones reflexivas).&#x20;

Una tabla puede tener&#x20;más de una clave ajena.
