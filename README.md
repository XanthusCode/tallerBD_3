# Taller Nr 3

![Diagrama ERD](C:\Users\Johan\Documents\CampusLands\Programacion\Bases_De_Datos\Taller_3\erd.png)

#### **Consultas sobre una tabla**

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   SELECT o.codigo_oficina, c.nombre_ciudad
   FROM oficina o
   JOIN ubicacion u ON o.id_ubicacion = u.id_ubicacion
   JOIN ciudad c ON u.id_ciudad = c.id_ciudad;
   +----------------+---------------+
   | codigo_oficina | nombre_ciudad |
   +----------------+---------------+
   | OF1            | Fuenlabrada   |
   | OF2            | París         |
   | OF3            | Madrid        |
   +----------------+---------------+
   ```
   
   
   
2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```sql
   SELECT c.nombre_ciudad, co.telefono
   FROM oficina AS o
   JOIN ubicacion AS u ON o.id_ubicacion = u.id_ubicacion
   JOIN ciudad As c ON u.id_ciudad = c.id_ciudad
   JOIN pais AS p ON c.id_region = p.id_pais
   JOIN contacto AS co ON o.id_contacto = co.id_contacto
   WHERE p.nombre_pais = 'España';
   +---------------+-----------------+
   | nombre_ciudad | telefono        |
   +---------------+-----------------+
   | Sevilla	    | +34 123 456 789 |
   +---------------+-----------------+
   ```

   

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
    jefe tiene un código de jefe igual a 7.

  ```sql
  SELECT nombre, apellido1, apellido2, email
  FROM empleado
  WHERE codigo_jefe = 7;
  +--------+-----------+-----------+---------------------------+
  | nombre | apellido1 | apellido2 | email                     |
  +--------+-----------+-----------+---------------------------+
  | Juan   | Perez     | Mantilla  | JuanMantilla@empresa.com  |
  +--------+-----------+-----------+---------------------------+
  ```

  

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
    empresa.

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
    empleados que no sean representantes de ventas.

  ```sql
   SELECT nombre, apellido1, apellido2, puesto
      -> FROM empleado
      -> WHERE puesto <> 'Representante de Ventas';
  +--------+------------+------------+---------------------------+
  | nombre | apellido1  | apellido2  | puesto                    |
  +--------+------------+------------+---------------------------+
  | Juan   | Perez      | Mantila    | Gerente                   |
  | María  | González   | Martínez   | Asistente Administrativo  |
  +--------+------------+------------+---------------------------+
  ```

  

6. Devuelve un listado con el nombre de los todos los clientes españoles.

     ```sql
     SELECT cl.nombre_cliente
     FROM cliente cl
     JOIN ubicacion u ON cl.id_ubicacion = u.id_ubicacion
     JOIN ciudad ci ON u.id_ciudad = ci.id_ciudad
     JOIN region r ON ci.id_region = r.id_region
     JOIN pais p ON r.id_pais = p.id_pais
     WHERE p.nombre_pais = 'España';
     +--------------------+
     | nombre_cliente     | 
     +--------------------+
     | Pedro              |
     +--------------------+
     ```

     

7. Devuelve un listado con los distintos estados por los que puede pasar un
     pedido.

     ```sql
     SELECT  nombre_estado 
     FROM estado;
     +----------------+
     | nombre_estado  | 
     +----------------+
     | En proceso     | 
     | Finalizado     | 
     | Negado         | 
     +----------------+
     ```

     

8. Devuelve un listado con el código de cliente de aquellos clientes que
     realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
       aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
       • Utilizando la función YEAR de MySQL.

     ```sql
     SELECT DISTINCT codigo_cliente
     FROM pedido
     WHERE YEAR(fecha_pedido) = 2008;
     
     +----------------+
     | codigo_cliente | 
     +----------------+
     | 1              |
     | 3              | 
     +----------------+
     ```

       • Utilizando la función DATE_FORMAT de MySQL.

     ```sql
     SELECT DISTINCT codigo_cliente
     FROM pedido
     WHERE DATE_FORMAT(fecha_pedido, '%Y') = '2008';
     +----------------+
     | codigo_cliente | 
     +----------------+
     | 1              |
     | 3              | 
     +----------------+
     ```

       • Sin utilizar ninguna de las funciones anteriores.

     ```sql
     SELECT DISTINCT codigo_cliente
     FROM pedido
     WHERE fecha_pedido >= '2008-01-01' AND fecha_pedido < '2009-01-01';
     +----------------+
     | codigo_cliente | 
     +----------------+
     | 1              |
     | 3              | 
     +----------------+
     ```

     

9. Devuelve un listado con el código de pedido, código de cliente, fecha
     esperada y fecha de entrega de los pedidos que no han sido entregados a
       tiempo.

     ```sql
     SELECT id_pedido, codigo_cliente, fecha_esperada, fecha_entrega
     FROM pedido
     WHERE fecha_entrega > fecha_esperada;
     +-----------+-----------------+-----------------+----------------+
     | id_pedido | codigo_cliente  | fecha_esperada  | fecha_entrega  |
     +-----------+-----------------+-----------------+----------------+
     |5      	|1				  |2010-04-25	    |2010-04-26      |    
     |7     	    |1		     	  |2010-04-25     	|2010-04-27      |
     |9       	|1	              |2010-04-25       |2010-04-28      |
     +--------+------------+------------+-----------------------------+
     ```

     

10. Devuelve un listado con el código de pedido, código de cliente, fecha
     esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
     menos dos días antes de la fecha esperada.
     • Utilizando la función ADDDATE de MySQL.

     ```sql
     SELECT id_pedido, codigo_cliente, fecha_esperada, fecha_entrega
     FROM pedido
     WHERE fecha_entrega <= ADDDATE(fecha_esperada, -2);
     +-----------+-----------------+-----------------+----------------+
     | id_pedido | codigo_cliente  | fecha_esperada  | fecha_entrega  |
     +-----------+-----------------+-----------------+----------------+
     |3      	|3				  |2024-04-23	    |2024-04-20      |    
     +--------+------------+------------+-----------------------------+
     ```

     • Utilizando la función DATEDIFF de MySQL.

     ```sql
     SELECT id_pedido, codigo_cliente, fecha_esperada, fecha_entrega
     FROM pedido
     WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2;
     +-----------+-----------------+-----------------+----------------+
     | id_pedido | codigo_cliente  | fecha_esperada  | fecha_entrega  |
     +-----------+-----------------+-----------------+----------------+
     |3      	|3				  |2024-04-23	    |2024-04-20      |    
     +--------+------------+------------+-----------------------------+
     ```

     • ¿Sería posible resolver esta consulta utilizando el operador de suma + o
     resta -?

     *No es posible resolver esta consulta utilizando directamente el operador de suma (+) o resta (-) en la comparación de fechas, ya que MySQL requiere una función específica (como `DATEDIFF`) para calcular la diferencia entre fechas.*

     

     

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

      ```sql
      SELECT id_pedido, fecha_pedido, fecha_esperada, fecha_entrega, id_estado, comentarios, codigo_cliente
      FROM pedido
      WHERE id_estado = (
          SELECT id_estado FROM estado WHERE nombre_estado = 'Negado'
      ) AND YEAR(fecha_pedido) = 2009;
      
      
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |id_pedido | fecha_pedido  |fecha_esperada |fecha_entrega |id_estado |comentarios|cod|
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |2	       |2009-04-18	   |2009-04-24	   |2009-04-24	  |10	     |Coment  2  | 2 |
      +----------+---------------+---------------+--------------+----------+-----------+---+
      ```

      

12. Devuelve un listado de todos los pedidos que han sido entregados en el
      mes de enero de cualquier año.

      ```sql
      SELECT id_pedido, fecha_pedido, fecha_esperada, fecha_entrega, id_estado, comentarios, codigo_cliente
      FROM pedido
      WHERE MONTH(fecha_entrega) = 1;
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |id_pedido | fecha_pedido  |fecha_esperada |fecha_entrega |id_estado |comentarios|cod|
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |1	       |2008-04-19	   |2024-01-25	   |2024-01-25	  |1	     |Coment 1   | 1 |
      +----------+---------------+---------------+--------------+----------+-----------+---+
      ```

      

13. Devuelve un listado con todos los pagos que se realizaron en el
      año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

      ```sql
      SELECT id_pedido AS id, fecha_pedido, fecha_esperada, fecha_entrega, id_estado, comentarios, codigo_cliente, metodo_pago As met
      FROM pago
      WHERE YEAR(fecha_pago) = 2008
      AND metodo_pago = 'Paypal'
      ORDER BY monto_pago DESC;
      
      +----+-------------+---------------+--------------+----------+-----------+---+------+
      |id  | fecha_pedido|fecha_esperada |fecha_entrega |id_estado |comentarios|cod|met   |
      +----+-------------+---------------+--------------+----------+-----------+---+------+
      |8	 |2008-07-20   |2008-08-05	   |2008-08-06	  |8	     |Coment 8   | 1 |Paypal|
      |6	 |2008-06-15   |2008-07-01	   |2008-07-02	  |6	     |Coment 6   | 3 |Paypal|
      |5	 |2008-04-19   |2010-04-25	   |2010-04-26    |5	     |Coment 5   | 1 |Paypal|
      +----+-------------+---------------+--------------+----------+-----------+---+------+
      ```

      

14. Devuelve un listado con todas las formas de pago que aparecen en la
      tabla pago. Tenga en cuenta que no deben aparecer formas de pago
      repetidas.

      ```sql
      SELECT DISTINCT metodo_pago
      FROM pedido;
      
      +----------------------+
      | metodo_pago          | 
      +----------------------+
      |Tarjeta de crédito    |
      |Cheque                |
      |Transferencia bancaria|
      |Paypal                |
      |Efectivo              | 
      +----------------------+
      ```

      

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.

      ```sql
      SELECT p.id_producto AS id, p.nombre, p.id_gama AS gama, p.id_dimensiones AS dimensiones, p.id_proveedor AS proveedor, p.descripcion, p.id_stock, p.precio_venta
      FROM producto p
      JOIN gama_producto g ON p.id_gama = g.id_gama
      JOIN stock s ON p.id_stock = s.id_stock
      WHERE g.descripcion_text = 'Productos ornamentales para decoración.' AND s.stock_actual > 100
      ORDER BY p.precio_venta DESC;
      
      
      id	nombre	gama	dimensiones	proveedor	descripcion	id_stock	precio_venta
      PROD4	Florero Ornamental	Ornamentales	4	4	Florero ornamental de cristal.	1	29.99
      PROD1	Planta Ornamental	Ornamentales	1	1	Planta ornamental de interior.	1	15.99
      
      
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |id  | nombre            |  
      +----------+---------------+---------------+--------------+----------+-----------+---+
      |PROD4|Florero Ornamental|Ornamentales|4|4|Florero ornamental de cristal.|1|29.99
      |PROD1|Planta Ornamental|Ornamentales|1|1|Planta ornamental de interior.|1|15.99
      +----------+---------------+---------------+--------------+----------+-----------+---+
      ```

      

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
      cuyo representante de ventas tenga el código de empleado 11 o 30.

      ```sql
      SELECT c.codigo_cliente, c.nombre_cliente, c.nombre_contacto, c.apellido_contacto, c.codigo_empleado_rep, c.id_credito, c.id_ubicacion
      FROM cliente c
      JOIN cliente_direccion cd ON c.codigo_cliente = cd.codigo_cliente
      JOIN ubicacion u ON cd.id_ubicacion = u.id_ubicacion
      JOIN ciudad ci ON u.id_ciudad = ci.id_ciudad
      WHERE ci.nombre_ciudad = 'Madrid'
      AND c.codigo_empleado_rep IN (11, 30);
      
      
      codigo_cliente	nombre_cliente	nombre_contacto	apellido_contacto	codigo_empleado_rep	id_credito	id_ubicacion
      3	Cliente 3	Contacto 3	Apellido 3	30	3	3
      ```

      

#### Consultas multitabla (Composición interna)

*Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con*
*sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.*

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
    representante de ventas.
    
    ```sql
    SELECT c.nombre_cliente, e.nombre, e.apellido1
    FROM cliente c
    JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado;
    
    
    nombre_cliente	nombre	apellido1
    Cliente 2	Juan	Perez
    ```
    
2. Muestra el nombre de los clientes que hayan realizado pagos junto con el
    nombre de sus representantes de ventas.
    
    ```sql
    SELECT DISTINCT c.nombre_cliente, e.nombre, e.apellido1
    FROM cliente c
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado
    WHERE p.id_estado = 1; 
    
    nombre_cliente	nombre	apellido1
    Cliente 2	Juan	Perez
    ```
    
3. Muestra el nombre de los clientes que no hayan realizado pagos junto con
    el nombre de sus representantes de ventas.
    
    ```sql
    SELECT c.nombre_cliente, c.nombre_contacto, c.apellido_contacto, e.nombre AS nombre_representante
    FROM cliente c
    LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    LEFT JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado
    WHERE p.id_pedido IS NULL;
    
    nombre_cliente	nombre_contacto	apellido_contacto	nombre_representante
    Juan Jose	Contacto	Basto	Antonia
    
    ```
    
4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
    representantes junto con la ciudad de la oficina a la que pertenece el
    representante.

  ```sql
  SELECT c.nombre_cliente, e.nombre, e.apellido1, ci.nombre_ciudad
  FROM cliente c
  JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
  JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado
  JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
  JOIN ubicacion u ON o.id_ubicacion = u.id_ubicacion
  JOIN ciudad ci ON u.id_ciudad = ci.id_ciudad;
  
  
  nombre_cliente	nombre	apellido1	nombre_ciudad
  Martha	Juan	Perez	París
  Martha	Juan	Perez	París
  ```


5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
    de sus representantes junto con la ciudad de la oficina a la que pertenece el
    representante.

  ```sql
  SELECT c.nombre_cliente, e.nombre AS nombre_representante, ci.nombre_ciudad AS ciudad_oficina
  FROM cliente c
  LEFT JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado
  LEFT JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
  LEFT JOIN ciudad ci ON o.id_ubicacion = ci.id_ciudad
  WHERE c.codigo_cliente NOT IN (SELECT DISTINCT codigo_cliente FROM pedido);
  
  nombre_cliente	nombre_representante	ciudad_oficina
  Juan Jose	Antonia	Sevilla
  ```


6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

     ```sql
     SELECT u.linea_direccion1, u.linea_direccion2, u.codigo_postal
     FROM cliente c
     JOIN empleado e ON c.codigo_empleado_rep = e.codigo_empleado
     JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
     JOIN ubicacion u ON o.id_ubicacion = u.id_ubicacion
     JOIN ciudad ci ON u.id_ciudad = ci.id_ciudad
     WHERE ci.nombre_ciudad = 'Fuenlabrada';
     
     Calle Principal 123|Edificio A|41001
     ```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto
    con la ciudad de la oficina a la que pertenece el representante.
8. Devuelve un listado con el nombre de los empleados junto con el nombre
    de sus jefes.
9. Devuelve un listado que muestre el nombre de cada empleados, el nombre
    de su jefe y el nombre del jefe de sus jefe.
10. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.
11. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.



#### Consultas multitabla (Composición externa)

*Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL*
*LEFT JOIN y NATURAL RIGHT JOIN.*

1. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pago.
2. Devuelve un listado que muestre solamente los clientes que no han
realizado ningún pedido.
3. Devuelve un listado que muestre los clientes que no han realizado ningún
pago y los que no han realizado ningún pedido.
4. Devuelve un listado que muestre solamente los empleados que no tienen
una oficina asociada.
5. Devuelve un listado que muestre solamente los empleados que no tienen un
cliente asociado.
6. Devuelve un listado que muestre solamente los empleados que no tienen un
cliente asociado junto con los datos de la oficina donde trabajan.
7. Devuelve un listado que muestre los empleados que no tienen una oficina
asociada y los que no tienen un cliente asociado.
8. Devuelve un listado de los productos que nunca han aparecido en un
pedido.
9. Devuelve un listado de los productos que nunca han aparecido en un
pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
producto.

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que
hayan sido los representantes de ventas de algún cliente que haya realizado
la compra de algún producto de la gama Frutales.
11. Devuelve un listado con los clientes que han realizado algún pedido pero no
han realizado ningún pago.
12. Devuelve un listado con los datos de los empleados que no tienen clientes
asociados y el nombre de su jefe asociado.





#### Consultas resumen

1. ¿Cuántos empleados hay en la compañía?
2. ¿Cuántos clientes tiene cada país?
3. ¿Cuál fue el pago medio en 2009?
4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma
    descendente por el número de pedidos.
5. Calcula el precio de venta del producto más caro y más barato en una
    misma consulta.
6. Calcula el número de clientes que tiene la empresa.
7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?
8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan
    por M?
9. Devuelve el nombre de los representantes de ventas y el número de clientes
    al que atiende cada uno.
10. Calcula el número de clientes que no tiene asignado representante de
    ventas.
11. Calcula la fecha del primer y último pago realizado por cada uno de los
    clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.
12. Calcula el número de productos diferentes que hay en cada uno de los
    pedidos.
13. Calcula la suma de la cantidad total de todos los productos que aparecen en
    cada uno de los pedidos.
14. Devuelve un listado de los 20 productos más vendidos y el número total de
    unidades que se han vendido de cada uno. El listado deberá estar ordenado
    por el número total de unidades vendidas.
15. La facturación que ha tenido la empresa en toda la historia, indicando la
    base imponible, el IVA y el total facturado. La base imponible se calcula
    sumando el coste del producto por el número de unidades vendidas de la
    tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
    suma de los dos campos anteriores.
16. La misma información que en la pregunta anterior, pero agrupada por
    código de producto.
17. La misma información que en la pregunta anterior, pero agrupada por
    código de producto filtrada por los códigos que empiecen por OR.
18. Lista las ventas totales de los productos que hayan facturado más de 3000
    euros. Se mostrará el nombre, unidades vendidas, total facturado y total
    facturado con impuestos (21% IVA).
19. Muestre la suma total de todos los pagos que se realizaron para cada uno
    de los años que aparecen en la tabla pagos.



#### Consultas variadas

1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
han realizado ningún pedido.
2. Devuelve un listado con los nombres de los clientes y el total pagado por
cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
realizado ningún pago.
3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
ordenados alfabéticamente de menor a mayor.
4. Devuelve el nombre del cliente, el nombre y primer apellido de su
    representante de ventas y el número de teléfono de la oficina del representante de ventas, de aquellos clientes que no hayan realizado ningún pago.

5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
nombre y primer apellido de su representante de ventas y la ciudad donde
está su oficina.
6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
empleados que no sean representante de ventas de ningún cliente.
7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
número de empleados que tiene.