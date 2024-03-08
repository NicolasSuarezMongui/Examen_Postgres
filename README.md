# Examen Postgres

## 1. Modelo entidad/relación

### Script de creación
``` sql
DROP DATABASE IF EXISTS ventas;
CREATE DATABASE ventas;

CREATE TABLE cliente (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido1 VARCHAR(100) NOT NULL,
    apellido2 VARCHAR(100),
    ciudad VARCHAR(100),
    categoria INTEGER CHECK (categoria > 0)
);

CREATE TABLE comercial (
    id SERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    apellido1 VARCHAR(100) NOT NULL,
    apellido2 VARCHAR(100),
    comision FLOAT
);

CREATE TABLE pedido (
    id SERIAL PRIMARY KEY,
    total FLOAT NOT NULL,
    fecha DATE,
    id_cliente INTEGER REFERENCES cliente(id),
    id_comercial INTEGER REFERENCES comercial(id)
);

```

### Script de población
``` sql
INSERT INTO cliente VALUES(1, 'Aarón', 'Rivero', 'Gómez', 'Almería', 100);
INSERT INTO cliente VALUES(2, 'Adela', 'Salas', 'Díaz', 'Granada', 200);
INSERT INTO cliente VALUES(3, 'Adolfo', 'Rubio', 'Flores', 'Sevilla', NULL);
INSERT INTO cliente VALUES(4, 'Adrián', 'Suárez', NULL, 'Jaén', 300);
INSERT INTO cliente VALUES(5, 'Marcos', 'Loyola', 'Méndez', 'Almería', 200);
INSERT INTO cliente VALUES(6, 'María', 'Santana', 'Moreno', 'Cádiz', 100);
INSERT INTO cliente VALUES(7, 'Pilar', 'Ruiz', NULL, 'Sevilla', 300);
INSERT INTO cliente VALUES(8, 'Pepe', 'Ruiz', 'Santana', 'Huelva', 200);
INSERT INTO cliente VALUES(9, 'Guillermo', 'López', 'Gómez', 'Granada', 225);
INSERT INTO cliente VALUES(10, 'Daniel', 'Santana', 'Loyola', 'Sevilla', 125);

INSERT INTO comercial VALUES(1, 'Daniel', 'Sáez', 'Vega', 0.15);
INSERT INTO comercial VALUES(2, 'Juan', 'Gómez', 'López', 0.13);
INSERT INTO comercial VALUES(3, 'Diego','Flores', 'Salas', 0.11);
INSERT INTO comercial VALUES(4, 'Marta','Herrera', 'Gil', 0.14);
INSERT INTO comercial VALUES(5, 'Antonio','Carretero', 'Ortega', 0.12);
INSERT INTO comercial VALUES(6, 'Manuel','Domínguez', 'Hernández', 0.13);
INSERT INTO comercial VALUES(7, 'Antonio','Vega', 'Hernández', 0.11);
INSERT INTO comercial VALUES(8, 'Alfredo','Ruiz', 'Flores', 0.05);

INSERT INTO pedido VALUES(1, 150.5, '2017-10-05', 5, 2);
INSERT INTO pedido VALUES(2, 270.65, '2016-09-10', 1, 5);
INSERT INTO pedido VALUES(3, 65.26, '2017-10-05', 2, 1);
INSERT INTO pedido VALUES(4, 110.5, '2016-08-17', 8, 3);
INSERT INTO pedido VALUES(5, 948.5, '2017-09-10', 5, 2);
INSERT INTO pedido VALUES(6, 2400.6, '2016-07-27', 7, 1);
INSERT INTO pedido VALUES(7, 5760, '2015-09-10', 2, 1);
INSERT INTO pedido VALUES(8, 1983.43, '2017-10-10', 4, 6);
INSERT INTO pedido VALUES(9, 2480.4, '2016-10-10', 8, 3);
INSERT INTO pedido VALUES(10, 250.45, '2015-06-27', 8, 2);
INSERT INTO pedido VALUES(11, 75.29, '2016-08-17', 3, 7);
INSERT INTO pedido VALUES(12, 3045.6, '2017-04-25', 2, 1);
INSERT INTO pedido VALUES(13, 545.75, '2019-01-25', 6, 1);
INSERT INTO pedido VALUES(14, 145.82, '2017-02-02', 6, 1);
INSERT INTO pedido VALUES(15, 370.85, '2019-03-11', 1, 5);
INSERT INTO pedido VALUES(16, 2389.23, '2019-03-11', 1, 5);
```

