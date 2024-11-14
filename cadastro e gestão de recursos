CREATE DATABASE ecommercer;

use ecommercer;

CREATE TABLE categoria(
	id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50) NOT NULL UNIQUE,
    descrição TEXT
);

CREATE TABLE produtos(
	id INT AUTO_INCREMENT PRIMARY KEY,
	nome VARCHAR(100) NOT NULL,
	descrição TEXT,
	preço DECIMAL(10, 2) NOT NULL,
	categoria_id INT, 
	FOREIGN KEY (categoria_id) REFERENCES categoria(id) ON DELETE SET NULL ON UPDATE CASCADE
);

CREATE TABLE estoque(
	produto_id INT PRIMARY KEY,
	quantidade INT DEFAULT 0,
    FOREIGN KEY (produto_id) REFERENCES produtos(id) ON DELETE CASCADE ON UPDATE CASCADE
);

DELIMITER //
CREATE TRIGGER add_estoque AFTER INSERT ON produtos
FOR EACH ROW
BEGIN
	INSERT INTO estoque (produto_id, quantidade) VALUES (NEW.id, 0);
END//

DELIMITER //
CREATE TRIGGER decrementar_estoque AFTER INSERT ON pedidos_produtos 
FOR EACH ROW 
BEGIN
	UPDATE estoque SET quantidade = quantidade - NEW.quantidade
    WHERE produto_id = NEW.produto_id;
END//


DELIMITER //
CREATE PROCEDURE adicionar_produto(
	IN nome_produto VARCHAR(100),
    IN descrição_produto TEXT,
    IN preço_produto DECIMAL(10, 2),
    IN categoria INT,
    IN quantidade_estoque INT
)
BEGIN 
	DECLARE prod_id INT;
    
    INSERT INTO produtos (nome, descrição, preço, categoria_id)
    VALUES (nome_produto, descrição_produto, preço_produto, categoria);
    
    SET prod_id = LAST_INSERT_ID();
    
    UPDATE estoque SET quantidade = quantidade_estoque WHERE produto_id = prod_id;
END //
DELIMITER ;

DELIMITER //
CREATE PROCEDURE atualizar_produto(
	IN produto_id INT,
    IN novo_nome VARCHAR(100),
	IN nova_descriçao TEXT,
    IN novo_preço DECIMAL(10, 2),
    IN nova_categoria INT
)
BEGIN 
	UPDATE produtos
    SET nome = novo_nome, descrição = nova_descrição, preço = novo_preço, categoria_id = nova_categoria
    WHERE id = produto_id;
END //
DELIMITER ;

SELECT 
	p.id AS produto_id,
    p.nome As nome_produto,
    p.descrição,
    p.preço,
    c.nome AS cateogoria, 
    e.quantidade AS estoque_disponivel
FROM 
	produtos p 
LEFT JOIN
	categorias c ON p.categoria_id = c.id
LEFT JOIN
	estoque e ON p.id = e.produto_id;
    
SELECT
	p.nome AS produto,
    e.quantidade AS estoque_disponivel
FROM
	produtos p
JOIN 
	estoque e ON p.id = e.produto_id
WHERE
	e.quantidade < 10;
