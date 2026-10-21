# Propiedades de las Relaciones

Las tablas del modelo relacional no son colecciones arbitrarias de datos: cumplen propiedades matemáticas precisas derivadas de la teoría&#x20;de conjuntos que garantizan su integridad y consistencia.

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

#### Sin Filas Repetidas

El cuerpo de la relación es un conjunto&#x20;matemático. Por definición, los conjuntos no&#x20;incluyen elementos repetidos: dos registros&#x20;de una misma relación deben diferir, al menos,&#x20;en el valor de un campo.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

#### Sin Orden Definido

Ni las filas ni los atributos están ordenados.\
Las filas de una relación no tienen orden (a&#x20;diferencia de una tabla visual), y los atributos&#x20;tampoco, ya que la cabecera se define&#x20;también como conjunto.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

#### Valores Atómicos

Todos los valores de los atributos son&#x20;atómicos: un atributo solo puede tomar un\
único valor en cada fila. Esto garantiza la&#x20;indivisibilidad de cada dato almacenado en la&#x20;relación.