## 2. Consultas sobre una tabla
1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

    ``` sql
    SELECT * FROM pedido ORDER BY fecha DESC;
    ```

2. Devuelve todos los datos de los dos pedidos de mayor valor.

    ``` sql
    SELECT * FROM pedido ORDER BY total DESC LIMIT 2;
    ```

3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

    ``` sql
    SELECT DISTINCT id_cliente FROM pedido;
    ```

4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

    ``` sql
    SELECT * FROM pedido WHERE extract(year FROM fecha) = 2017 AND total>500;
    ```

5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

    ``` sql
    SELECT nombre || ' ' || apellido1 || ' ' || COALESCE(apellido2, '') FROM comercial WHERE comision BETWEEN 0.05 AND 0.11;
    ```

6. Devuelve el valor de la comisión de mayor valor que existe en la tabla `comercial`.

    ``` sql
    SELECT MAX(comision) FROM comercial;
    ```

7. Devuelve el identificador, nombre y primer apellido de aquellos cuyo segundo apellido **no** es ` NULL `. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

    ``` sql
    SELECT id, nombre || ' ' || apellido1 FROM cliente WHERE apellido2 IS NOT NULL ORDER BY apellido1, nombre;
    ```

8. Devuelve un listado de los nombres de los clientes que empiezan por `A` y terminan por `n` y también los nombres que empiezan por `P`. El listado deberá estar ordenado alfabéticamente.

    ``` sql
    SELECT nombre FROM cliente WHERE nombre LIKE 'A%n' OR nombre LIKE 'P%' ORDER BY nombre;
    ```

9. Devuelve un listado de los nombres de los cleintes que **no** empiezan por `A`. El listado deberá estar ordenado alfabéticamente.

    ``` sql
    SELECT nombre FROM cliente WHERE nombre NOT LIKE 'A%' ORDER BY nombre;
    ```

10. Devuelve un listado con los nombres de los comerciales que terminan por `el` o `o`. Tenga en cuenta que se deberán eliminar los nombres repetidos.

    ``` sql
    SELECT DISTINCT nombre FROM comercial WHERE nombre LIKE '%el' OR nombre LIKE '%o';
    ```

## 3. Consultas multitabla (Composición interna)

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.

    ``` sql
    SELECT DISTINCT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(apellido2, '') AS cliente FROM cliente c, pedido p WHERE c.id = p.id_cliente;
    ```

2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

    ``` sql
    SELECT c.*, p.* FROM cliente c, pedido p WHERE c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

    ``` sql
    SELECT p.*, c.* FROM comercial c, pedido p WHERE c.id = p.id_comercial ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

    ``` sql
    SELECT c.*, p.*, c1.* FROM cliente c, pedido p, comercial c1 WHERE c.id = p.id_cliente AND c1.id = p.id_comercial;
    ```

5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año `2017`, cuya cantidad esté entre `300` € y `1000` €.

    ``` sql
    SELECT * FROM cliente c, pedido p WHERE c.id = p.id_cliente AND extract(year FROM p.fecha) = 2017 AND p.total BETWEEN 300 AND 1000;
    ```

6. Devuelve el nombre y los apellidos de todos los comerciales que han participado en algún pedido realizado por `María Santana Moreno`.

    ``` sql
    SELECT DISTINCT c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS comercial FROM comercial c, pedido p, cliente c1 WHERE c.id = p.id_comercial AND c1.id = p.id_cliente AND c1.nombre = 'María' AND c1.apellido1 = 'Santana' AND c1.apellido2 = 'Moreno';
    ```

