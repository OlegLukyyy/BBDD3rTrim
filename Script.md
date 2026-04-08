-- ======================================================
-- SCRIPT ACTUALIZADO PARA UD06: GESTIÓN DE INTEGRIDAD Y TRANSACCIONES
-- ======================================================

DROP DATABASE IF EXISTS nexus;
CREATE DATABASE nexus CHARACTER SET utf8mb4;
USE nexus;

-- 1. ENTIDADES MAESTRAS (Plural, campos singular, id autonumérico)
CREATE TABLE localidades (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(100) NOT NULL
) ENGINE=InnoDB;

CREATE TABLE oficinas (
id INT AUTO_INCREMENT PRIMARY KEY,
codigo VARCHAR(10) NOT NULL UNIQUE,
ciudad VARCHAR(60) NOT NULL
) ENGINE=InnoDB;

CREATE TABLE empleados (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(50) NOT NULL,
apellido1 VARCHAR(50) NOT NULL,
apellido2 VARCHAR(50),
email VARCHAR(100) NOT NULL UNIQUE,
telefono VARCHAR(20)
) ENGINE=InnoDB;

CREATE TABLE productos (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(120) NOT NULL,
precioActual DECIMAL(10,2) NOT NULL,
stock INT NOT NULL DEFAULT 0
) ENGINE=InnoDB;

-- 2. CLIENTES Y DIRECCIONES
CREATE TABLE clientes (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(100) NOT NULL,
activo BOOLEAN DEFAULT TRUE,
idDireccionFacturacion INT, -- Nombre FK: id + Entidad
idDireccionEntrega INT
) ENGINE=InnoDB;

CREATE TABLE direcciones (
id INT AUTO_INCREMENT PRIMARY KEY,
idCliente INT NOT NULL, -- Nombre FK: id + Entidad
idLocalidad INT NOT NULL,
direccion VARCHAR(150) NOT NULL,
cp VARCHAR(10),
CONSTRAINT fk_direcciones_clientes FOREIGN KEY (idCliente) REFERENCES clientes(id) ON DELETE CASCADE,
CONSTRAINT fk_direcciones_localidades FOREIGN KEY (idLocalidad) REFERENCES localidades(id)
) ENGINE=InnoDB;

ALTER TABLE clientes ADD CONSTRAINT fk_cli_fact FOREIGN KEY (idDireccionFacturacion) REFERENCES direcciones(id);
ALTER TABLE clientes ADD CONSTRAINT fk_cli_entr FOREIGN KEY (idDireccionEntrega) REFERENCES direcciones(id);

-- 3. RELACIÓN: empleados-oficinas (Sin id propio, usa índices)
-- Usamos comillas invertidas para permitir el guion medio en el nombre
CREATE TABLE `empleados-oficinas` (
idEmpleado INT NOT NULL,
idOficina INT NOT NULL,
fechaInicio DATE NOT NULL,
fechaFin DATE,
PRIMARY KEY (idEmpleado, idOficina, fechaInicio),
CONSTRAINT fk_eo_empleados FOREIGN KEY (idEmpleado) REFERENCES empleados(id),
CONSTRAINT fk_eo_oficinas FOREIGN KEY (idOficina) REFERENCES oficinas(id)
) ENGINE=InnoDB;

-- 4. ENTIDADES DE VENTA
CREATE TABLE pedidos (
id INT AUTO_INCREMENT PRIMARY KEY,
idCliente INT NOT NULL,
numPedido INT NOT NULL,
fecha DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
idEmpleado INT NOT NULL,
idDireccionFacturacion INT NOT NULL,
idDireccionEntrega INT,
CONSTRAINT fk_ped_cli FOREIGN KEY (idCliente) REFERENCES clientes(id),
CONSTRAINT fk_ped_emp FOREIGN KEY (idEmpleado) REFERENCES empleados(id),
CONSTRAINT fk_ped_fact FOREIGN KEY (idDireccionFacturacion) REFERENCES direcciones(id),
CONSTRAINT fk_ped_entr FOREIGN KEY (idDireccionEntrega) REFERENCES direcciones(id)
) ENGINE=InnoDB;

-- 5. RELACIÓN: detalles-pedidos (Sin id propio, PK compuesta)
CREATE TABLE `detalles-pedidos` (
idPedido INT NOT NULL,
idProducto INT NOT NULL,
cantidad INT NOT NULL,
precioUnidad DECIMAL(10,2) NOT NULL,
PRIMARY KEY (idPedido, idProducto),
CONSTRAINT fk_det_ped FOREIGN KEY (idPedido) REFERENCES pedidos(id) ON DELETE CASCADE,
CONSTRAINT fk_det_prod FOREIGN KEY (idProducto) REFERENCES productos(id)
) ENGINE=InnoDB;

-- 6. ENTIDADES LOGÍSTICAS Y FINANCIERAS
CREATE TABLE pagos (
id INT AUTO_INCREMENT PRIMARY KEY,
idPedido INT NOT NULL,
cantidad DECIMAL(12,2) NOT NULL,
fecha DATETIME NOT NULL,
CONSTRAINT fk_pag_ped FOREIGN KEY (idPedido) REFERENCES pedidos(id)
) ENGINE=InnoDB;

CREATE TABLE entregas (
id INT AUTO_INCREMENT PRIMARY KEY,
fechaEntrega DATETIME,
idPedido INT NOT NULL,
idOficinaEntrega INT NOT NULL,
CONSTRAINT fk_ent_ped FOREIGN KEY (idPedido) REFERENCES pedidos(id),
CONSTRAINT fk_ent_ofi FOREIGN KEY (idOficinaEntrega) REFERENCES oficinas(id)
) ENGINE=InnoDB;

-- 7. RELACIÓN: entrega-productos (Sin id propio, PK compuesta)
CREATE TABLE `entrega-productos` (
idEntrega INT NOT NULL,
idProducto INT NOT NULL,
PRIMARY KEY (idEntrega, idProducto),
CONSTRAINT fk_ep_entregas FOREIGN KEY (idEntrega) REFERENCES entregas(id) ON DELETE CASCADE,
CONSTRAINT fk_ep_productos FOREIGN KEY (idProducto) REFERENCES productos(id)
) ENGINE=InnoDB;

-- 8. ENTIDAD TÉCNICA: auditoria-sistema
CREATE TABLE `auditoria-sistema` (
id INT AUTO_INCREMENT PRIMARY KEY,
fechaHora TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
usuario VARCHAR(50),
tablaAfectada VARCHAR(50),
accion TEXT
) ENGINE=InnoDB;
