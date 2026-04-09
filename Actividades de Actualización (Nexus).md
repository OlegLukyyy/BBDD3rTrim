# Nivel 1: Actualización Básica y Cálculos
## Actividad 1
```
El empleado con id = 1 ha actualizado sus apellidos. Cambia su apellido1 a 'Díaz-Valerio'.
```
```mysql
UPDATE empleados 
SET apellido1='Díaz-Valerio'
WHERE id=1;
```
## Actividad 2
```
Debido al aumento de costes, incrementa el precioActual de todos los productos en un 5%.
```
```mysql
UPDATE productos  
SET precioActual=precioActual+precioActual*0.05
```
## Actividad 3
```
Actualiza la tabla empleados para asegurar que todos los correos electrónicos estén en minúsculas utilizando la función LOWER().
```
```mysql
UPDATE empleados 
SET email=LOWER(email);
```
## Actividad 4
```
Modifica el producto con id = 2. Cambia su nombre a 'Ratón Óptico Pro' y aumenta su stock en 50 unidades en una sola sentencia SQL.
```
```mysql
UPDATE productos
SET nombre='Ratón Óptico Pro'
	AND stock=50
WHERE id=2;
```
## Actividad 5
```
Actualiza la tabla productos para que el precioActual se incremente en 2.00€ fijos a modo de gastos de gestión para todos los registros.
```
```mysql
UPDATE productos 
SET precioActual=precioActual +2
```
# Nivel 2: Filtros Avanzados (WHERE)
## Actividad 6
```
Los productos con id 1, 3 y 5 están en promoción. Reduce su precioActual un 15% utilizando la cláusula IN.
```
```mysql
UPDATE productos 
SET precioActual = precioActual*0.8
WHERE id IN (1,3,5)
```
## Actividad 7
```
Incrementa en 10 unidades el stock de todos los productos cuyo precioActual esté entre 20.00€ y 100.00€.
```
```mysql
UPDATE productos
SET stock= stock+10
WHERE precioActual BETWEEN 20 AND 100;
```
## Actividad 8
```
Localiza las direcciones que contienen la palabra 'Calle' y actualízalas para que utilicen la abreviatura 'C/' mediante la función REPLACE().
```
```mysql
UPDATE direcciones
SET direccion = REPLACE (direccion,'Calle','C/');
```
## Actividad 9
```
En la tabla de relación empleados-oficinas, busca los registros donde fechaFin sea NULL y asígnales la fecha '2026-12-31'.
```
```mysql
UPDATE empleadosoficinas
SET fechaFin='2026-12-31'
WHERE fechaFIN IS NULL;
```
## Actividad 10
```
(Solo MySQL) Ordena los productos por stock de forma ascendente y actualiza los 3 productos con menos existencias duplicando su precio.
```
```mysql
UPDATE productos 
SET precioActual=precioActual*2
ORDER BY stock ASC
LIMIT 3;
```
# Nivel 3: Lógica de Negocio y Relaciones

## Actividad 11
```
Actualiza el precioUnidad en la tabla detalles-pedidos para que coincida con el precioActual de la tabla productos, pero solo para el pedido con id = 50.
```
```mysql
UPDATE detallespedidos dp
INNER JOIN productos pr ON pr.id=dp.idProducto 
SET dp.precioUnidad=pr.precioActual
WHERE id=50;
```
## Actividad 12
```
El Cliente 1 ha solicitado que todos sus pedidos se envíen a su dirección de facturación. Actualiza idDireccionEntrega para que sea igual a idDireccionFacturacion en todos los pedidos del Cliente 1.
```
```mysql
UPDATE pedidos pe
SET pe.idDireccionEntrega=pe.idDireccionFacturacion
WHERE pe.idCliente=1;
```
## Actividad 13
```
Cambia el estado de los clientes a activo = 1 si han realizado algún pedido con fecha posterior al 1 de marzo de 2025.
```
```mysql
UPDATE clientes cl
SET cl.activo=1 
WHERE EXISTS (SELECT 1 FROM pedidos pe where pe.fecha>'2025-03-01' AND pe.idCliente=cl.id);
```
## Actividad 14 (Por terminar)
```
Actualiza la fechaEntrega en la tabla entregas sumándole 2 días a todos los envíos que tengan como destino la 'Oficina 3' (usa su idOficinaEntrega).
```
```mysql
UPDATE entregas en
INNER JOIN pedidos pe ON pe.id=en.idPedido
SET en.fechaEntrega=ADDDATE(en.fechaEntrega,2)
WHERE pe.idDireccionEntrega='Oficina 3'
```