7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial `Daniel Sáez Vega`.

    ``` sql
    SELECT DISTINCT c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2,'') AS cliente FROM cliente c, pedido p, comercial c1 WHERE c.id = p.id_cliente AND c1.id = p.id_comercial AND c1.nombre = 'Daniel' AND c1.apellido1 = 'Sáez' AND c1.apellido2 = 'Vega';
    ```

## 4. Consultas multitabla (Composición externa)

1. Devuelve un listado con **todos los clientes** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.

    ``` sql
    SELECT c.*, p.* FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

2. Devuelve un listado con **todos los comerciales** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

    ``` sql
    SELECT c.*, p.* FROM comercial c RIGHT JOIN pedido p ON c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

    ``` sql
    SELECT c.* FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente WHERE p.id IS NULL; 
    ```

4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

    ``` sql
    SELECT c.* FROM comercial c LEFT JOIN pedido p ON c.id = p.id_comercial WHERE p.id IS NULL;
    ```

5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En el listado deberá diferenciar de algún modo los clientes y los comerciales.

    ``` sql
    SELECT c.nombre,c.apellido1, 'Cliente' AS Tipo FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente WHERE p.id IS NULL
    UNION
    SELECT c.nombre, c.apellido1, 'Comercial' AS Tipo FROM comercial c LEFT JOIN pedido p ON c.id = p.id_comercial WHERE p.id IS NULL
    ORDER BY apellido1, nombre;
    ```

6. ¿Se podrían realizar las consultas anteriores con `NATURAL LEFT JOIN` o `NATURAL RIGHT JOIN`? Justifique su respuesta.

    - No es posible ya que el `NATURAL` obliga o valida que en ambas tablas donde se este haciendo el `JOIN` tenga las mismas columnas y con los mismos nombres en este caso tocaría modificar todas las tablas lo cual no es viable.

## 5. Consultas resumen

1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT COUNT(*) AS "Número de pedidos", SUM(total) AS "Total de ventas" FROM pedido;
    ```

2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT AVG(total) AS "Media de ventas" FROM pedido;
    ```

3. Calcula el número total de comerciales distintos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT COUNT(DISTINCT c.id) FROM comercial c, pedido p WHERE c.id = p.id_comercial;
    ```

4. Calcula el número total de clientes que apaecen en la tabla `cliente`.

    ``` sql
    SELECT COUNT(*) FROM cliente;
    ```

5. Calcula cuál es la mayor cantidad que aparece en la tabla `pedido`.

    ``` sql
    SELECT MAX(total) FROM pedido;
    ```

6. Calcula cuál es la menor cantidad aparece en la tabla `pedido`.

    ``` sql
    SELECT MIN(total) FROM pedido;
    ```

7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla `cliente`.

    ``` sql
    SELECT ciudad, MAX(categoria) FROM cliente GROUP BY ciudad;
    ```

8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidad el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos y total.

    ``` sql
    SELECT p.fecha, MAX(p.total), c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente  FROM pedido p INNER JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id, p.fecha;
    ```

9. Calcula cuál es el máximo valor de los pedidos realizado durate el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.

    ``` sql
    SELECT p.fecha, MAX(p.total), c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente  FROM pedido p INNER JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id, p.fecha HAVING MAX(p.total) > 2000;
    ```

10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha `2016-08-17`. Muestra el identificador del comercial, nombre, apellidos y total.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS comercial, MAX(p.total) FROM pedido p INNER JOIN comercial c ON c.id = p.id_comercial WHERE fecha='2016-08-17'  GROUP BY c.id;
    ```

11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de los clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer indicando que el número de pedidos realizados es `0`.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente, count(p.id) AS "Número de pedidos" FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de los clientes **durante el año 2017**.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente, count(p.id) AS "Número de pedidos" FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente WHERE extract(year FROM p.fecha) = 2017 GROUP BY c.id;
    ```

13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es `0`. Puede hacer uso de la función **`COALESCE`**

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 AS cliente, COALESCE(MAX(p.total), 0) FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente GROUP BY p.id, c.id;
    ```

