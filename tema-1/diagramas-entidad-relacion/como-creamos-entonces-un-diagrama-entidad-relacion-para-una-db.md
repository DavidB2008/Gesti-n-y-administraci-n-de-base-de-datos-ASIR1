# ¿Cómo creamos entonces un Diagrama Entidad-Relación para una DB?

Para el ejemplo, se va a realizar el diagrama E/R que representa la información de una Universidad sobre los grados y sus asignaturas que se\
pueden estudiar en ella y se va a incluir, además, la información de los alumnos matriculados en las asignaturas junto con las calificaciones\
que obtienen en ellas. Para este caso se va a suponer que una asignatura solo puede pertenecer a un grado.

#### Paso 1: Identificar las&#xD; Entidades

Lo primero es buscar las entidades del&#x20;problema.&#x20;

En este caso son: Asignatura,&#x20;Grado y Alumno.

#### Paso 2: Identificar las&#xD; Relaciones

Después se buscan las relaciones entre&#x20;las entidades.&#x20;

Asignatura se relaciona&#x20;con Grado a través de la relación&#x20;"pertenece", y Alumno se relaciona con&#x20;Asignatura a través de&#x20;"esta\_matriculado".

#### Paso 3: Añadir los Atributos

Se añaden los atributos de cada entidad.\
De Grado: nombre y código. De&#x20;Asignatura: nombre, código (clave&#x20;primaria), duración y horas semanales.&#x20;

De&#x20;Alumno: DNI (clave primaria), nombre,&#x20;apellidos, fecha de nacimiento y email. La&#x20;nota es un atributo de la relación&#x20;esta\_matriculado, ya que cada alumno\
obtiene una nota distinta por asignatura.
