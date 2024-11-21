CREATE DATABASE e_commerce;

USE e_commerce;
CREATE TABLE categorias (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE
);

CREATE TABLE produtos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);

CREATE TABLE estoque (
    produto_id INT PRIMARY KEY,
    quantidade INT NOT NULL,
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);

DELIMITER //
CREATE TRIGGER atualizar_estoque AFTER INSERT ON pedidos_produtos
FOR EACH ROW
BEGIN
    UPDATE estoque 
    SET quantidade = quantidade - NEW.quantidade
    WHERE produto_id = NEW.produto_id;
END //
DELIMITER ;


CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL
);

CREATE TABLE pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    data_pedido DATETIME DEFAULT CURRENT_TIMESTAMP,
    status VARCHAR(20) DEFAULT 'Pendente',
    valor_total DECIMAL(10, 2),
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE pedidos_produtos (
    pedido_id INT,
    produto_id INT,
    quantidade INT,
    PRIMARY KEY (pedido_id, produto_id),
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);

DELIMITER //
CREATE FUNCTION calcular_frete(valor_total DECIMAL(10, 2))
RETURNS DECIMAL(10, 2)
BEGIN
    DECLARE frete DECIMAL(10, 2);
    IF valor_total > 100 THEN
        SET frete = 0;
    ELSE
        SET frete = 10;
    END IF;
    RETURN frete;
END //
DELIMITER ;
