# Consultas SQL: Avanzando en SQL con MySQL

**Filtrando las consultas de los datos**

- La importancia de conocer la base de datos antes de realizar las consultas;
- Comando de consultas y cómo crear filtros;
- Mezclar filtros condicionales con AND y OR;
- Utilizar >, >=, <, <=, = ou <> en los filtros que implican valores;
- Funcionamiento del comando LIKE.

**Pasos para realizar en esta aula:

1) Para que las consultas en la base de datos puedan ser efectuadas, es preciso conocer sus tablas y sus relaciones. Para ello, dirígete a Workbench y verifica si la base de datos jugos_ventas está disponible.

2) Expandiendo el árbol de estructura de base de datos sobre jugos_ventas, podemos ver los componentes de una base de datos. Para las consultas, uno de los elementos más importantes son las tablas que pueden ser vistas en mayor detalle hasta su estructura de campos.

3) Dirígete al menú y selecciona Database / Reverse Engineer.

4) Haz Clic en Next dos veces y después escoge la base en la cual la ingeniería reversa será efectuada.

5) Continúa en el asistente confirmando las selecciones por defecto hasta el final.

6) Podrás ver un esquema visual de las tablas. Este esquema puede servir como una guía para tus consultas.

7) Conociendo cómo es nuestra base, podemos hacer nuestras consultas. Selecciona un nuevo script de SQL, con la base de datos seleccionada, y digita:

````sql

     USE jugos_ventas;

     SELECT DNI, NOMBRE, DIRECCION_1, DIRECCION_2, BARRIO, CIUDAD, ESTADO, CP, 
     FECHA_DE_NACIMIENTO, EDAD, SEXO, LIMITE_DE_CREDITO, VOLUMEN_DE_COMPRA, PRIMERA_COMPRA 
     FROM tabla_de_clientes;
````     

Aquí veremos todos los campos de la tabla tabla_de_clientes. Esto porque los campos fueron seleccionados uno a uno.

8) Digita a continuación:
````sql
SELECT * FROM tabla_de_clientes;
````
Este resultado fue igual al de la consulta anterior. Ello porque al colocar * estamos seleccionando todos los campos.

9) Digita:

````sql
SELECT DNI, NOMBRE FROM tabla_de_clientes;
````
Ahora podemos ver que no es necesario seleccionar todos los campos de una tabla. Basta destacar los campos que serán vistos.

10) Digita:

````sql
SELECT DNI AS IDENTIFICACION, NOMBRE AS CLIENTE FROM tabla_de_clientes;
````

No siempre el nombre original de la columna es el nombre que queremos que sea retornado por la consulta. Por ello, podemos crear Alias (Sobrenombres) para los campos escribiendo algo después del comando AS.

11) Podemos filtrar nuestra consulta. Digita:

````sql

SELECT * FROM tabla_de_productos WHERE  SABOR = 'Uva';

SELECT * FROM tabla_de_productos WHERE  SABOR = 'Mango';

SELECT * FROM tabla_de_productos WHERE  ENVASE = 'Botella PET';

````

El resultado es el mismo si se escribe en mayúscula o en minúscula ya que MySQL no es case sensitive:

````sql

SELECT * FROM tabla_de_productos WHERE  ENVASE = 'botella pet';

````

Los filtros usados retornan todos los registros de la tabla donde se cumple lo especificado por la cláusula WHERE. Podemos usar cualquier columna como criterio.

12) Existen comandos de filtro aplicados a valores:

````sql

SELECT * FROM tabla_de_productos WHERE PRECIO_DE_LISTA > 16;


SELECT * FROM tabla_de_productos WHERE PRECIO_DE_LISTA BETWEEN 16 AND 16.02;

````

En este caso podemos usar >, >=, <, <=, =, <> y BETWEEN. Así, podemos aplicar filtros sobre los valores para obtener diversos resultados.

13) Es posible aplicar consultas condicionales usando los operadores AND y OR. El output va a depender del significado de AND y OR en una estructura lógica. Digita:

````sql
SELECT * FROM tabla_de_productos WHERE SABOR='mango' AND TAMANO='470 ml';

````

A causa del operador AND, el output solamente ocurrirá cuando las dos condiciones se cumplan en el mismo registro de la tabla.

14) Digita:

````sql

SELECT * FROM tabla_de_productos WHERE SABOR='mango' OR TAMANO='470 ml';

````

Aquí obtuvimos un filtro (Sabor = Mango) u otro ( Tamaño = 470 ml). Esto porque usamos el operador OR.

15) Podemos usar parte de un texto para ser empleado como criterio de localización de registros de la tabla. Digita a continuación:

````sql

SELECT * FROM tabla_de_productos WHERE SABOR LIKE '%manzana';

````

Aquí buscaremos todos los registros cuyo sabor contenga la palabra Manzana, pero solamente al final del texto, ya que el signo % precede el texto Manzana.

16) Podemos mezclar condiciones LIKE con otras. Digita:


````sql
SELECT * FROM tabla_de_productos
WHERE SABOR LIKE '%manzana' 
AND ENVASE = 'Botella PET';
````
Finalmente, obtuvimos el resultado de la consulta del texto “Manzana” tan solo para envases de tipo “Botella PET”.



 **El estándar SQL es bueno para aplicaciones que van a funcionar a largo plazo por que:**

 - Con el estándar SQL, las aplicaciones pueden ser fácilmente transportadas de una base de datos de un fabricante a otro.


**Pasos  para realizar en esta clase**

1) Instala MySQL, conforme al video Instalando MySQL Server, del aula 1 del curso de Introducción a SQL con MySQL.

2) Abre MySQL Workbench. Utiliza la conexión LOCALHOST.

3) Haz clic con el Botón derecho del mouse sobre la lista de las bases de datos y escoge Create Schema...

4) Digita el nombre jugos_ventas. Haz clic en Apply dos veces.

5) Haz Download del archivo RecuperacionAmbiente.zip.

6) Descompacta el archivo.

7) Selecciona la pestaña Administration en el área Navigator.

8) Selecciona el link Data Import/Restore.

9) En la opción Import From Dump Project Folder escoge el directorio DumpJugosVentas.

10) Selecciona Start Import.

11) Verifica en la base de datos jugos_ventas si las tablas fueron creadas.

12) Existe otra manera de importar los archivos (Solo en caso de que el método anterior no funcione). Basta ejecutar todos los archivos .sql compactados en el archivo RecuperacionAmbiente.zip y listo.

