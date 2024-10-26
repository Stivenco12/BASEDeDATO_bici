# BASEDeDATO_bici

``` sql

CREATE DATABASE bicicletass;

use bicicletass;

CREATE TABLE país(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100)
);

CREATE TABLE region(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100)
);

ALTER TABLE region
  ADD idpais_fk INT(11),
  ADD CONSTRAINT FK_PaisRegion FOREIGN KEY (idpais_fk) REFERENCES país(id);

CREATE TABLE ciudad(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  idregion_fk INT(11),
  CONSTRAINT FK_Region FOREIGN KEY (idregion_fk) REFERENCES region(id)
);

ALTER TABLE region
  DROP FOREIGN KEY FK_PaisRegion;

ALTER TABLE region
  DROP COLUMN idpais_fk;

ALTER TABLE ciudad
  DROP FOREIGN KEY FK_Region;

ALTER TABLE ciudad
  DROP COLUMN idregion_fk;

ALTER TABLE país
  ADD idregion_fk INT(11),
  ADD CONSTRAINT FK_Region FOREIGN KEY (idregion_fk) REFERENCES region(id);

ALTER TABLE region
  ADD idciudad_fk INT(11),
  ADD CONSTRAINT FK_Ciudad FOREIGN KEY (idciudad_fk) REFERENCES ciudad(id);

CREATE TABLE surcursales(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  Direccion VARCHAR(100),
  idciudad_fk INT(11),
  CONSTRAINT FK_Ciudadsucursales FOREIGN KEY (idciudad_fk) REFERENCES ciudad(id)
);

ALTER TABLE ciudad
  add idsucursales_fk INT(11),
  add CONSTRAINT FK_SucursalesCiudad FOREIGN KEY (idsucursales_fk) REFERENCES surcursales(id);

CREATE TABLE total_ventas(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Total decimal(10,2),
  Fecha date,
  idsucursales_fk INT(11),
  CONSTRAINT FK_SucursalesVentas FOREIGN KEY (idsucursales_fk) REFERENCES surcursales(id)
);

CREATE TABLE sector_ventas(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  idsucursales_fk INT(11),
  CONSTRAINT FK_SucursalesSector FOREIGN KEY (idsucursales_fk) REFERENCES surcursales(id)
);

CREATE TABLE empleados(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  cargo VARCHAR(50),
  telefono VARCHAR(13),
  email VARCHAR(100)
);

ALTER TABLE empleados
  add idsucursales_fk INT(11),
  add CONSTRAINT FK_SucursalesEmpleados FOREIGN KEY (idsucursales_fk) REFERENCES surcursales(id);

CREATE TABLE proveedores(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  Telefono VARCHAR(13),
  direccion VARCHAR(255),
  email VARCHAR(100),
  idpais_fk INT(11),
  CONSTRAINT FK_ProveedoresPais FOREIGN KEY (idpais_fk) REFERENCES país(id)
);

CREATE TABLE repuesto(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  precio DECIMAL(10,2),
  idproveedor_fk INT(11),
  CONSTRAINT FK_ProveedoresRepuesto FOREIGN KEY (idproveedor_fk) REFERENCES proveedores(id)
);

CREATE TABLE cuentas_saldar(
  id INT AUTO_INCREMENT PRIMARY KEY,
  monto DECIMAL(10,2),
  fecha_vencimiento DATE,
  estado VARCHAR(50),
  idproveedor_fk INT(11),
  CONSTRAINT FK_ProveedoresSaldar FOREIGN KEY (idproveedor_fk) REFERENCES proveedores(id)
);

CREATE TABLE productos(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(100),
  Categoria VARCHAR(100),
  precio DECIMAL(10,2),
  Stock int,
  idproveedor_fk INT(11),
  CONSTRAINT FK_ProveedoresProductos FOREIGN KEY (idproveedor_fk) REFERENCES proveedores(id)
);

CREATE TABLE bicicleta(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Marca VARCHAR(100),
  Tipo VARCHAR(50),
  Valor DECIMAL(10,2),
  idproveedor_fk INT(11),
  CONSTRAINT FK_ProveedoresBicicleta FOREIGN KEY (idproveedor_fk) REFERENCES proveedores(id)
);

CREATE TABLE stock(
  id INT AUTO_INCREMENT PRIMARY KEY,
  Cantidad int,
  idProductos_fk int(11),
  CONSTRAINT FK_ProductosStock FOREIGN KEY (idProductos_fk) REFERENCES productos(id),
  idSucursal_fk int(11),
  CONSTRAINT FK_SucursalStock FOREIGN KEY (idSucursal_fk) REFERENCES surcursales(id)
);



CREATE TABLE Estado(
  ransito VARCHAR(30),
  entrega VARCHAR(30),
  entregado VARCHAR(30),
  id INT AUTO_INCREMENT PRIMARY KEY,
  idProductos_fk int(11),
  CONSTRAINT FK_ProductosStado FOREIGN KEY (idProductos_fk) REFERENCES productos(id)
);

DROP TABLE Clientes;

CREATE TABLE Clientes(
  id_clientes INT AUTO_INCREMENT PRIMARY KEY,
  Nombre VARCHAR(50),
  direccion VARCHAR(50),
  Telefono VARCHAR(13),
  Email VARCHAR(150),
  idpais_fk INT,
  CONSTRAINT FK_ClientePais FOREIGN KEY (idpais_fk) REFERENCES `país`(id)
);


CREATE TABLE Pedidos(
  id_pedido INT AUTO_INCREMENT PRIMARY KEY,
  Cantidad INT(11),
  fecha DATE,
  idEstado_fk INT(11),
  CONSTRAINT FK_PedidosEstados FOREIGN KEY (idEstado_fk) REFERENCES Estado(id),
  idCliente_fk INT(11),
  CONSTRAINT FK_PedidosClientes FOREIGN KEY (IdCliente_fk) REFERENCES Clientes(id_clientes),
  idProducto_fk INT(11),
  CONSTRAINT FK_PedidosProducto FOREIGN KEY (idProducto_fk) REFERENCES productos(id)
);

CREATE TABLE Fecha(
  Id_fecha INT AUTO_INCREMENT PRIMARY KEY,
  Fecha DATE,
  Descripsion VARCHAR(50)
);

CREATE TABLE Venta (
  Id_Venta INT AUTO_INCREMENT PRIMARY KEY,
  IdFecha_fk INT(11),
  CONSTRAINT FK_VentaFecha FOREIGN KEY (IdFecha_fk) REFERENCES Fecha(Id_fecha),
  Cantidad INT,
  Total DECIMAL(20,2),
  IdCliente_fk INT(11),
  CONSTRAINT FK_VentaCliente FOREIGN KEY (IdCliente_fk) REFERENCES Clientes(id_clientes),
  IdBici_fk INT(11),
  IdEmpleado_fk INT(11),
  CONSTRAINT FK_VentaEmpleado FOREIGN KEY (IdEmpleado_fk) REFERENCES empleados(id)
);

CREATE TABLE Pago(
  Id_Pago INT AUTO_INCREMENT PRIMARY KEY,
  Tipo_pago VARCHAR(50),
  Monto Decimal(20,2),
  Fecha DATE,
  IdVenta_fk INT(11),
  CONSTRAINT FK_PagoVenta FOREIGN KEY (IdVenta_fk) REFERENCES Venta(Id_Venta)
);

CREATE TABLE Facturacion(
  Id_Factura INT AUTO_INCREMENT PRIMARY KEY,
  Total INT(200),
  IdVenta_fk INT(11),
  CONSTRAINT FK_FacturaVenta FOREIGN KEY (IdVenta_fk) REFERENCES Venta(Id_Venta)
);

