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

    <center>

     id |  total  |   fecha    | id_cliente | id_comercial
    :----:|:---------:|:------------:|:------------:|:--------------:
    16 | 2389.23 | 2019-03-11 |          1 |            5
    15 |  370.85 | 2019-03-11 |          1 |            5
    13 |  545.75 | 2019-01-25 |          6 |            1
    8 | 1983.43 | 2017-10-10 |          4 |            6
    1 |   150.5 | 2017-10-05 |          5 |            2
    3 |   65.26 | 2017-10-05 |          2 |            1
    5 |   948.5 | 2017-09-10 |          5 |            2
    12 |  3045.6 | 2017-04-25 |          2 |            1
    14 |  145.82 | 2017-02-02 |          6 |            1
    9 |  2480.4 | 2016-10-10 |          8 |            3
    2 |  270.65 | 2016-09-10 |          1 |            5
    11 |   75.29 | 2016-08-17 |          3 |            7
    4 |   110.5 | 2016-08-17 |          8 |            3
    6 |  2400.6 | 2016-07-27 |          7 |            1
    7 |    5760 | 2015-09-10 |          2 |            1
    10 |  250.45 | 2015-06-27 |          8 |            2

2. Devuelve todos los datos de los dos pedidos de mayor valor.

    ``` sql
    SELECT * FROM pedido ORDER BY total DESC LIMIT 2;
    ```

    <center>

     id | total  |   fecha    | id_cliente | id_comercial
    :----:|:--------:|:------------:|:------------:|:--------------:
    7 |   5760 | 2015-09-10 |          2 |            1
    12 | 3045.6 | 2017-04-25 |          2 |            1

3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

    ``` sql
    SELECT DISTINCT id_cliente FROM pedido;
    ```

    <center>

     |id_cliente|
    |:------------:|
    |      3
    |      5
    |      4
    |      6
    |      2
    |      7
    |      1
    |      8

5. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

    ``` sql
    SELECT * FROM pedido WHERE extract(year FROM fecha) = 2017 AND total>500;
    ```

    <center>

     id |  total  |   fecha    | id_cliente | id_comercial
    :----:|:---------:|:------------:|:------------:|:--------------:
    5 |   948.5 | 2017-09-10 |          5 |            2
    8 | 1983.43 | 2017-10-10 |          4 |            6
    12 |  3045.6 | 2017-04-25 |          2 |            1

6. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

    ``` sql
    SELECT nombre || ' ' || apellido1 || ' ' || COALESCE(apellido2, '') AS comercial FROM comercial WHERE comision BETWEEN 0.05 AND 0.11;
    ```
    <center>

    |       comercial
    :------------------------:
    Diego Flores Salas
    Antonio Vega Hernández
    Alfredo Ruiz Flores

7. Devuelve el valor de la comisión de mayor valor que existe en la tabla `comercial`.

    ``` sql
    SELECT MAX(comision) FROM comercial;
    ```

    <center>

    | max
    |------
    |0.15

8. Devuelve el identificador, nombre y primer apellido de aquellos cuyo segundo apellido **no** es ` NULL `. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

    ``` sql
    SELECT id, nombre || ' ' || apellido1 AS cliente FROM cliente WHERE apellido2 IS NOT NULL ORDER BY apellido1, nombre;
    ```

    <center>

     id |     cliente
    :----:|:-----------------:
    9 | Guillermo López
    5 | Marcos Loyola
    1 | Aarón Rivero
    3 | Adolfo Rubio
    8 | Pepe Ruiz
    2 | Adela Salas
    10 | Daniel Santana
    6 | María Santana

9. Devuelve un listado de los nombres de los clientes que empiezan por `A` y terminan por `n` y también los nombres que empiezan por `P`. El listado deberá estar ordenado alfabéticamente.

    ``` sql
    SELECT nombre FROM cliente WHERE nombre LIKE 'A%n' OR nombre LIKE 'P%' ORDER BY nombre;
    ```

    <center>

    | nombre
    :--------:
    Aarón
    Adrián
    Pepe
    Pilar