14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.

    ``` sql
    SELECT extract(year FROM p.fecha) AS Anho,MAX(p.total) FROM pedido p GROUP BY Anho ORDER BY Anho;
    ```

15. Devuelve el número total de pedidos que se han realizado cada año.

    ``` sql
    SELECT extract(year FROM p.fecha) AS Anho,COUNT(p.id) FROM pedido p GROUP BY Anho ORDER BY Anho;
    ```

## 6. Con operadores básicos de comparación

1. Devuelve un listado con todos los pedidos que ha realizado `Adela Salas Díaz`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT p.* FROM pedido p, cliente c WHERE p.id_cliente = c.id AND c.nombre = 'Adela' AND c.apellido1 = 'Salas' AND c.apellido2 = 'Díaz';
    ```

2. Devuelve el número de pedidos en los que ha participado el comercial `Daniel Sáez Vega`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT p.* FROM pedido p, comercial c WHERE p.id_comercial = c.id AND c.nombre = 'Daniel' AND c.apellido1 = 'Sáez' AND c.apellido2 = 'Vega';
    ```

3. Devuelve los datos del cliente que realizó el pedido más caro en el año `2019`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT c.* FROM cliente c, pedido p WHERE c.id = p.id_cliente AND p.total = (SELECT MAX(total) FROM pedido WHERE extract(year FROM fecha) = 2019);
    ```

4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente `Pepe Ruiz Santana`.

    ``` sql
    SELECT p.fecha, p.total FROM cliente c, pedido p WHERE c.id = p.id_cliente AND c.nombre = 'Pepe' AND c.apellido1 = 'Ruiz' AND c.apellido2 = 'Santana' GROUP BY c.id, p.fecha, p.total ORDER BY p.total LIMIT 1;
    ```

5. Devuelve un lsitado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año `2017` con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.

    ``` sql
    SELECT c.*, p.* FROM cliente c, pedido p WHERE c.id = p.id_cliente AND p.fecha BETWEEN '2017-01-01' AND '2017-12-31' AND p.total >= (SELECT AVG(total) FROM pedido WHERE extract(year FROM fecha) = 2017);
    ```

## 7 Subconsultas con `ALL` y `ANY`

1. Devuelve el pedido más caro que existe en la tabla `pedido` si hacer uso de `MAX`, `ORDER BY` ni `LIMIT`.

    ``` sql
    SELECT * FROM pedido WHERE total >= ALL(SELECT total FROM pedido);
    ```

2. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando `ANY` o `ALL`).

    ``` sql
    SELECT * FROM cliente WHERE id <> ALL(SELECT DISTINCT id_cliente FROM pedido);
    ```

3. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `ANY` o `ALL`).

    ``` sql
    SELECT * FROM comercial WHERE id <> ALL(SELECT DISTINCT id_comercial FROM pedido);
    ```

## 8. Subconsultas con `IN` y `NOT IN`

1. Devuelve un listado de los cleintes que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).

    ``` sql
    SELECT * FROM cliente WHERE id NOT IN (SELECT c.id FROM cliente c, pedido p WHERE c.id = p.id_cliente);
    ```

2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).

    ``` sql
    SELECT * FROM comercial WHERE id NOT IN (SELECT c.id FROM comercial c, pedido p WHERE c.id = p.id_comercial);
    ```

## 9. Subconsultas con `EXISTS` Y `NOT EXISTS`

1. Devuelve un lsitado de los clientes que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).

    ``` sql
    SELECT * FROM cliente c WHERE NOT EXISTS (SELECT * FROM pedido p WHERE c.id = p.id_cliente);
    ```

2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).

    ``` sql
    SELECT * FROM comercial c WHERE NOT EXISTS (SELECT * FROM pedido p WHERE c.id = p.id_comercial);
    ```