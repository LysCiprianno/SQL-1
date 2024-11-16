# Explorando o Banco de Dados da Empresa "Momento" com Consultas SQL

Vamos embarcar juntos nessa jornada para explorar o banco de dados da nossa renomada empresa "Momento" com consultas SQL que revelam todos os detalhes importantes. Pronto para uma aventura empresarial e filosófica? 📈📙

-- Criação das Tabelas
 
-- Tabela de Funcionários
CREATE TABLE funcionarios (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    cargo VARCHAR(50),
    salario DECIMAL(10, 2),
    data_contratacao DATE,
    departamento_id INT,
    cônjuge BOOLEAN
);
 
-- Tabela de Departamentos
CREATE TABLE departamentos (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    localizacao VARCHAR(100)
);
 
-- Tabela de Produtos
CREATE TABLE produtos (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    valor_unitario DECIMAL(10, 2),
    quantidade INT
);
 
-- Tabela de Escritórios
CREATE TABLE escritorios (
    id INT PRIMARY KEY,
    localizacao VARCHAR(100),
    custo_suprimentos DECIMAL(10, 2)
);
 
-- Tabela de Regiões
CREATE TABLE regioes (
    id INT PRIMARY KEY,
    nome VARCHAR(100)
);
 
-- Relacionamento Escritórios e Regiões
CREATE TABLE regioes_escritorios (
    escritorio_id INT,
    regiao_id INT,
    FOREIGN KEY (escritorio_id) REFERENCES escritorios(id),
    FOREIGN KEY (regiao_id) REFERENCES regioes(id)
);
 
-- Inserção de Departamentos
INSERT INTO departamentos (id, nome, localizacao)
VALUES
(1, 'Tecnologia', 'São Paulo'),
(2, 'Vendas', 'Rio de Janeiro'),
(3, 'Inovações', 'Brasil');
 
-- Inserção de Funcionários
INSERT INTO funcionarios (id, nome, cargo, salario, data_contratacao, departamento_id, cônjuge)
VALUES
(1, 'Sócrates', 'Filósofo', 5000, '2000-01-01', 1, TRUE),
(2, 'Platão', 'Filósofo', 5500, '2005-01-01', 1, TRUE),
(3, 'Aristóteles', 'Filósofo', 6000, '2010-01-01', 1, FALSE),
(4, 'Agostinho de Hipona', 'Filósofo', 4500, '2015-01-01', 2, TRUE),
(5, 'René Descartes', 'Filósofo', 7000, '2020-01-01', 2, FALSE),
(6, 'Immanuel Kant', 'Filósofo', 8000, '2022-01-01', 3, TRUE);
 
-- Inserção de Produtos
INSERT INTO produtos (id, nome, valor_unitario, quantidade)
VALUES
(1, 'Filosofia de Sócrates', 150.00, 50),
(2, 'Teoria das Ideias de Platão', 180.00, 30),
(3, 'Metafísica de Aristóteles', 200.00, 20);
 
-- Inserção de Escritórios
INSERT INTO escritorios (id, localizacao, custo_suprimentos)
VALUES
(1, 'São Paulo', 5000.00),
(2, 'Rio de Janeiro', 4000.00),
(3, 'Brasil - Inovações', 3000.00);
 
-- Relacionamento Escritórios e Regiões
INSERT INTO regioes_escritorios (escritorio_id, regiao_id)
VALUES
(1, 1), (2, 2), (3, 3);
 
-- Inserção de Regiões
INSERT INTO regioes (id, nome)
VALUES
(1, 'Sudeste'),
(2, 'Sul'),
(3, 'Nordeste');
 
-- Total de Funcionários na Empresa
SELECT COUNT(*) AS total_funcionarios
FROM funcionarios;
 
-- Funcionários no Departamento de Tecnologia
SELECT COUNT(*) AS total_tec
FROM funcionarios
WHERE departamento_id = 1;
 
-- Custo Total de Salários do Departamento de Vendas
SELECT SUM(salario) AS custo_total_vendas
FROM funcionarios
WHERE departamento_id = 2;
 
-- Produtos Mais Vendidos (Por Quantidade)
SELECT nome, quantidade
FROM produtos
ORDER BY quantidade DESC
LIMIT 1;
 
-- Produto Mais Caro no Inventário
SELECT nome, valor_unitario
FROM produtos
ORDER BY valor_unitario DESC
LIMIT 1;
 
-- Funcionários com Cônjuge
SELECT COUNT(*) AS funcionarios_com_conjuges
FROM funcionarios
WHERE cônjuge = TRUE;
 
-- Funcionário Contratado Há Mais Tempo
SELECT nome
FROM funcionarios
ORDER BY data_contratacao ASC
LIMIT 1;
 
-- Funcionário Contratado Há Menos Tempo
SELECT nome
FROM funcionarios
ORDER BY data_contratacao DESC
LIMIT 1;
 
-- Funcionários com Mais Tempo na Empresa
SELECT nome
FROM funcionarios
ORDER BY data_contratacao ASC;
 
-- Média Salarial dos Funcionários ao Longo dos Anos
SELECT YEAR(data_contratacao) AS ano, AVG(salario) AS media_salarial
FROM funcionarios
GROUP BY YEAR(data_contratacao);
 
-- Média Salarial Excluindo CEO, CMO e CFO
SELECT AVG(salario) AS media_salarial
FROM funcionarios
WHERE cargo NOT IN ('CEO', 'CMO', 'CFO');
 
-- Média Salarial no Departamento de Tecnologia
SELECT AVG(salario) AS media_salarial_tec
FROM funcionarios
WHERE departamento_id = 1;
 
-- Departamento com Maior Média Salarial
SELECT departamento_id, AVG(salario) AS media_salarial
FROM funcionarios
GROUP BY departamento_id
ORDER BY media_salarial DESC
LIMIT 1;
 
-- Departamento com Menor Número de Funcionários
SELECT departamento_id, COUNT(*) AS num_funcionarios
FROM funcionarios
GROUP BY departamento_id
ORDER BY num_funcionarios ASC
LIMIT 1;
 
-- Produto Mais Valioso da Empresa
SELECT nome, valor_unitario * quantidade AS valor_total
FROM produtos
ORDER BY valor_total DESC
LIMIT 1;
 
-- Produto Mais Vendido da Empresa
SELECT nome
FROM produtos
ORDER BY quantidade DESC
LIMIT 1;
 
-- Produto Menos Vendido da Empresa
SELECT nome
FROM produtos
ORDER BY quantidade ASC
LIMIT 1;
 
-- Quantos Escritórios a "Momento" Possui em Cada Região
SELECT r.nome AS regiao, COUNT(e.id) AS num_escritorios
FROM regioes r
JOIN regioes_escritorios re ON r.id = re.regiao_id
JOIN escritorios e ON e.id = re.escritorio_id
GROUP BY r.nome;
 
-- Custo Total de Suprimentos em Cada Escritório
SELECT e.localizacao, e.custo_suprimentos
FROM escritorios e
ORDER BY e.custo_suprimentos DESC;