10. Devuelve un listado de los nombres de los cleintes que **no** empiezan por `A`. El listado deberá estar ordenado alfabéticamente.

    ``` sql
    SELECT nombre FROM cliente WHERE nombre NOT LIKE 'A%' ORDER BY nombre;
    ```

    <center>

    |  nombre
    :-----------:
    Daniel
    Guillermo
    Marcos
    María
    Pepe
    Pilar

11. Devuelve un listado con los nombres de los comerciales que terminan por `el` o `o`. Tenga en cuenta que se deberán eliminar los nombres repetidos.

    ``` sql
    SELECT DISTINCT nombre FROM comercial WHERE nombre LIKE '%el' OR nombre LIKE '%o';
    ```

    <center>

    | nombre
    :---------:
    Daniel
    Diego
    Antonio
    Alfredo
    Manuel

## 3. Consultas multitabla (Composición interna)

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.

    ``` sql
    SELECT DISTINCT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(apellido2, '') AS cliente FROM cliente c, pedido p WHERE c.id = p.id_cliente;
    ```

    <center>

     id |       cliente
    :----:|:----------------------:
    8 | Pepe Ruiz Santana
    5 | Marcos Loyola Méndez
    1 | Aarón Rivero Gómez
    4 | Adrián Suárez
    2 | Adela Salas Díaz
    7 | Pilar Ruiz
    3 | Adolfo Rubio Flores
    6 | María Santana Moreno

2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

    ``` sql
    SELECT c.*, p.* FROM cliente c, pedido p WHERE c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

    <center>

     id | nombre | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial
    :----:|:--------:|:-----------:|:-----------:|:---------:|:-----------:|:----:|:---------:|:------------:|:------------:|:--------------:
    5 | Marcos | Loyola    | Méndez    | Almería |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2
    5 | Marcos | Loyola    | Méndez    | Almería |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5
    3 | Adolfo | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2
    7 | Pilar  | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1
    2 | Adela  | Salas     | Díaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1
    2 | Adela  | Salas     | Díaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1
    2 | Adela  | Salas     | Díaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
    6 | María  | Santana   | Moreno    | Cádiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1
    6 | María  | Santana   | Moreno    | Cádiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1
    4 | Adrián | Suárez    |           | Jaén    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6

3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

    ``` sql
    SELECT p.*, c.* FROM comercial c, pedido p WHERE c.id = p.id_comercial ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

    <center>

     id |  total  |   fecha    | id_cliente | id_comercial | id | nombre  | apellido1 | apellido2 | comision
    :----:|:---------:|:------------:|:------------:|:--------------:|:----:|:---------:|:-----------:|:-----------:|:----------:
    15 |  370.85 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
    16 | 2389.23 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
    2 |  270.65 | 2016-09-10 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
    8 | 1983.43 | 2017-10-10 |          4 |            6 |  6 | Manuel  | Domínguez | Hernández |     0.13
    9 |  2480.4 | 2016-10-10 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
    4 |   110.5 | 2016-08-17 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
    1 |   150.5 | 2017-10-05 |          5 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    10 |  250.45 | 2015-06-27 |          8 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    5 |   948.5 | 2017-09-10 |          5 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    13 |  545.75 | 2019-01-25 |          6 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    14 |  145.82 | 2017-02-02 |          6 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    3 |   65.26 | 2017-10-05 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    6 |  2400.6 | 2016-07-27 |          7 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    12 |  3045.6 | 2017-04-25 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    7 |    5760 | 2015-09-10 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    11 |   75.29 | 2016-08-17 |          3 |            7 |  7 | Antonio | Vega      | Hernández |     0.11

