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

#### Integridad Referencial y Relaciones&#xD; entre Tablas

#### La clave ajena: el&#xD; vínculo entre&#xD; relaciones

La clave foránea y la clave primaria de la tabla referenciada han de estar\
definidas sobre los mismos dominios. Este mecanismo es el que permite\
establecer vínculos consistentes entre tablas, garantizando la integridad\
referencial del modelo relacional.

<figure><img src="../../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Una tabla puede poseer más de una clave ajena,&#x20;una por cada tabla referenciada de la que dependa.

Una tabla puede no tener ninguna clave ajena si no&#x20;depende de ninguna otra relación.

Una clave ajena puede relacionar una tabla consigo&#x20;misma en las denominadas relaciones reflexivas.
