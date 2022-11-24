# Consultas SQL: Avanzando en SQL con MySQL

**Presentación de los datos de una consulta**

- Presentar solamente filas distintas en una selección;
- Ordenar la salida de la selección;
- Agrupar los registros por un conjunto de campos y aplicando un criterio de agrupamiento sobre los campos numéricos (SUM, MIN, MAX, AVG, etc);
- Utilizar el comando HAVING para aplicar un filtro utilizando los campos numéricos agrupados como condición;
- Limitar la salida de las consultas;
- Usar el comando CASE para clasificar un determinado campo por un criterio.

**Pasos para realizar en esta aula:

1) De nuevo en Workbench, vamos a ver formas diferentes de exhibir los resultados. Digita:

````sql

SELECT ENVASE, TAMANO FROM tabla_de_productos;

````

Observa que tenemos registros donde el conjunto ENVASE / TAMAÑO se repite.

2) Ahora digita el comando:

````sql

SELECT DISTINCT ENVASE, TAMANO FROM tabla_de_productos;

````

El simple hecho de incluir la cláusula DISTINCT hace que los registros no se repitan.

3) Podemos aplicar filtros a la selección con DISTINCT e incluso añadir más campos.

````sql

SELECT DISTINCT ENVASE, TAMANO, SABOR FROM tabla_de_productos
WHERE SABOR = 'Naranja';

````

4) Podemos limitar el número de registros exibidos en el output. Digita:


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