4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

    ``` sql
    SELECT c.*, p.*, c1.* FROM cliente c, pedido p, comercial c1 WHERE c.id = p.id_cliente AND c1.id = p.id_comercial;
    ```

     id | nombre | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial | id | nombre  | apellido1 | apellido2 | comision
    :----:|:--------:|:-----------:|:-----------:|:---------:|:-----------:|:----:|:---------:|:------------:|:------------:|:--------------:|:----:|:---------:|:-----------:|:-----------:|:----------:
    5 | Marcos | Loyola    | Méndez    | Almería |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
    2 | Adela  | Salas     | Díaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
    5 | Marcos | Loyola    | Méndez    | Almería |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    7 | Pilar  | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    2 | Adela  | Salas     | Díaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    4 | Adrián | Suárez    |           | Jaén    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6 |  6 | Manuel  | Domínguez | Hernández |     0.13
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3 |  3 | Diego   | Flores    | Salas     |     0.11
    8 | Pepe   | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2 |  2 | Juan    | Gómez     | López     |     0.13
    3 | Adolfo | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7 |  7 | Antonio | Vega      | Hernández |     0.11
    2 | Adela  | Salas     | Díaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    6 | María  | Santana   | Moreno    | Cádiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    6 | María  | Santana   | Moreno    | Cádiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1 |  1 | Daniel  | Sáez      | Vega      |     0.15
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12
    1 | Aarón  | Rivero    | Gómez     | Almería |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5 |  5 | Antonio | Carretero | Ortega    |     0.12

5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año `2017`, cuya cantidad esté entre `300` € y `1000` €.

    ``` sql
    SELECT * FROM cliente c, pedido p WHERE c.id = p.id_cliente AND extract(year FROM p.fecha) = 2017 AND p.total BETWEEN 300 AND 1000;
    ```

     id | nombre | apellido1 | apellido2 | ciudad  | categoria | id | total |   fecha    | id_cliente | id_comercial
    :----:|:--------:|:-----------:|:-----------:|:---------:|:-----------:|:----:|:-------:|:------------:|:------------:|:--------------:
    5 | Marcos | Loyola    | Méndez    | Almería |       200 |  5 | 948.5 | 2017-09-10 |          5 |            2

6. Devuelve el nombre y los apellidos de todos los comerciales que han participado en algún pedido realizado por `María Santana Moreno`.

    ``` sql
    SELECT DISTINCT c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS comercial FROM comercial c, pedido p, cliente c1 WHERE c.id = p.id_comercial AND c1.id = p.id_cliente AND c1.nombre = 'María' AND c1.apellido1 = 'Santana' AND c1.apellido2 = 'Moreno';
    ```

    <center>

    |    comercial
    :------------------:
    Daniel Sáez Vega

7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial `Daniel Sáez Vega`.

    ``` sql
    SELECT DISTINCT c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2,'') AS cliente FROM cliente c, pedido p, comercial c1 WHERE c.id = p.id_cliente AND c1.id = p.id_comercial AND c1.nombre = 'Daniel' AND c1.apellido1 = 'Sáez' AND c1.apellido2 = 'Vega';
    ```

    <center>

    |       cliente
    :----------------------:
    Adela Salas Díaz
    María Santana Moreno
    Pilar Ruiz

## 4. Consultas multitabla (Composición externa)

1. Devuelve un listado con **todos los clientes** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.

    ``` sql
    SELECT c.*, p.* FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```
     id |  nombre   | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial
    :----:|:-----------:|:-----------:|:-----------:|:---------:|:-----------:|:----:|:---------:|:------------:|:------------:|:--------------:
    9 | Guillermo | López     | Gómez     | Granada |       225 |    |         |            |            |
    5 | Marcos    | Loyola    | Méndez    | Almería |       200 |  1 |   150.5 | 2017-10-05 |          5 |            2
    5 | Marcos    | Loyola    | Méndez    | Almería |       200 |  5 |   948.5 | 2017-09-10 |          5 |            2
    1 | Aarón     | Rivero    | Gómez     | Almería |       100 |  2 |  270.65 | 2016-09-10 |          1 |            5
    1 | Aarón     | Rivero    | Gómez     | Almería |       100 | 16 | 2389.23 | 2019-03-11 |          1 |            5
    1 | Aarón     | Rivero    | Gómez     | Almería |       100 | 15 |  370.85 | 2019-03-11 |          1 |            5
    3 | Adolfo    | Rubio     | Flores    | Sevilla |           | 11 |   75.29 | 2016-08-17 |          3 |            7
    8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  4 |   110.5 | 2016-08-17 |          8 |            3
    8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 |  9 |  2480.4 | 2016-10-10 |          8 |            3
    8 | Pepe      | Ruiz      | Santana   | Huelva  |       200 | 10 |  250.45 | 2015-06-27 |          8 |            2
    7 | Pilar     | Ruiz      |           | Sevilla |       300 |  6 |  2400.6 | 2016-07-27 |          7 |            1
    2 | Adela     | Salas     | Díaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
    2 | Adela     | Salas     | Díaz      | Granada |       200 |  3 |   65.26 | 2017-10-05 |          2 |            1
    2 | Adela     | Salas     | Díaz      | Granada |       200 |  7 |    5760 | 2015-09-10 |          2 |            1
    10 | Daniel    | Santana   | Loyola    | Sevilla |       125 |    |         |            |            |
    6 | María     | Santana   | Moreno    | Cádiz   |       100 | 13 |  545.75 | 2019-01-25 |          6 |            1
    6 | María     | Santana   | Moreno    | Cádiz   |       100 | 14 |  145.82 | 2017-02-02 |          6 |            1
    4 | Adrián    | Suárez    |           | Jaén    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6

