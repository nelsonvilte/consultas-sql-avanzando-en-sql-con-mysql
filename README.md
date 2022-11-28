# Consultas SQL: Avanzando en SQL con MySQL

**Ejemplos de informes**

- Colocamos en práctica nuestro conocimiento generando dos informes conforme a lo especificado por la gerencia de la empresa de jugo de frutas.

**Realizando una consulta al informe**

En esta aula construimos un informe que presentó a los clientes que tenían ventas inválidas. Complementa este informe listando solamente a los que tuvieron ventas inválidas y calcula la diferencia entre el límite de venta máximo y la cantidad vendida en porcentuales.

Tips:

- Utiliza el comando SQL empleado al final del video.

- Filtra solamente las líneas donde: (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) < 0

- Lista el campo X.CANTIDAD_LIMITE

- Crea nuevo campo ejecutando la fórmula: (1 - (X.QUANTIDADE_LIMITE/X.QUANTIDADE_VENDAS)) * 100.



````sql
SELECT A.DNI, A.NOMBRE, A.MES_AÑO, 
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
   WHEN  (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta Válida'
   ELSE 'Venta Inválida'
END AS STATUS_VENTA, ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) AS PORCENTAJE
 FROM(
SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y") AS MES_AÑO, 
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA, 
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA  
FROM facturas F 
INNER JOIN 
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN 
tabla_de_clientes TC
ON TC.DNI = F.DNI
GROUP BY
F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y"))A
WHERE (A.CANTIDAD_MAXIMA - A.CANTIDAD_VENDIDA) < 0;

````

**Realizando una nueva consulta al informe**

En esta aula construimos un informe que presentó a los clientes que tenían ventas inválidas. Ahora lista solamente a los que tuvieron ventas inválidas en el año 2018 excediendo más del 50% de su límite permitido por mes. Calcula la diferencia entre el límite de venta máximo y la cantidad vendida, en porcentuales.

Tips:

- Te puedes apoyar en el código que realizaste para el desafío anterior.


````sql
SELECT A.DNI, A.NOMBRE, A.MES_AÑO, 
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
   WHEN  (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta Válida'
   ELSE 'Venta Inválida'
END AS STATUS_VENTA, ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) AS PORCENTAJE
 FROM(
SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y") AS MES_AÑO, 
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA, 
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA  
FROM facturas F 
INNER JOIN 
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN 
tabla_de_clientes TC
ON TC.DNI = F.DNI
GROUP BY
F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y"))A
WHERE (A.CANTIDAD_MAXIMA - A.CANTIDAD_VENDIDA) < 0 AND ROUND((1 - (A.CANTIDAD_MAXIMA/A.CANTIDAD_VENDIDA)) * 100,2) > 50
AND A.MES_AÑO LIKE "%2018";

````


**Ventas porcentuales por tamaño**

Modifica el informe pero ahora para ver el ranking de las ventas por tamaño.

Tips:

- Puede parecer difícil pero este es el ejercicio más fácil de resolver.

Lo único que debes hacer es tomar el informe, y en vez de utilizar el sabor como parámetro, utilizas tamaño. El restante de la consulta permanece igual:

````sql
SELECT VENTAS_TAMANO.TAMANO, VENTAS_TAMANO.AÑO, VENTAS_TAMANO.CANTIDAD_TOTAL,
ROUND((VENTAS_TAMANO.CANTIDAD_TOTAL/VENTA_TOTAL.CANTIDAD_TOTAL)*100,2) 
AS PORCENTAJE FROM (
SELECT P.TAMANO, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
YEAR(F.FECHA_VENTA) AS AÑO FROM
tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON F.NUMERO = IFa.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY P.TAMANO, YEAR(F.FECHA_VENTA)
ORDER BY SUM(IFa.CANTIDAD) DESC) VENTAS_TAMANO
INNER JOIN (
SELECT SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
YEAR(F.FECHA_VENTA) AS AÑO FROM
tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON F.NUMERO = IFa.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY YEAR(F.FECHA_VENTA)) VENTA_TOTAL
ON VENTA_TOTAL.AÑO = VENTAS_TAMANO.AÑO
ORDER BY VENTAS_TAMANO.CANTIDAD_TOTAL DESC;

````

**Pasos para realizar en esta aula:**

1) Vamos a poner en práctica nuestro conocimiento.

2) Primero, generamos una selección que determina si las ventas mensuales por cliente son válidas o no. Consideramos válidas las ventas por debajo de la cantidad límite e inválidas por encima de la cantidad límite existente en el registro del cliente. La consulta se muestra a continuación:

````sql

SELECT A.DNI, A.NOMBRE, A.MES_AÑO, 
A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA AS DIFERENCIA,
CASE
   WHEN  (A.CANTIDAD_VENDIDA - A.CANTIDAD_MAXIMA) <= 0 THEN 'Venta Válida'
   ELSE 'Venta Inválida'
END AS STATUS_VENTA
 FROM(
SELECT F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y") AS MES_AÑO, 
SUM(IFa.CANTIDAD) AS CANTIDAD_VENDIDA, 
MAX(VOLUMEN_DE_COMPRA)/10 AS CANTIDAD_MAXIMA  
FROM facturas F 
INNER JOIN 
items_facturas IFa
ON F.NUMERO = IFa.NUMERO
INNER JOIN 
tabla_de_clientes TC
ON TC.DNI = F.DNI
GROUP BY
F.DNI, TC.NOMBRE, DATE_FORMAT(F.FECHA_VENTA, "%m - %Y"))A;

````

3) Otro ejemplo de informe es el que determina la venta por sabores, para el año de 2016, presentando el porcentaje de participación de cada uno de estos sabores, ordenados.



````sql

SELECT VENTAS_SABOR.SABOR, VENTAS_SABOR.AÑO, VENTAS_SABOR.CANTIDAD_TOTAL,
ROUND((VENTAS_SABOR.CANTIDAD_TOTAL/VENTA_TOTAL.CANTIDAD_TOTAL)*100,2) 
AS PORCENTAJE FROM (
SELECT P.SABOR, SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
YEAR(F.FECHA_VENTA) AS AÑO FROM
tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON F.NUMERO = IFa.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY P.SABOR, YEAR(F.FECHA_VENTA)
ORDER BY SUM(IFa.CANTIDAD) DESC) VENTAS_SABOR
INNER JOIN (
SELECT SUM(IFa.CANTIDAD) AS CANTIDAD_TOTAL, 
YEAR(F.FECHA_VENTA) AS AÑO FROM
tabla_de_productos P
INNER JOIN
items_facturas IFa
ON P.CODIGO_DEL_PRODUCTO = IFa.CODIGO_DEL_PRODUCTO
INNER JOIN
facturas F
ON F.NUMERO = IFa.NUMERO
WHERE YEAR(F.FECHA_VENTA) = 2016
GROUP BY YEAR(F.FECHA_VENTA)) VENTA_TOTAL
ON VENTA_TOTAL.AÑO = VENTAS_SABOR.AÑO
ORDER BY VENTAS_SABOR.CANTIDAD_TOTAL DESC;

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