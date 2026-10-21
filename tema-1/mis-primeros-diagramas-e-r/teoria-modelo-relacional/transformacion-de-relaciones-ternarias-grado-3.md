# Transformación de Relaciones Ternarias (Grado 3)

#### Relaciones muchos a muchos a muchos (N:M:P)

Este tipo de relación se transforma en una tabla cuya clave primaria es la&#x20;concatenación de las claves primarias de las tablas surgidas al&#x20;transformar las entidades que forman parte de la relación.&#x20;

Junto a estos&#x20;atributos se incluyen los atributos propios de la relación. Cada uno de los&#x20;atributos que forman la clave primaria de esta tabla son a la vez claves\
ajenas respecto a cada una de las tablas donde dicho atributo es clave&#x20;primaria.&#x20;

En definitiva, es la misma situación que en una BINARIA.

#### Relaciones muchos a muchos a uno (N:M:1)

Este tipo de relación se transforma en una tabla cuya clave primaria es la&#x20;concatenación de las claves primarias de las tablas que corresponden a la\
cardinalidad N y M.&#x20;

Junto a estos atributos se incluyen los atributos&#x20;propios de la relación más la clave primaria de la tabla que corresponde a&#x20;la cardinalidad 1.&#x20;

Cada uno de los atributos que forman la clave primaria&#x20;de esta tabla y los atributos añadidos de la relación de cardinalidad 1 son&#x20;claves ajenas respecto a cada una de las tablas donde dicho atributo es&#x20;clave primaria.