2. Devuelve un listado con **todos los comerciales** junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

    ``` sql
    SELECT c.*, p.* FROM comercial c RIGHT JOIN pedido p ON c.id = p.id_cliente ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

     id | nombre  | apellido1 | apellido2 | comision | id |  total  |   fecha    | id_cliente | id_comercial
    :----:|:---------:|:-----------:|:-----------:|:----------:|:----:|:---------:|:------------:|:------------:|:--------------:
    5 | Antonio | Carretero | Ortega    |     0.12 |  1 |   150.5 | 2017-10-05 |          5 |            2
    5 | Antonio | Carretero | Ortega    |     0.12 |  5 |   948.5 | 2017-09-10 |          5 |            2
    6 | Manuel  | Domínguez | Hernández |     0.13 | 13 |  545.75 | 2019-01-25 |          6 |            1
    6 | Manuel  | Domínguez | Hernández |     0.13 | 14 |  145.82 | 2017-02-02 |          6 |            1
    3 | Diego   | Flores    | Salas     |     0.11 | 11 |   75.29 | 2016-08-17 |          3 |            7
    2 | Juan    | Gómez     | López     |     0.13 |  3 |   65.26 | 2017-10-05 |          2 |            1
    2 | Juan    | Gómez     | López     |     0.13 | 12 |  3045.6 | 2017-04-25 |          2 |            1
    2 | Juan    | Gómez     | López     |     0.13 |  7 |    5760 | 2015-09-10 |          2 |            1
    4 | Marta   | Herrera   | Gil       |     0.14 |  8 | 1983.43 | 2017-10-10 |          4 |            6
    8 | Alfredo | Ruiz      | Flores    |     0.05 |  9 |  2480.4 | 2016-10-10 |          8 |            3
    8 | Alfredo | Ruiz      | Flores    |     0.05 |  4 |   110.5 | 2016-08-17 |          8 |            3
    8 | Alfredo | Ruiz      | Flores    |     0.05 | 10 |  250.45 | 2015-06-27 |          8 |            2
    1 | Daniel  | Sáez      | Vega      |     0.15 |  2 |  270.65 | 2016-09-10 |          1 |            5
    1 | Daniel  | Sáez      | Vega      |     0.15 | 15 |  370.85 | 2019-03-11 |          1 |            5
    1 | Daniel  | Sáez      | Vega      |     0.15 | 16 | 2389.23 | 2019-03-11 |          1 |            5
    7 | Antonio | Vega      | Hernández |     0.11 |  6 |  2400.6 | 2016-07-27 |          7 |            1

3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

    ``` sql
    SELECT c.* FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente WHERE p.id IS NULL; 
    ```

    <center>

     id |  nombre   | apellido1 | apellido2 | ciudad  | categoria
    :----:|:-----------:|:-----------:|:-----------:|:---------:|:-----------:
    10 | Daniel    | Santana   | Loyola    | Sevilla |       125
    9 | Guillermo | López     | Gómez     | Granada |       225

4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

    ``` sql
    SELECT c.* FROM comercial c LEFT JOIN pedido p ON c.id = p.id_comercial WHERE p.id IS NULL;
    ```

    <center>

     id | nombre  | apellido1 | apellido2 | comision
    :----:|:---------:|:-----------:|:-----------:|:----------:
    8 | Alfredo | Ruiz      | Flores    |     0.05
    4 | Marta   | Herrera   | Gil       |     0.14

5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En el listado deberá diferenciar de algún modo los clientes y los comerciales.

    ``` sql
    SELECT c.nombre,c.apellido1, 'Cliente' AS Tipo FROM cliente c LEFT JOIN pedido p ON c.id = p.id_cliente WHERE p.id IS NULL
    UNION
    SELECT c.nombre, c.apellido1, 'Comercial' AS Tipo FROM comercial c LEFT JOIN pedido p ON c.id = p.id_comercial WHERE p.id IS NULL
    ORDER BY apellido1, nombre;
    ```
    <center>

      nombre   | apellido1 |   tipo
    :-----------:|:-----------:|:-----------:
    Marta     | Herrera   | Comercial
    Guillermo | López     | Cliente
    Alfredo   | Ruiz      | Comercial
    Daniel    | Santana   | Cliente


6. ¿Se podrían realizar las consultas anteriores con `NATURAL LEFT JOIN` o `NATURAL RIGHT JOIN`? Justifique su respuesta.

    - No es posible ya que el `NATURAL` obliga o valida que en ambas tablas donde se este haciendo el `JOIN` tenga las mismas columnas y con los mismos nombres en este caso tocaría modificar todas las tablas lo cual no es viable.

## 5. Consultas resumen

1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT COUNT(*) AS "Número de pedidos", SUM(total) AS "Total de ventas" FROM pedido;
    ```

    <center>

     Número de pedidos |  Total de ventas
    :-------------------:|:--------------------:
    |                16 | 20992.829999999998


