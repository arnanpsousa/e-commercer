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