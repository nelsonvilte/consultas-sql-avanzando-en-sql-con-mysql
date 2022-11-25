# Consultas SQL: Avanzando en SQL con MySQL

**Uniendo tablas y consultas**

- Conectar dos o más tablas a través de comandos de JOIN;
- Tipos de JOIN existentes y cuáles son soportados por MySQL;
- Comandos UNION y UNION ALL, para unir dos o más selecciones siempre y cuando tengan los mismos campos seleccionados;
- Utilizar una consulta como criterio de filtro de otra consulta;
- Utilizar una consulta dentro de otra consulta;
- Crear y utilizar vistas (Views).

**Pasos para realizar en esta aula:**

1) Aquí veremos cómo conectar las consultas de tablas diferentes. Esta unión se conoce como JOIN.

2) Observa el contenido de dos tablas digitando los siguientes comandos:


````sql
SELECT * FROM tabla_de_vendedores;

````

````sql

SELECT * FROM facturas;

````


3) Podemos conectar estas dos tablas por un campo en común (MATRICULA). Digita:

````sql

SELECT * FROM tabla_de_vendedores A
INNER JOIN
facturas B
ON A.MATRICULA = B.MATRICULA;

````


4) Podemos aplicar agrupamientos al resultado de la consulta que conecta una o más tablas:

````sql

SELECT A.NOMBRE, B.MATRICULA, COUNT(*) FROM tabla_de_vendedores A
INNER JOIN
facturas B
ON A.MATRICULA = B.MATRICULA
GROUP BY A.NOMBRE, B.MATRICULA;

````

5) No siempre todos los registros pueden ser conectados. Existen otros tipos de JOIN que nos permiten identificar lo que no puede ser conectado. Observa la siguiente consulta:

````sql

SELECT count(*) FROM tabla_de_clientes;

````

Ella muestra que tenemos 15 clientes.

6) Vamos a realizar un JOIN con la tabla de facturas para ver cuántos clientes poseen facturas emitidas. Digita:

````sql

SELECT DISTINCT A.DNI, A.NOMBRE, B.DNI FROM tabla_de_clientes A
INNER JOIN
facturas B
ON A.DNI = B.DNI;

````


Si cuentas a los clientes verás que, en la consulta acima, tenemos 12 registros. Existen tres clientes que están registrados pero nunca se les emitió facturas.

7) Podemos usar un LEFT JOIN. Digita:

````sql

SELECT DISTINCT A.DNI, A.NOMBRE, A.CIUDAD, B.DNI FROM tabla_de_clientes A
LEFT JOIN
facturas B
ON A.DNI = B.DNI;


````


El cliente que posee el DNI proveniente de la tabla de facturas con el valor nulo, es el cliente a quien nunca se le emitió facturas.

8) La selección correcta sería:

````sql

SELECT DISTINCT A.DNI, A.NOMBRE, A.CIUDAD, B.DNI FROM tabla_de_clientes A
LEFT JOIN
facturas B
ON A.DNI = B.DNI
WHERE B.DNI IS NULL;

````

9) Podemos juntar dos o más consultas, Desde que los campos seleccionados sean los mismos. Digita:

````sql


SELECT DISTINCT BARRIO FROM tabla_de_clientes
UNION
SELECT DISTINCT BARRIO FROM tabla_de_vendedores;

````

10) El comando UNION ALL no realiza la selección con un DISTINCT. Los registros se repiten si existen en ambas tablas. Digita:

````sql

SELECT DISTINCT BARRIO FROM tabla_de_clientes
UNION ALL
SELECT DISTINCT BARRIO FROM tabla_de_vendedores;

````

Observa que Del Valle aparece dos veces, al igual que Condesa, Contadero y Oblatos. Una proveniente de la tabla de clientes y la otra proveniente de la tabla de productos.

11) Podemos simular el FULL JOIN, que no es soportado por MySQL, usando el LEFT JOIN y el RIGHT JOIN con UNION. Digita:

````sql

SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_clientes.BARRIO,
tabla_de_vendedores.NOMBRE, VACACIONES
FROM tabla_de_clientes
LEFT JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO
UNION
SELECT tabla_de_clientes.NOMBRE, tabla_de_clientes.CIUDAD, tabla_de_clientes.BARRIO,
tabla_de_vendedores.NOMBRE, VACACIONES
FROM tabla_de_clientes
RIGHT JOIN
tabla_de_vendedores
ON tabla_de_clientes.BARRIO = tabla_de_vendedores.BARRIO;

