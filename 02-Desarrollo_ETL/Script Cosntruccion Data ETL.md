```sql
-- Crear la base de datos de Stage_Northwind

CREATE DATABASE stage_northwind

-- Crear la base de datos Datamart_Northwind

CREATE DATABASE datamart_northwind


-- Implementar la base de datos de stage_northwind

USE stage_northwind

CREATE TABLE Categorias(
	categoriaid INT NOT NULL,
	nombrecategoria VARCHAR (15)
	)

CREATE TABLE Clientes(
	clienteid CHAR (5) NOT NULL,
	compania VARCHAR (40) NOT NULL,
	ciudad VARCHAR (15),
	region VARCHAR (15),
	codigopostal CHAR (10),
	pais NVARCHAR (15)
	)

CREATE TABLE Empleados(
	empleadoid INT NOT NULL,
	nombre VARCHAR (10) NOT NULL,
	apellido VARCHAR (20) NOT NULL,
	fecha_contratacion DATE
	)

CREATE TABLE Producto(
	productoid INT NOT NULL,
	nombre_producto VARCHAR (50) NOT NULL,
	precio_unitario DECIMAL (15,2) NOT NULL,
	)

CREATE TABLE Proveedor(
	proveedorid INT NOT NULL,
	poeveedor_nombre VARCHAR (40) NOT NULL,
	ciudad VARCHAR (15),
	region VARCHAR (15),
	pais VARCHAR (15)
	)

CREATE TABLE Ventas(
	clienteid CHAR (5) NOT NULL,
	empleado_codigo INT NOT NULL,
	producto_codigo INT NOT NULL,
	ventas_orden DATETIME NOT NULL,
	ventas_monto DECIMAL (15,2) NOT NULL,
	ventas_unidades INT NOT NULL,
	ventas_precio_unitario DECIMAL (15,2),
	ventas_descuento DECIMAL (15,2)
	)

-- Crear el datamartNorthwind

USE datamart_northwind

//*CREATE TABLE dim_Cliente(
	cliente_Skey INT NOT NULL IDENTITY (1,1),
	cliente_codigoBKey CHAR (5) NOT NULL,
	cliente_compania VARCHAR (40) NOT NULL,
	cliente_ciudad VARCHAR (15) NOT NULL,
	cliente_region VARCHAR (25) NOT NULL,
	cliente_pais VARCHAR (15) NOT NULL
	)
*//

DROP TABLE dim_Cliente

CREATE TABLE dim_Cliente(
	cliente_Skey INT NOT NULL IDENTITY (1,1),
	cliente_codigoBKey CHAR (5) NOT NULL,
	cliente_compania VARCHAR (40) NOT NULL,
	cliente_ciudad VARCHAR (15) NOT NULL,
	cliente_region VARCHAR (25) NOT NULL,
	cliente_pais VARCHAR (15) NOT NULL,
	CONSTRAINT pk_dimcliente
	PRIMARY KEY (cliente_Skey)
	)

CREATE TABLE dim_Empleado(
	empleado_Skey INT NOT NULL IDENTITY (1,1),
	empleado_codigoBKey INT NOT NULL,
	empleado_nombreCompleto VARCHAR (100) NOT NULL
	CONSTRAINT pk_dimempleado
	PRIMARY KEY (empleado_SKey)
	)

CREATE TABLE dim_Producto(
	producto_SKey INT NOT NULL IDENTITY (1,1),
	prodcuto_codigoBKey INT NOT NULL,
	produto_nombre VARCHAR (80) NOT NULL,
	prodcuto_categoriaNombre VARCHAR (15) NOT NULL
	CONSTRAINT pk_dimprodcuto
	PRIMARY KEY (producto_SKey)
	)

CREATE TABLE dim_Tiempo(
	tiempo_SKey INT NOT NULL IDENTITY (1,1),
	tiempo_fechaActual DATETIME NOT NULL,
	tiempo_anio INT NOT NULL,
	tiempo_trimestre INT NOT NULL,
	tiempo_mes INT NOT NULL,
	tiempo_semana INT NOT NULL,
	tiempo_diadeAnio INT NOT NULL,
	tiempo_diadeMes INT NOT NULL,
	tiempo_diaSemana INT NOT NULL
	CONSTRAINT pk_dimtiempo
	PRIMARY KEY (tiempo_SKey)
	)

CREATE TABLE fact_Ventas(
	cliente_SKey INT NOT NULL,
	empleado_SKey INT NOT NULL,
	producto_SKey INT NOT NULL,
	tiempo_SKey INT NOT NULL,
	ventas_nOrden INT NOT NULL,
	ventas_monto DECIMAL (15,2) NOT NULL,
	ventas_unidades INT NOT NULL,
	ventas_pUnitario DECIMAL (15,2) NOT NULL,
	ventas_descuento DECIMAL (15,2) NOT NULL
	CONSTRAINT pk_factVentas
	PRIMARY KEY (cliente_SKey, empleado_SKey, producto_SKey, tiempo_SKey),
	CONSTRAINT fk_fact_Ventas_dimcliente
	FOREIGN KEY (cliente_SKey)
	REFERENCES dim_Cliente(cliente_SKey),
	CONSTRAINT fk_fact_Ventas_dimempleado
	FOREIGN KEY (empleado_SKey)
	REFERENCES dim_Empleado(empleado_SKey),
	CONSTRAINT fk_fact_Ventas_dimproducto
	FOREIGN KEY (producto_SKey)
	REFERENCES dim_Producto(producto_SKey),
	CONSTRAINT fk_fact_Ventas_dimtiempo
	FOREIGN KEY (tiempo_SKey)
	REFERENCES dim_Tiempo(tiempo_SKey)
	)
	```