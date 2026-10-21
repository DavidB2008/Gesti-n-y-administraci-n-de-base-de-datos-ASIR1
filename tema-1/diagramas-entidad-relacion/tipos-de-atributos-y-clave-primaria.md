# Tipos de Atributos y Clave Primaria

#### Obligatorios y Opcionales

Un atributo obligatorio siempre debe\
estar definido, como el DNI en la entidad\
EMPLEADO, imprescindible para\
identificar cada instancia. Un atributo\
opcional puede quedar sin definir para\
algunas instancias, como la edad, que no\
es necesaria para la identificación.

#### Simples y Compuestos

Un atributo simple no se puede dividir en\
partes más pequeñas con significado\
propio (p. ej. el DNI). Un atributo\
compuesto tiene varios componentes\
con significado propio, como la dirección,\
que se descompone en calle, número y\
población, pero que en conjunto tiene un\
signficado completo añadido.

#### Clave Primaria

La clave primaria es la clave candidata\
elegida para identificar de forma única\
cada instancia de una entidad. En\
EMPLEADO puede ser el DNI o el código\
de empleado. Una vez elegida, ningún\
subconjunto de ella debe ser clave\
candidata por sí solo.

#### Clave Candidata

Se trata de uno o más atributos cuyos valores son únicos, es decir no-repetibles, para cada instancia de una entidad, pero que no son elegidos para identificar la entidad. Si tenemos la entidad EMPLEADO con los siguientes atributos: DNI, codigo\_empleado, nombre, apellidos, direccion, fecha\_nacimiento, etc... Dos claves candidatas son DNI y codigo\_empleado, ya que ambas identifican de manera única una instancia de EMPLEADO.