2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT AVG(total) AS "Media de ventas" FROM pedido;
    ```

    <center>

    |  Media de ventas
    :--------------------:
    1312.0518749999999


3. Calcula el número total de comerciales distintos que aparecen en la tabla `pedido`.

    ``` sql
    SELECT COUNT(DISTINCT c.id) FROM comercial c, pedido p WHERE c.id = p.id_comercial;
    ```

    <center>

    | count
    :-------:
    |    6


4. Calcula el número total de clientes que apaecen en la tabla `cliente`.

    ``` sql
    SELECT COUNT(*) FROM cliente;
    ```

    <center>

    | count
    :-------:
    |    10

5. Calcula cuál es la mayor cantidad que aparece en la tabla `pedido`.

    ``` sql
    SELECT MAX(total) FROM pedido;
    ```

    <center>

    | max
    :------:
    |5760

6. Calcula cuál es la menor cantidad aparece en la tabla `pedido`.

    ``` sql
    SELECT MIN(total) FROM pedido;
    ```

    <center>

    |  min
    :-------:
    |65.26

7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla `cliente`.

    ``` sql
    SELECT ciudad, MAX(categoria) FROM cliente GROUP BY ciudad;
    ```

    <center>

     ciudad  | max
    :---------:|:-----:
    Almería | 200
    Huelva  | 200
    Sevilla | 300
    Granada | 225
    Jaén    | 300
    Cádiz   | 100

8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidad el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos y total.

    ``` sql
    SELECT p.fecha, MAX(p.total), c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente  FROM pedido p INNER JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id, p.fecha;
    ```

    <center>

    fecha    |   max   | id |       cliente
    :------------:|:---------:|:----:|:----------------------:
    2017-02-02 |  145.82 |  6 | María Santana Moreno
    2015-06-27 |  250.45 |  8 | Pepe Ruiz Santana
    2019-03-11 | 2389.23 |  1 | Aarón Rivero Gómez
    2015-09-10 |    5760 |  2 | Adela Salas Díaz
    2016-10-10 |  2480.4 |  8 | Pepe Ruiz Santana
    2019-01-25 |  545.75 |  6 | María Santana Moreno
    2016-08-17 |   75.29 |  3 | Adolfo Rubio Flores
    2017-04-25 |  3045.6 |  2 | Adela Salas Díaz
    2016-08-17 |   110.5 |  8 | Pepe Ruiz Santana
    2017-10-05 |   150.5 |  5 | Marcos Loyola Méndez
    2017-09-10 |   948.5 |  5 | Marcos Loyola Méndez
    2016-07-27 |  2400.6 |  7 | Pilar Ruiz
    2016-09-10 |  270.65 |  1 | Aarón Rivero Gómez
    2017-10-10 | 1983.43 |  4 | Adrián Suárez
    2017-10-05 |   65.26 |  2 | Adela Salas Díaz

9. Calcula cuál es el máximo valor de los pedidos realizado durate el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.

    ``` sql
    SELECT p.fecha, MAX(p.total), c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente  FROM pedido p INNER JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id, p.fecha HAVING MAX(p.total) > 2000;
    ```

    <center>

    fecha    |   max   | id |      cliente
    :------------:|:---------:|:----:|:--------------------:
    2019-03-11 | 2389.23 |  1 | Aarón Rivero Gómez
    2015-09-10 |    5760 |  2 | Adela Salas Díaz
    2016-10-10 |  2480.4 |  8 | Pepe Ruiz Santana
    2017-04-25 |  3045.6 |  2 | Adela Salas Díaz
    2016-07-27 |  2400.6 |  7 | Pilar Ruiz

10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha `2016-08-17`. Muestra el identificador del comercial, nombre, apellidos y total.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS comercial, MAX(p.total) FROM pedido p INNER JOIN comercial c ON c.id = p.id_comercial WHERE fecha='2016-08-17'  GROUP BY c.id;
    ```

    <center>

     id |       comercial        |  max
    :----:|:------------------------:|:-------:
    3 | Diego Flores Salas     | 110.5
    7 | Antonio Vega Hernández | 75.29

