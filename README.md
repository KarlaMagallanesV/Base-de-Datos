# Creamos el script para la BD Gamarra Market

## Primero vemos nuestro  Diseño Logico 

![alt text](https://github.com/KarlaMagallanesV/Base-de-Datos/blob/main/img/dise%C3%B1o%20fisico.png)

## Ahora debemos de crear la conexion con RDS y Mysql📋

# Creacion de nuestra Base de Datos en RDS 👇
![alt text](https://github.com/KarlaMagallanesV/Base-de-Datos/blob/main/img/rds.png)

# Conexion con Mysql Workbeanch 👇

![alt text](https://github.com/KarlaMagallanesV/Base-de-Datos/blob/main/img/conexionMYSQL.png)

# Ahora empezemos con los scripts

## CREACION DE LA BD Y TABLAS
```
/* Crear base de datos dbGamarraMarket */ 
    DROP DATABASE IF EXISTS dbGamarraMarket;
    CREATE DATABASE dbGamarraMarket DEFAULT CHARACTER SET utf8;

/* Poner en uso la base de datos dbGamarraMarket */
    USE dbGamarraMarket;

/*Creacion e Interaccion de las tablas Cliente,Vendedor,Prenda,Venta y Venta_Detalle*/

/* Crear la tabla CLIENTE */

CREATE TABLE CLIENTE ( 
    id int, 
    tipo_documento char(3), 
    numero_documento char(9), 
    nombres varchar(60), 
    apellidos varchar(90), 
    email varchar(80), celular char(9), 
    fecha_nacimiento date, activo bool, CONSTRAINT cliente_pk PRIMARY KEY (id)
);

  /* Listar estructura de tabla CLIENTE */ 
    SHOW COLUMNS IN CLIENTE;

/* Listar tablas existentes en la base de datos en uso */
    SHOW TABLES;

/* Agregar columna estado civil */
    ALTER TABLE CLIENTE 
    ADD COLUMN estado_civil char(1);

/* Eliminar columna fecha_nacimiento */ 
    ALTER TABLE CLIENTE
    DROP COLUMN estado_civil;

/*Creacion de la tabla VENDEDOR*/

CREATE TABLE VENDEDOR (
    id INT PRIMARY KEY,
    tipo_documento CHAR(3) NOT NULL,
    numero_documento CHAR(15) NOT NULL UNIQUE,
    nombres VARCHAR(60) NOT NULL,
    apellidos VARCHAR(90) NOT NULL,
    salario DECIMAL(8,2) NOT NULL,
    celular CHAR(9),
    email VARCHAR(80),
    activo BOOLEAN NOT NULL
);

/* Crear la tabla PRENDA */

CREATE TABLE PRENDA (
    id INT PRIMARY KEY,
    descripcion VARCHAR(90) NOT NULL,
    marca VARCHAR(60) NOT NULL,
    cantidad INT NOT NULL,
    talla VARCHAR(10),
    precio DECIMAL(8,2) NOT NULL,
    activo BOOLEAN NOT NULL
);

/*Creacion de la tabla VENTA*/

CREATE TABLE VENTA (
    id INT PRIMARY KEY,
    fecha_hora TIMESTAMP NOT NULL,
    activo BOOLEAN NOT NULL,
    cliente_id INT NOT NULL,
    vendedor_id INT NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES CLIENTE(id),
    FOREIGN KEY (vendedor_id) REFERENCES VENDEDOR(id)
);

/*Creacion de la  tabla DETALLE_VENTA*/

CREATE TABLE VENTA_DETALLE (
    id INT PRIMARY KEY,
    cantidad INT NOT NULL,
    venta_id INT NOT NULL,
    prenda_id INT NOT NULL,
    FOREIGN KEY (venta_id) REFERENCES VENTA(id),
    FOREIGN KEY (prenda_id) REFERENCES PRENDA(id)
);

/* Listar tablas existentes en la base de datos en uso */
    SHOW TABLES;
```

## AHORA LAS RELACIONES 

```
/* Crear relación cliente_venta */
    ALTER TABLE VENTA 
    ADD CONSTRAINT cliente_venta 
    FOREIGN KEY (cliente_id) 
    REFERENCES CLIENTE (id) 
    ON UPDATE CASCADE 
    ON DELETE CASCADE;

/* Crear relación vendedor_venta */
    ALTER TABLE VENTA 
    ADD CONSTRAINT vendedor_venta 
    FOREIGN KEY (vendedor_id) 
    REFERENCES VENDEDOR (id) 
    ON UPDATE CASCADE 
    ON DELETE CASCADE;

/* Crear relación venta_a_venta_detalle */
    ALTER TABLE VENTA_DETALLE 
    ADD CONSTRAINT venta_a_venta_detalle 
    FOREIGN KEY (venta_id) 
    REFERENCES VENTA (id) 
    ON UPDATE CASCADE 
    ON DELETE CASCADE;

/* Crear relación prenda_a_venta_detalle */
    ALTER TABLE VENTA_DETALLE 
    ADD CONSTRAINT prenda_a_venta_detalle 
    FOREIGN KEY (prenda_id) 
    REFERENCES PRENDA (id) 
    ON UPDATE CASCADE 
    ON DELETE CASCADE;

/* Listar relaciones de tablas de la base de datos activa */
    SELECT  
        i.constraint_name, k.table_name, k.column_name, 
        k.referenced_table_name, k.referenced_column_name 
    FROM 
        information_schema.TABLE_CONSTRAINTS i
    LEFT JOIN
        information_schema.KEY_COLUMN_USAGE k
    ON 1.CONSTRAINT_NAME = k.CONSTRAINT_NAME
    WHERE 1.CONSTRAINT_TYPE = 'FOREIGN KEY'
    AND 1.TABLE_SCHEMA DATABASE();
```

