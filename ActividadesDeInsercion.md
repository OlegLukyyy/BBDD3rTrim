# Nivel 1: Inserción Básica e Integridad

## Actividad 1

```
 Inserta una nueva localidad llamada 'Huelva'. Asegúrate de especificar el nombre de la columna en la sentencia SQL para seguir las buenas prácticas.
```

```mysql
INSERT INTO localidades(nombre)
VALUES ('Huelva');
```

## Actividad 2

```
Registra una nueva oficina en 'Almería' con el código OFI-04. No incluyas la columna id en la lista de columnas y deja que el sistema lo gestione.
```

```mysql
INSERT INTO oficinas(ciudad, código)
VALUES ('Almería','OFI-04' );
```

## Actividad 3

```
Inserta un producto llamado 'Altavoces Bluetooth' con un precio de 55.50 y un stock de 10. Presta atención a qué valores requieren comillas y cuáles no.
```

```mysql
INSERT INTO productos(nombre, stock)
VALUES ('Altavoces Bluetooth','10' );
```

## Actividad 4

```
Inserta un nuevo empleado con tus datos personales, pero no especifiques el campo telefono. Utiliza la palabra clave DEFAULT para que el sistema asigne el valor predefinido.
```

```mysql
INSERT INTO empleados(apellido1,apellido2,email,nombre,telefono)
VALUES ('Lukianets','Oleks','luki@g.com','oleh',DEFAULT);
```

## Actividad 5

```
Crea un cliente llamado 'Suministros Industriales S.A.' y marca el campo activo como 1 (True).
```

```mysql
INSERT INTO clientes(activo,nombre )
VALUES (true,'Suministros Industriales S.A.');
```

## Actividad 6

```
Intenta insertar una oficina con el código OFI-01 (ya existente en el script de repoblado). Anota el error que devuelve MySQL.
```

```mysql
INSERT INTO oficinas(codigo)
VALUES ('OFI-04');
-- no error
```

## Actividad 7

```
Intenta insertar una dirección vinculada al id_cliente = 999. Explica por qué el motor de la base de datos bloquea esta operación.
```

```mysql
-- La acción se bloquea debido a que no existe usuario con ese id
```

# Nivel 2: Eficiencia e Inserción Múltiple

## Actividad 8

```
Inserta en una sola sentencia SQL tres nuevas localidades: 'Granada', 'Córdoba' y 'Jaén'.
```

```mysql
INSERT INTO localidades(nombre)
VALUES ('Granada'),
		('Córdoba'),
        ('Jaén');
```

## Actividad 9

```
Registra de forma masiva (en una sola sentencia) cuatro productos de papelería: 'Papel A4', 'Bolígrafo Azul', 'Grapadora' y 'Archivador'. Asigna precios variados y usa DEFAULT para el stock.
```

```mysql
INSERT INTO productos(nombre,precio,stock)
VALUES ('Papel A4',10,DEFAULT),
		('Bolígrafo Azul',8,DEFAULT),
        ('Grapadora',3,DEFAULT),
        ('Archivador',1,DEFAULT);
```

## Actividad 10

```
La empresa abre dos sedes en Algeciras. Inserta en una sola consulta las oficinas con códigos OFI-AL1 y OFI-AL2.
```

```mysql
INSERT INTO oficinas(ciudad,codigo)
VALUES ('Algeciras','OFI-AL1'),
		('Algeciras','OFI-AL2');
```

## Actividad 11

```
Intenta una inserción múltiple de 3 empleados donde el segundo tenga un email duplicado. Comprueba si se ha llegado a insertar el primer registro o si la sentencia ha fallado por completo.
```

```mysql
Se cancela por completo, actua como transacion.
```

## Actividad 12

```
Inserta en una única sentencia 3 direcciones diferentes para el Cliente 1 ('Empresa A'), variando la calle y la localidad en cada una.
```

```mysql
INSERT INTO direcciones(cp,direccion,idCliente,idLocalidad)
	VALUES(11202,'Avenida gatos', 1, 1),
    		(11701,'Avenida patos', 1, 2),
            (21701,'Avenida perros', 1, 3)
```

## Actividad 13

```
Asigna a la empleada 'Gema Díaz' (ID 1) a la oficina 'OFI-02' con fecha de inicio de hoy y la fecha de fin como NULL.
```

```mysql

```
