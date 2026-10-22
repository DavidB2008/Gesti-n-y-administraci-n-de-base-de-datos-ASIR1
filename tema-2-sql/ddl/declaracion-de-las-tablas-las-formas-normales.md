# Declaración de las tablas: Las Formas Normales

## Declaración de las tablas: Las Formas Normales

## El arte de la normalización

**Diseñar bases de datos relacionales robustas: de la 1FN a la 3FN.**

La normalización es el proceso de organizar los datos en tablas relacionadas para reducir redundancias, proteger la integridad y evitar anomalías al insertar, modificar o borrar información.

> **Idea clave:** un lugar para cada dato y cada dato en su único lugar.

### 1. El coste del caos: la «tabla monstruo»

Una tabla que mezcla información de clientes, vehículos, pólizas y datos técnicos puede parecer cómoda al principio, pero acaba provocando problemas.

#### Anomalías de inserción

No podemos guardar un nuevo modelo de coche si antes no lo asociamos a un cliente. Esto obliga a introducir datos que no deberían ser necesarios.

#### Anomalías de modificación

Si Ford cambia de nombre, habría que actualizar todos los registros donde aparece la marca. Si se olvida una fila, la base de datos queda con información contradictoria.

#### Anomalías de borrado

Si un cliente cancela su seguro y borramos su fila, también podemos perder los datos técnicos del vehículo que estaban guardados únicamente en esa fila.

**Conclusión:** mezclar entidades distintas en una sola tabla genera duplicación, dependencias innecesarias y pérdida accidental de información.

### 2. ¿Qué es la normalización?

Es una forma de estructurar las tablas de una base de datos relacional para reducir redundancias y evitar anomalías. Los datos se separan en tablas que representan conceptos distintos y se conectan mediante claves.

Los pasos principales que presenta el documento son la **Primera Forma Normal (1FN)**, la **Segunda Forma Normal (2FN)** y la **Tercera Forma Normal (3FN)**.

### 3. El concepto clave: dependencia funcional

Una **dependencia funcional** es una relación entre atributos en la que conocer el valor de uno permite determinar el valor de otro.

Se representa conceptualmente como **A → B**: el atributo A determina el atributo B.

* **Determinante:** atributo o conjunto de atributos que permite conocer otro valor.
* **Ejemplo del documento:** si conocemos el DNI de una persona, podemos determinar su nombre. El DNI actúa como determinante.
* **Analogía:** una matrícula identifica un vehículo concreto y permite asociarle datos como el modelo o el color en un contexto determinado.

Las dependencias funcionales ayudan a decidir qué atributos pertenecen a cada tabla y de qué clave deben depender.

### 4. Primera Forma Normal (1FN): atomicidad

#### El problema

Una celda no debe contener varios valores del mismo tipo, por ejemplo, una lista de matrículas como `Z-2345, B-2456` dentro de una sola columna.

Tampoco conviene crear columnas repetidas como `Atributo1`, `Atributo2` y `Atributo3` para almacenar elementos de una lista.

#### La solución y la regla

* La tabla debe contener valores **atómicos**: un único valor indivisible en cada celda.
* No debe haber grupos repetidos de valores dentro de una misma fila.
* Cada matrícula, teléfono u otro elemento repetible debe representarse de manera que pueda consultarse individualmente, usando filas o una tabla relacionada según el modelo.

#### Beneficio

Se pueden buscar, filtrar y contar valores individuales mediante consultas SQL sin tener que separar cadenas de texto manualmente.

**Pregunta de comprobación:** ¿hay listas separadas por comas en alguna celda? Si la respuesta es sí, revisa si esos valores deben representarse como registros separados.

### 5. Segunda Forma Normal (2FN): dependencia total de la clave

La 2FN requiere que la tabla ya cumpla la 1FN.

#### ¿Qué problema evita?

Evita las **dependencias parciales**: ocurre una dependencia parcial cuando un atributo que no forma parte de la clave depende solo de una parte de una clave primaria compuesta, y no de la clave completa.

#### Ejemplo conceptual del documento