````

12) Las subconsultas permiten realizar selecciones usando como criterios otras selecciones. Digita:

````sql

SELECT * FROM tabla_de_clientes
WHERE BARRIO IN (SELECT DISTINCT BARRIO FROM tabla_de_vendedores);

````


13) Podemos aplicar una consulta sobre otra consulta directamente. Digita:

````sql

SELECT X.ENVASE, X.PRECIO_MAXIMO FROM
(SELECT ENVASE, MAX(PRECIO_DE_LISTA) 
AS PRECIO_MAXIMO FROM tabla_de_productos GROUP BY ENVASE) X
WHERE X.PRECIO_MAXIMO >=10;

````


14) Podemos transformar una consulta en una vista (View) que después puede ser usada en otras consultas como una tabla. Crea la vista. Para ello, expande el árbol de la esquina superior izquierda, donde tenemos el nombre de la base, y vaya a Views.


15) Haz clic con el botón derecho del mouse y selecciona la opción Create View…

16) Digita el siguiente comando:

````sql
CREATE VIEW ‘vw_envases_grandes’
AS SELECT ENVASE, MAX(PRECIO_DE_LISTA) 
AS PRECIO_MAXIMO FROM tabla_de_productos GROUP BY ENVASE;

````
17) Haz clic en Apply y sigue los pasos hasta crear la vista.

18) Podemos manipular la vista como una tabla. Digita:

````sql

SELECT X.ENVASE, X. PRECIO_MAXIMO FROM
vw_envases_grandes X 
WHERE PRECIO_MAXIMO >=10;

````


19) Podemos crear JOINs de tablas con views:

````sql

SELECT A.NOMBRE_DEL_PRODUCTO, A.ENVASE, A.PRECIO_DE_LISTA, 
B.PRECIO_MAXIMO FROM tabla_de_productos A
INNER JOIN
vw_envases_grandes B
ON A.ENVASE = B.ENVASE;

````















````sql

SELECT * FROM tabla_de_productos LIMIT 5;

````

El output está limitado a los primeros 5 registros.

5) Podemos exhibir los registros dentro de un intervalo de filas. 

Digita:

````sql

SELECT * FROM tabla_de_productos LIMIT 5,4;

````

6) El output de un comando SELECT puede ser presentado de forma ordenada. 

Observa:

````sql

SELECT * FROM tabla_de_productos ORDER BY PRECIO_DE_LISTA;

````

Tenemos los valores ordenados por precio de lista, de menor a mayor.

7) Podemos cambiar este orden. Digita:

````sql

SELECT * FROM tabla_de_productos ORDER BY PRECIO_DE_LISTA DESC;

````

8) Los valores pueden ser ordenados alfabéticamente cuando incluimos un campo de texto en el criterio de ordenamiento. Digita:

````sql

SELECT * FROM tabla_de_productos ORDER BY NOMBRE_DEL_PRODUCTO;

````

9) También, en el criterio de ordenamiento del tipo texto, podemos cambiar el orden de mayor a menor. Digita:

````sql

SELECT * FROM tabla_de_productos ORDER BY NOMBRE_DEL_PRODUCTO DESC;

````

10) El criterio de ordenamiento puede ser diferente para cada tipo. Observa el ejemplo a continuación donde usamos dos campos como criterio de ordenamiento y un orden diferente para cada uno de ellos:

````sql

SELECT * FROM tabla_de_productos ORDER BY ENVASE DESC, NOMBRE_DEL_PRODUCTO ASC;

````

11) Los datos pueden ser agrupados. Cuando esto sucede, tenemos que aplicar um criterio de agrupamiento para los campos numéricos. Podemos emplear SUM, AVG, MAX, MIN, entre otros. Digita el comando siguiente:

````sql

SELECT ESTADO, LIMITE_DE_CREDITO FROM tabla_de_clientes;

````

Puedes notar que tenemos varias líneas para EM y JC. ¿Cómo hacemos para sumar todos los límites de crédito para EM y JC?

12) La solución está en el siguiente comando:


````sql

SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes GROUP BY ESTADO;