11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de los clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer indicando que el número de pedidos realizados es `0`.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente, count(p.id) AS "Número de pedidos" FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente GROUP BY c.id ORDER BY c.apellido1, c.apellido2, c.nombre;
    ```

    <center>

     id |        cliente        | Número de pedidos
    :----:|:-----------------------:|:-------------------:
    9 | Guillermo López Gómez |                 0
    5 | Marcos Loyola Méndez  |                 2
    1 | Aarón Rivero Gómez    |                 3
    3 | Adolfo Rubio Flores   |                 1
    8 | Pepe Ruiz Santana     |                 3
    7 | Pilar Ruiz            |                 1
    2 | Adela Salas Díaz      |                 3
    10 | Daniel Santana Loyola |                 0
    6 | María Santana Moreno  |                 2
    4 | Adrián Suárez         |                 1

12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de los clientes **durante el año 2017**.

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 || ' ' || COALESCE(c.apellido2, '') AS cliente, count(p.id) AS "Número de pedidos" FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente WHERE extract(year FROM p.fecha) = 2017 GROUP BY c.id;
    ```

    <center>

     id |       cliente        | Número de pedidos
    :----:|:----------------------:|:-------------------:
    2 | Adela Salas Díaz     |                 2
    4 | Adrián Suárez        |                 1
    5 | Marcos Loyola Méndez |                 2
    6 | María Santana Moreno |                 1

13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es `0`. Puede hacer uso de la función **`COALESCE`**

    ``` sql
    SELECT c.id, c.nombre || ' ' || c.apellido1 AS cliente, COALESCE(MAX(p.total), 0) AS Máximo FROM pedido p RIGHT JOIN cliente c ON c.id = p.id_cliente GROUP BY p.id_cliente, c.id;
    ```

    <center>

     id |     cliente     | máximo
    :----:|:-----------------:|:---------:
    1 | Aarón Rivero    | 2389.23
    7 | Pilar Ruiz      |  2400.6
    6 | María Santana   |  545.75
    10 | Daniel Santana  |       0
    2 | Adela Salas     |    5760
    8 | Pepe Ruiz       |  2480.4
    5 | Marcos Loyola   |   948.5
    9 | Guillermo López |       0
    3 | Adolfo Rubio    |   75.29
    4 | Adrián Suárez   | 1983.43