En una tabla cuya clave compuesta combina `ID_Póliza` y `Matrícula`, un dato como la ciudad asociada a la póliza no debe depender únicamente de la matrícula si en realidad describe la póliza. Ese dato debe colocarse en la tabla de la entidad de la que depende.

#### Regla de la 2FN

* La tabla debe estar en 1FN.
* Si la clave está formada por varios atributos, cada atributo que no sea clave debe depender de **toda la clave**, no solo de una parte.
* Si un atributo depende solo de una parte, se separa en la tabla adecuada y se relaciona mediante claves.

#### Pregunta de comprobación

¿La tabla tiene una clave formada por dos o más atributos? Si es así, comprueba si algún dato tiene sentido dependiendo únicamente de la mitad de esa clave.

### 6. Tercera Forma Normal (3FN): relación directa

La 3FN requiere que la tabla esté en 2FN y elimina las **dependencias transitivas**.

#### ¿Qué es una dependencia transitiva?

Se produce cuando un atributo que no es clave depende de otro atributo que tampoco es la clave principal, en lugar de depender directamente de la clave.

#### Ejemplo conceptual

Si una tabla de vehículos guarda el identificador del modelo y, además, repite datos del fabricante que se determinan a partir del modelo, esos datos del fabricante no deberían repetirse en cada vehículo. Los modelos y sus fabricantes pueden almacenarse en una tabla propia y enlazarse mediante una clave.

Otro ejemplo del documento es:

`Película → Director → Nacionalidad del director`

Si la nacionalidad se determina a partir del director, existe una dependencia entre atributos no clave. Conviene almacenar la información del director como entidad independiente y referenciarla con una clave foránea.

#### Regla de la 3FN

* La tabla debe estar en 2FN.
* Los atributos que no son clave deben depender de la clave, y no de otros atributos no clave.
* Se extraen las entidades ocultas en la tabla y se conectan mediante claves primarias y foráneas.

### 7. Mapa de transformación: del caos al modelo relacional

El documento muestra cómo una tabla que mezclaba clientes, pólizas y vehículos puede dividirse en varias tablas relacionadas. La idea es conservar la información original sin duplicarla innecesariamente.

#### Clientes

Puede contener datos propios de cada cliente, por ejemplo:

* **DNI** (identificador del cliente).
* Nombre.
* Dirección.
* Teléfono.

#### Pólizas

Puede contener los datos propios del seguro:

* **ID\_Póliza** (identificador de la póliza).
* DNI del cliente como referencia o clave foránea.
* Fecha de alta.
* Importe.

#### Vehículos

Puede contener los datos propios de cada vehículo:

* Matrícula.
* ID del modelo como clave foránea.
* Color u otros datos propios del vehículo.

#### Modelos

Puede contener los datos propios del modelo:

* **ID\_Modelo**.
* Marca.
* Nombre del modelo.
* Tipo de coche.
* Motor.

La estructura exacta de las columnas debe respetar las dependencias reales del caso. El principio del esquema es que cada tabla guarde los datos de su entidad y que las relaciones se establezcan mediante claves.

#### ¿Qué se consigue?

* Se reducen las redundancias.
* Se evitan anomalías de inserción, modificación y borrado.
* Se conserva la información mediante una descomposición sin pérdida.
* Cada entidad puede actualizarse sin tener que modificar copias repetidas en otras filas.

### 8. Matriz de diagnóstico de las formas normales

| Forma normal | Problema que elimina                              | Regla para recordar                                                         |
| ------------ | ------------------------------------------------- | --------------------------------------------------------------------------- |
| **1FN**      | Valores múltiples en una celda y grupos repetidos | Un valor atómico por celda                                                  |
| **2FN**      | Dependencias parciales de una clave compuesta     | Cada atributo no clave depende de toda la clave                             |
| **3FN**      | Dependencias transitivas                          | Los atributos no clave dependen de la clave, no de otros atributos no clave |

### 9. Integridad frente a rendimiento: normalización y desnormalización

El documento distingue dos contextos de trabajo.

#### Máxima normalización (OLTP)

Adecuada para sistemas transaccionales, como sistemas financieros o comercio electrónico:

