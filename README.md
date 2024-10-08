cREATE DATABASE galletitas
use galletitas
CREATE TABLE Clientes (
    dni VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    direccion VARCHAR(100),
    numero_telefono VARCHAR(15),
    correo VARCHAR(50)
);

CREATE TABLE Caja (
    id INT PRIMARY KEY,
    contenido VARCHAR(255),
    id_lote INT,
    dni_cliente VARCHAR(20),
    id_pj INT,
    FOREIGN KEY (id_lote) REFERENCES Lote(id),
    FOREIGN KEY (dni_cliente) REFERENCES Clientes(dni)
);

CREATE TABLE Lote (
    id INT PRIMARY KEY,
    pais VARCHAR(50),
    estado VARCHAR(50)
);

CREATE TABLE Vehiculo (
    patente VARCHAR(20) PRIMARY KEY,
    tipo VARCHAR(50),
    capacidad INT,
    estado VARCHAR(50)
);

CREATE TABLE Empleado (
    dni VARCHAR(20) PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50)
);

INSERT INTO Clientes (dni, nombre, apellido, direccion, numero_telefono, correo) 
VALUES 
('12345678', 'Carlos', 'Pérez', 'Calle Falsa 123', '123456789', 'estoylol@gmail.com'),
('87654321', 'Ana', 'Gómez', 'Avenida Siempre Viva 742', '987654321', 'anhre@hotmail.com');

INSERT INTO Lote (id, pais, estado) 
VALUES 
(1, 'Argentina', 'Disponible'),
(2, 'Brasil', 'En tránsito');

INSERT INTO Caja (id, contenido, id_lote, dni_cliente, id_pj) 
VALUES 
(1, 'Galletas de chocolate', 1, '12345678', 101),
(2, 'Galletas de avena', 2, '87654321', 102);

INSERT INTO Vehiculo (patente, tipo, capacidad, estado) 
VALUES 
('ABC123', 'Camión', 1000, 'Disponible'),
('XYZ789', 'Camioneta', 500, 'En reparación');

INSERT INTO Empleado (dni, nombre, apellido) 
VALUES 
('45678901', 'Luis', 'Martínez'),
('65432109', 'María', 'Fernández');