14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.

    ``` sql
    SELECT extract(year FROM p.fecha) AS Anho,MAX(p.total) FROM pedido p GROUP BY Anho ORDER BY Anho;
    ```

    <center>

     anho |   max
    :------:|:---------:
    2015 |    5760
    2016 |  2480.4
    2017 |  3045.6
    2019 | 2389.23

15. Devuelve el número total de pedidos que se han realizado cada año.

    ``` sql
    SELECT extract(year FROM p.fecha) AS Anho,COUNT(p.id) FROM pedido p GROUP BY Anho ORDER BY Anho;
    ```

    <center>

     anho | count
    :------:|:-------:
    2015 |     2
    2016 |     5
    2017 |     6
    2019 |     3

## 6. Con operadores básicos de comparación

1. Devuelve un listado con todos los pedidos que ha realizado `Adela Salas Díaz`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT p.* FROM pedido p, cliente c WHERE p.id_cliente = c.id AND c.nombre = 'Adela' AND c.apellido1 = 'Salas' AND c.apellido2 = 'Díaz';
    ```
    <center>

     id | total  |   fecha    | id_cliente | id_comercial
    :----:|:--------:|:------------:|:------------:|:--------------:
    3 |  65.26 | 2017-10-05 |          2 |            1
    7 |   5760 | 2015-09-10 |          2 |            1
    12 | 3045.6 | 2017-04-25 |          2 |            1

2. Devuelve el número de pedidos en los que ha participado el comercial `Daniel Sáez Vega`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT p.* FROM pedido p, comercial c WHERE p.id_comercial = c.id AND c.nombre = 'Daniel' AND c.apellido1 = 'Sáez' AND c.apellido2 = 'Vega';
    ```

    <center>

     id | total  |   fecha    | id_cliente | id_comercial
    :----:|:--------:|:------------:|:------------:|:--------------:
    3 |  65.26 | 2017-10-05 |          2 |            1
    6 | 2400.6 | 2016-07-27 |          7 |            1
    7 |   5760 | 2015-09-10 |          2 |            1
    12 | 3045.6 | 2017-04-25 |          2 |            1
    13 | 545.75 | 2019-01-25 |          6 |            1
    14 | 145.82 | 2017-02-02 |          6 |            1

3. Devuelve los datos del cliente que realizó el pedido más caro en el año `2019`. (Sin utilizar `INNER JOIN`).

    ``` sql
    SELECT c.* FROM cliente c, pedido p WHERE c.id = p.id_cliente AND p.total = (SELECT MAX(total) FROM pedido WHERE extract(year FROM fecha) = 2019);
    ```

    <center>

     id | nombre | apellido1 | apellido2 | ciudad  | categoria
    :----:|:--------:|:-----------:|:-----------:|:---------:|:-----------:
    1 | Aarón  | Rivero    | Gómez     | Almería |       100

4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente `Pepe Ruiz Santana`.

    ``` sql
    SELECT p.fecha, p.total FROM cliente c, pedido p WHERE c.id = p.id_cliente AND c.nombre = 'Pepe' AND c.apellido1 = 'Ruiz' AND c.apellido2 = 'Santana' GROUP BY c.id, p.fecha, p.total ORDER BY p.total LIMIT 1;
    ```

    <center>

    fecha    | total
    :------------:|:-------:
    2016-08-17 | 110.5

5. Devuelve un lsitado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año `2017` con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.

    ``` sql
    SELECT c.*, p.* FROM cliente c, pedido p WHERE c.id = p.id_cliente AND p.fecha BETWEEN '2017-01-01' AND '2017-12-31' AND p.total >= (SELECT AVG(total) FROM pedido WHERE extract(year FROM fecha) = 2017);
    ```

     id | nombre | apellido1 | apellido2 | ciudad  | categoria | id |  total  |   fecha    | id_cliente | id_comercial
    ----:|:--------:|:-----------:|:-----------:|:---------:|:-----------:|:----:|:---------:|:------------:|:------------:|:--------------
    2 | Adela  | Salas     | Díaz      | Granada |       200 | 12 |  3045.6 | 2017-04-25 |          2 |            1
    4 | Adrián | Suárez    |           | Jaén    |       300 |  8 | 1983.43 | 2017-10-10 |          4 |            6


