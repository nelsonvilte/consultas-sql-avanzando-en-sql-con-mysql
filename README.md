# Consultas SQL: Avanzando en SQL con MySQL

**Funciones de MySQL**

- Algunas funciones de tipo STRING para texto;
- Funciones matemáticas;
- Funciones de tipo DATE para fechas;
- Abordamos funciones de conversión.


**Funciones con STRINGS**

Listando la dirección completa: Consulta listando el nombre del cliente y la dirección completa (Con calle, barrio, ciudad y estado).

````sql

SELECT NOMBRE, CONCAT(DIRECCION_1, ' ', BARRIO, ' ', CIUDAD, ' ', ESTADO) AS COMPLETO FROM tabla_de_clientes;

````


**Funciones de fecha**

Edad de los clientes: Consulta que muestre el nombre y la edad actual del cliente.

````sql

SELECT NOMBRE, TIMESTAMPDIFF(YEAR, FECHA_DE_NACIMIENTO, CURDATE()) AS    EDAD
FROM  tabla_de_clientes;

````

**Funciones matemáticas**
 
 Formato de facturación: En la tabla de facturas tenemos el valor del impuesto. En la tabla de ítems tenemos la cantidad y la facturación. Calcula el valor del impuesto pago en el año de 2016 redondeando al menor entero.

````sql

SELECT YEAR(FECHA_VENTA), FLOOR(SUM(IMPUESTO * (CANTIDAD * PRECIO))) 
AS RESULTADO
FROM facturas F
INNER JOIN items_facturas IFa ON F.NUMERO = IFa.NUMERO
WHERE YEAR(FECHA_VENTA) = 2016
GROUP BY YEAR(FECHA_VENTA);

````

**Convirtiendo datos** 

Listando con expresión natural: Construir un SQL cuyo resultado sea, para cada cliente:

- “El cliente Pepito Pérez facturó 120000 en el año 2016”.

Solamente para el año 2016.

````sql

SELECT CONCAT('El cliente ', TC.NOMBRE, ' facturó ', 
CONVERT(SUM(IFa.CANTIDAD * IFa.precio), CHAR(20))
 , ' en el año ', CONVERT(YEAR(F.FECHA_VENTA), CHAR(20))) AS FRASE FROM facturas F
INNER JOIN items_facturas IFa ON F.NUMERO = IFa.NUMERO
INNER JOIN tabla_de_clientes TC ON F.DNI = TC.DNI
WHERE YEAR(FECHA_VENTA) = 2016
GROUP BY TC.NOMBRE, YEAR(FECHA_VENTA);

````

**Pasos para realizar en esta aula:**

1) En esta aula veremos ejemplos de funciones.

2) Primero vimos las funciones de tipo texto. Observa algunos ejemplos con sus respectivos outputs:


````sql
SELECT LTRIM("    MySQL es fácil") AS RESULTADO;

````




````sql

SELECT RTRIM("MySQL es fácil    ") AS RESULTADO;

````

````sql

SELECT TRIM("    MySQL es fácil    ") AS RESULTADO;

````

````sql

SELECT CONCAT("MySQL es fácil,", " no lo crees?") AS RESULTADO;

````

````sql

SELECT UPPER("mysql es una base de datos interesante.") AS RESULTADO;

````

````sql

SELECT LOWER("MYSQL ES UNA BASE DE DATOS INTERESANTE.") AS RESULTADO;

````

````sql

SELECT SUBSTRING("mysql es una base de datos interesante.", 14,4) AS RESULTADO;

````

````sql

SELECT CONCAT(NOMBRE, " ", DNI) AS RESULTADO FROM tabla_de_clientes;

````

3) Tenemos las funciones de fechas. Ejecuta los siguientes comandos:

````sql
SELECT CURDATE();

````

````sql

SELECT CURRENT_TIMESTAMP();

````

````sql

SELECT YEAR(CURRENT_TIMESTAMP());

````

````sql

SELECT MONTH(CURRENT_TIMESTAMP());
COPIA EL CÓDIGO

````

````sql

SELECT DAY(CURRENT_TIMESTAMP());

````

````sql

SELECT MONTHNAME(CURRENT_TIMESTAMP());

````

````sql

SELECT DAYNAME(CURRENT_TIMESTAMP());

````

````sql

SELECT DATEDIFF(CURRENT_TIMESTAMP(), '2021-01-01') AS RESULTADO;

````

````sql

SELECT DATEDIFF(CURRENT_TIMESTAMP(), '1984-06-20') AS RESULTADO;

````

````sql

SELECT current_timestamp() AS DIA_HOY, 
DATE_SUB(current_timestamp(), INTERVAL 1 MONTH) AS RESULTADO;

````

````sql

SELECT DISTINCT FECHA_VENTA,
DAYNAME(FECHA_VENTA) AS DIA, MONTHNAME(FECHA_VENTA) AS MES, 
YEAR(FECHA_VENTA) AS AÑO FROM facturas;

````


4) Tenemos algunos ejemplos de funciones matemáticas:


````sql

SELECT (34+346-67)/15 * 29 AS RESULTADO;


````


````sql

SELECT CEILING (23.1222);


````


````sql

SELECT FLOOR (23.999999);

````


````sql
SELECT RAND() AS RESULTADO;

````


````sql
SELECT ROUND(254.545,2);

````


````sql

SELECT ROUND(254.545,1);

````


````sql

SELECT NUMERO, CANTIDAD, PRECIO, 
CANTIDAD * PRECIO AS FACTURACIÓN FROM items_facturas;

````


````sql
SELECT NUMERO, CANTIDAD, PRECIO, 
ROUND(CANTIDAD * PRECIO,2) AS FACTURACIÓN FROM items_facturas;


````


5) Tenemos también funciones de conversión. Ejecuta los siguientes ejemplos:


````sql

SELECT CURRENT_TIMESTAMP() AS RESULTADO;

````

````sql

SELECT CONCAT("La fecha y la hora de hoy son: ", CURRENT_TIMESTAMP()) AS RESULTADO;

````

````sql

SELECT CONCAT("La fecha y el horario son: ",
DATE_FORMAT(CURRENT_TIMESTAMP(),"%W, %d/%m/%Y a las %T" )) AS RESULTADO;

````

````sql

SELECT CONVERT(23.45, CHAR) AS RESULTADO;

````

````sql
SELECT SUBSTRING(CONVERT(23.45, CHAR),3,1) AS RESULTADO;

````