````


13) Podemos emplear otros criterios como el valor máximo.

````sql

SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS MAYOR_PRECIO 
FROM tabla_de_productos GROUP BY ENVASE;


````

Aquí vemos el mayor precio de lista para cada tipo de envase.

14) El comando COUNT cuenta el número de ocurrencias en la tabla. Digita:

````sql

SELECT ENVASE, COUNT(*) FROM tabla_de_productos 
GROUP BY ENVASE;


````


Tenemos el número de productos cuyo envase es botella PET, botella de vidrio y Lata.

15) El filtro puede ser aplicado sobre el agrupamiento, como una consulta cualquiera. Digita:

````sql

SELECT BARRIO, SUM(LIMITE_DE_CREDITO) AS LIMITE 
FROM tabla_de_clientes GROUP BY BARRIO;

````

16) Adicionalmente, el agrupamiento puede ser realizado en más de un campo. Digita:

````sql

SELECT ESTADO, BARRIO, MAX(LIMITE_DE_CREDITO) AS LIMITE 
FROM tabla_de_clientes GROUP BY ESTADO, BARRIO;

````

17) Podemos mezclar agrupamiento com ordenamiento. Digita:

````sql

SELECT ESTADO, BARRIO, MAX(LIMITE_DE_CREDITO) AS LIMITE,
EDAD FROM tabla_de_clientes 
WHERE EDAD >=20
GROUP BY ESTADO, BARRIO
ORDER BY EDAD;

````

18) Observa la consulta a continuación:

````sql

SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes GROUP BY ESTADO;

````

19) Queremos aplicar un filtro sobre el resultado de esta consulta. Entonces, digita:

````sql

SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes WHERE LIMITE_TOTAL > 300000
GROUP BY ESTADO;

````

Nota que la consulta anterior generó un error.

20) Usamos el comando HAVING para filtrar el output de una consulta usando como criterio el valor agrupado. Digita:

````sql

SELECT ESTADO, SUM(LIMITE_DE_CREDITO) AS LIMITE_TOTAL
FROM tabla_de_clientes 
GROUP BY ESTADO
HAVING LIMITE_TOTAL > 300000;

````

21) El criterio usado con el comando HAVING no necesita ser el mismo usado en el filtro. Observa el siguiente comando:

````sql

SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO,
MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO 
FROM tabla_de_productos GROUP BY ENVASE;

````

Utiliza el MIN para agrupamiento.

22) Pero, en la siguiente consulta, el criterio del comando HAVING pide la suma. Digita:

````sql

SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO,
MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO 
FROM tabla_de_productos GROUP BY ENVASE;

````

23) Al utilizar HAVING podemos usar más de un criterio empleando AND u OR.

````sql

SELECT ENVASE, MAX(PRECIO_DE_LISTA) AS PRECIO_MAXIMO,
MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO,
SUM(PRECIO_DE_LISTA) AS SUMA_PRECIO
FROM tabla_de_productos GROUP BY ENVASE
HAVING SUM(PRECIO_DE_LISTA) >= 80 
AND MAX(PRECIO_DE_LISTA) >= 5;

````

24) El comando CASE permite la clasificación de cada registro de la tabla. Digita el comando siguiente:

````sql

SELECT NOMBRE_DEL_PRODUCTO, PRECIO_DE_LISTA,
CASE
   WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
   WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
   ELSE 'Barato'
END AS PRECIO
FROM tabla_de_productos;

````

Con CASE fue posible clasificar los productos como Costoso, Barato o Asequible conforme al valor de su precio de lista.

25) Podemos usar el comando CASE como criterio de agrupamiento, Digita el siguiente comando:

````sql

SELECT ENVASE, SABOR,
CASE
   WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
   WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
   ELSE 'Barato'
END AS PRECIO, MIN(PRECIO_DE_LISTA) AS PRECIO_MINIMO
FROM tabla_de_productos
WHERE TAMANO = '700 ml'
GROUP BY ENVASE,
CASE
   WHEN PRECIO_DE_LISTA >= 12 THEN 'Costoso'
   WHEN PRECIO_DE_LISTA >= 5 AND PRECIO_DE_LISTA < 12 THEN 'Asequible'
   ELSE 'Barato'
END
ORDER BY ENVASE;


````