## 7 Subconsultas con `ALL` y `ANY`

1. Devuelve el pedido más caro que existe en la tabla `pedido` si hacer uso de `MAX`, `ORDER BY` ni `LIMIT`.

    ``` sql
    SELECT * FROM pedido WHERE total >= ALL(SELECT total FROM pedido);
    ```

    <center>

     id | total |   fecha    | id_cliente | id_comercial
    :----:|:-------:|:------------:|:------------:|:--------------:
    7 |  5760 | 2015-09-10 |          2 |            1

2. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando `ANY` o `ALL`).

    ``` sql
    SELECT * FROM cliente WHERE id <> ALL(SELECT DISTINCT id_cliente FROM pedido);
    ```

    <center>

     id |  nombre   | apellido1 | apellido2 | ciudad  | categoria
    :----:|:-----------:|:-----------:|:-----------:|:---------:|:-----------:
    9 | Guillermo | López     | Gómez     | Granada |       225
    10 | Daniel    | Santana   | Loyola    | Sevilla |       125

3. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `ANY` o `ALL`).

    ``` sql
    SELECT * FROM comercial WHERE id <> ALL(SELECT DISTINCT id_comercial FROM pedido);
    ```

    <center>

     id | nombre  | apellido1 | apellido2 | comision
    :----:|:---------:|:-----------:|:-----------:|:----------:
    4 | Marta   | Herrera   | Gil       |     0.14
    8 | Alfredo | Ruiz      | Flores    |     0.05

## 8. Subconsultas con `IN` y `NOT IN`

1. Devuelve un listado de los cleintes que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).

    ``` sql
    SELECT * FROM cliente WHERE id NOT IN (SELECT c.id FROM cliente c, pedido p WHERE c.id = p.id_cliente);
    ```

    <center>

     id |  nombre   | apellido1 | apellido2 | ciudad  | categoria
    :----:|:-----------:|:-----------:|:-----------:|:---------:|:-----------:
    9 | Guillermo | López     | Gómez     | Granada |       225
    10 | Daniel    | Santana   | Loyola    | Sevilla |       125

2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `IN` o `NOT IN`).

    ``` sql
    SELECT * FROM comercial WHERE id NOT IN (SELECT c.id FROM comercial c, pedido p WHERE c.id = p.id_comercial);
    ```

    <center>

     id | nombre  | apellido1 | apellido2 | comision
    :----:|:---------:|:-----------:|:-----------:|:----------:
    4 | Marta   | Herrera   | Gil       |     0.14
    8 | Alfredo | Ruiz      | Flores    |     0.05

## 9. Subconsultas con `EXISTS` Y `NOT EXISTS`

1. Devuelve un lsitado de los clientes que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).

    ``` sql
    SELECT * FROM cliente c WHERE NOT EXISTS (SELECT * FROM pedido p WHERE c.id = p.id_cliente);
    ```

    <center>

     id |  nombre   | apellido1 | apellido2 | ciudad  | categoria
    :----:|:-----------:|:-----------:|:-----------:|:---------:|:-----------:
    10 | Daniel    | Santana   | Loyola    | Sevilla |       125
    9 | Guillermo | López     | Gómez     | Granada |       225

2. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando `EXISTS` o `NOT EXISTS`).

    ``` sql
    SELECT * FROM comercial c WHERE NOT EXISTS (SELECT * FROM pedido p WHERE c.id = p.id_comercial);
    ```

    <center>

     id | nombre  | apellido1 | apellido2 | comision
    :----:|:---------:|:-----------:|:-----------:|:----------:
    8 | Alfredo | Ruiz      | Flores    |     0.05
    4 | Marta   | Herrera   | Gil       |     0.14
