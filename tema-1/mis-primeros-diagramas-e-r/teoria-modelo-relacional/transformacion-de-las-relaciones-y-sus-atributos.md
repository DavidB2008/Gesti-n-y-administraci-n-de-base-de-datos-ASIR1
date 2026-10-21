# Transformación de las Relaciones y sus Atributos

Dependiendo del tipo de relación y de la cardinalidad que tenga, existen diversas maneras de transformarlas:

#### Relaciones N:M

Se crea una nueva tabla que incluye los&#x20;atributos de la relación (si los tiene) y\
las claves primarias de las dos&#x20;entidades como foráneas, que forman&#x20;la clave primaria de la nueva tabla como&#x20;clave compuesta.\
Estamos gestionando por tanto una&#x20;tabla dónde lo más improtante que se\
registra es la relación entre registros de&#x20;las otras dos tablas.

#### Relaciones 1:N

Estas relaciones se pueden transformar&#x20;de dos maneras:

Propagar la clave primaria de la&#x20;entidad con cardinalidad máxima 1&#x20;a la que tiene cardinalidad máxima&#x20;N (la débil), haciendo desaparecer la&#x20;tabla de la relación.&#x20;

El atributo clave&#x20;primaria de la entidad con&#x20;cardinalidad 1 se añade como&#x20;columna y clave ajena en la tabla de&#x20;la entidad con cardinalidad N.&#x20;

Si la&#x20;relación tiene atributos, estos pasan&#x20;a la tabla con cardinalidad N.

Transformarla en una nueva tabla&#x20;como si fuese N:M.&#x20;

Recomendable&#x20;cuando hay muchos nulos&#x20;posibles, se prevé que pasará a&#x20;N:M en el futuro, o la relación tiene&#x20;atributos propios.

#### Relaciones 1:1

Caso particular de los anteriores.

Cardinalidades (0,1)-(0,1): mejor&#x20;crear una tabla para evitar nulos.<br>

Cardinalidades (0,1)-(1,1): propagar&#x20;la clave de la entidad (1,1) a la (0,1).<br>

Cardinalidades (1,1)-(1,1): la&#x20;propagación es indiferente; decidir&#x20;según frecuencia de acceso.<br>

TIP > La tabla más "débil" es&#x20;normalmente la que recibirá la clave&#x20;foránea ya que es la que tiene una&#x20;dependencia semántica.