* Reduce la redundancia.
* Favorece la integridad y las operaciones seguras.
* Puede requerir más operaciones `JOIN` para reunir datos de distintas tablas.

#### Desnormalización controlada (Data Warehouse / OLAP)

Puede usarse en entornos de análisis y almacenes de datos cuando interesa acelerar lecturas y reportes:

* Acepta cierta redundancia controlada.
* Puede reducir el número de tablas que hay que unir.
* Debe aplicarse conscientemente, porque aumenta la responsabilidad de mantener los datos coherentes.

**Idea del documento:** la 3FN es un estándar importante para bases de datos transaccionales. Ir a formas superiores, como 4FN o 5FN, no es automáticamente mejor en todos los casos: una fragmentación excesiva puede hacer más complejas las consultas. La decisión depende de las dependencias de los datos y de los requisitos reales de rendimiento e integridad.

### 10. Auditoría: comprobar 1FN y 2FN

#### Lista de control de 1FN: escáner de atomicidad

* [ ] ¿Hay listas separadas por comas dentro de alguna celda?
* [ ] ¿Se han creado columnas numeradas como `Atributo1`, `Atributo2` y `Atributo3` para guardar elementos repetidos?
* [ ] ¿Puedo consultar cada valor de forma individual sin analizar cadenas de texto?

Si hay listas o columnas repetidas, estudia la posibilidad de crear filas o una tabla relacionada.

#### Lista de control de 2FN: escáner de clave compuesta

* [ ] ¿Mi tabla tiene una clave formada por dos o más atributos?
* [ ] ¿Existe algún atributo que solo necesite una parte de esa clave para tener sentido?
* [ ] ¿He separado esos atributos en la tabla a la que realmente pertenecen?

Si un atributo depende de solo una parte de una clave compuesta, revisa la estructura para cumplir la 2FN.

### 11. Auditoría: la prueba de fuego de la 3FN

El documento propone el ejemplo:

`Película → Director → Nacionalidad_Director`

Hazte esta pregunta:

**Si cambio el valor de un atributo que no es clave (por ejemplo, el director), ¿eso me obliga lógicamente a cambiar otro atributo de esta misma tabla (por ejemplo, la nacionalidad del director)?**

Si la respuesta es sí, puede haber una dependencia transitiva: estás guardando en una tabla información que pertenece a otra entidad. Extrae esa entidad y conéctala mediante una clave foránea.

* [ ] ¿Los atributos no clave dependen directamente de la clave?
* [ ] ¿Hay algún atributo no clave que determine otro atributo no clave?
* [ ] ¿He separado las entidades independientes y definido las claves foráneas necesarias?

### 12. Resumen para estudiar

* **1FN:** cada celda contiene un solo valor atómico; no hay listas ni grupos repetidos.
* **2FN:** se cumple 1FN y ningún atributo no clave depende solo de una parte de una clave compuesta.
* **3FN:** se cumple 2FN y no hay dependencias transitivas de atributos no clave.
* **Dependencia funcional:** un atributo determina el valor de otro.
* **Anomalías:** problemas al insertar, modificar o borrar debidos a redundancias o a una mala estructura.
* **Descomposición sin pérdida:** separar una tabla en otras relacionadas sin perder la información original.
* **Clave primaria:** identifica cada registro de una tabla.
* **Clave foránea:** permite relacionar una tabla con otra.
* **Normalización:** prioriza la coherencia y reduce duplicaciones; la desnormalización puede aceptarse de forma controlada para ciertos requisitos de lectura.

### 13. El juramento del diseñador

> «Todo atributo en esta tabla debe depender de la clave, de toda la clave y de nada más que de la clave».

Esta frase resume la idea central que conecta las tres primeras formas normales: los atributos deben colocarse en la tabla correcta y depender de la clave que realmente los identifica.

***

**Nota sobre el material original:** el PDF contiene 15 diapositivas con esquemas visuales, tablas ilustrativas y listas de auditoría. Esta página recoge el texto legible y explica los diagramas en formato de apuntes para estudiar; los gráficos originales no se han insertado como imágenes independientes.
