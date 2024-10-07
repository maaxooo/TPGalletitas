CREATE DATABASE paintball_experience;
USE paintball_experience;

CREATE TABLE Clientes (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(100),
    telefono VARCHAR(15)
);

CREATE TABLE Pedidos (
    id_pedido INT AUTO_INCREMENT PRIMARY KEY,
    id_cliente INT,
    fecha_pedido DATE,
    total DECIMAL(10,2),
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
);

CREATE TABLE Trabajadores (
    id_trabajador INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100),
    puesto VARCHAR(50)
);

CREATE TABLE Empaques (
    id_empaque INT AUTO_INCREMENT PRIMARY KEY,
    id_pedido INT,
    id_trabajador INT,
    fecha_empaque DATE,
    FOREIGN KEY (id_pedido) REFERENCES Pedidos(id_pedido),
    FOREIGN KEY (id_trabajador) REFERENCES Trabajadores(id_trabajador)
);

-- Triggers

CREATE TRIGGER actualizar_total_pedido
AFTER INSERT ON Pedidos
FOR EACH ROW
BEGIN
    UPDATE Pedidos SET total = (SELECT SUM(precio) FROM Productos WHERE id_pedido = NEW.id_pedido) WHERE id_pedido = NEW.id_pedido;
END;

CREATE TRIGGER verificar_informacion_cliente
BEFORE INSERT ON Clientes
FOR EACH ROW
BEGIN
    IF NEW.nombre = '' THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'El nombre del cliente no puede estar vacío';
    END IF;
END;

CREATE TRIGGER registrar_empaque
AFTER INSERT ON Empaques
FOR EACH ROW
BEGIN
    INSERT INTO LogEmpaques (id_pedido, id_trabajador, fecha_empaque) VALUES (NEW.id_pedido, NEW.id_trabajador, NEW.fecha_empaque);
END;

CREATE TRIGGER borrar_cliente
BEFORE DELETE ON Clientes
FOR EACH ROW
BEGIN
    DELETE FROM Pedidos WHERE id_cliente = OLD.id_cliente;
END;

-- Stored Procedures

CREATE PROCEDURE agregar_cliente(IN nombre_cliente VARCHAR(100), IN email_cliente VARCHAR(100), IN telefono_cliente VARCHAR(15))
BEGIN
    INSERT INTO Clientes (nombre, email, telefono) VALUES (nombre_cliente, email_cliente, telefono_cliente);
END;

CREATE PROCEDURE agregar_pedido(IN cliente_id INT, IN fecha DATE, IN total DECIMAL(10, 2))
BEGIN
    INSERT INTO Pedidos (id_cliente, fecha_pedido, total) VALUES (cliente_id, fecha, total);
END;

CREATE PROCEDURE agregar_trabajador(IN nombre_trabajador VARCHAR(100), IN puesto_trabajador VARCHAR(50))
BEGIN
    INSERT INTO Trabajadores (nombre, puesto) VALUES (nombre_trabajador, puesto_trabajador);
END;

CREATE PROCEDURE obtener_pedidos_por_cliente(IN cliente_id INT)
BEGIN
    SELECT * FROM Pedidos WHERE id_cliente = cliente_id;
END;

CREATE PROCEDURE actualizar_total(IN pedido_id INT, IN nuevo_total DECIMAL(10, 2))
BEGIN
    UPDATE Pedidos SET total = nuevo_total WHERE id_pedido = pedido_id;
END;

