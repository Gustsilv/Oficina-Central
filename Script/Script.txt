CREATE DATABASE OficinaMecanica;
USE OficinaMecanica;
DROP DATABASE OficinaMecanica;
CREATE TABLE Cliente (
		idCliente INT AUTO_INCREMENT PRIMARY KEY,
        celular CHAR(11) NOT NULL,
        email VARCHAR(45) NOT NULL        
);
CREATE TABLE PessoaFisica (
		idCliente INT PRIMARY KEY,
        cpfCliente CHAR(11) NOT NULL,
        nomeCliente VARCHAR(45) NOT NULL,
        CONSTRAINT cpf_do_cliente UNIQUE (cpfCliente),
        CONSTRAINT fk_pessoa FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
CREATE TABLE PessoaJuridica (
		idCliente INT PRIMARY KEY,
        cnpjCliente CHAR(14) NOT NULL,
        clienteRazaoSocial VARCHAR(50) NOT NULL,
        CONSTRAINT cnpj_do_cliente UNIQUE (cnpjCliente),
        CONSTRAINT fk_empresa FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
CREATE TABLE Endereco (
		idCliente INT PRIMARY KEY,
        logradouro VARCHAR(100) NOT NULL,
		numLogradouro INT NOT NULL,
		bairroLogradouro VARCHAR(45) NOT NULL,
		cidade VARCHAR(45) NOT NULL,
		estado VARCHAR(20) NOT NULL,
        CONSTRAINT fk_cliente_endereco FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente)
);
CREATE TABLE Veiculo (
		idVeiculo INT AUTO_INCREMENT PRIMARY KEY,
        idCliente INT,
        modelo VARCHAR(20) NOT NULL,
        marca VARCHAR(20) NOT NULL,
        tipo ENUM ('Carro','Moto') DEFAULT 'Carro',
        anoLancamento DATE NOT NULL,
        placa CHAR(7) NOT NULL,
        CONSTRAINT fk_cliente FOREIGN KEY (idCliente) REFERENCES Cliente(idCLiente),
        CONSTRAINT placa_do_veiculo UNIQUE (placa)
);
CREATE TABLE EquipeMecanica (
		idOficina INT AUTO_INCREMENT PRIMARY KEY,
        idVeiculo INT,
        nomeOficina VARCHAR(45) NOT NULL,
        nomePrestador VARCHAR(45) NOT NULL,
        cnpjOficina CHAR(14) NOT NULL,
        telefoneOficina VARCHAR(11) NOT NULL,
        emailOficina VARCHAR(45) NOT NULL,
        CONSTRAINT cnpj_da_oficina UNIQUE (cnpjOficina),
        CONSTRAINT fk_veiculo FOREIGN KEY (idVeiculo) REFERENCES Veiculo(idVeiculo)        
);
CREATE TABLE EnderecoOficina (
		idOficina INT PRIMARY KEY,
        logradouroOficina VARCHAR(100) NOT NULL,
		numLogOficina INT NOT NULL,
		bairroLogOficina VARCHAR(45) NOT NULL,
		cidadeOficina VARCHAR(45) NOT NULL,
		estadoOficina VARCHAR(20) NOT NULL,
        CONSTRAINT fk_endereco_oficina FOREIGN KEY (idOficina) REFERENCES EquipeMecanica(idOficina)
);
CREATE TABLE OrdemServico (
		idOrdemServico INT AUTO_INCREMENT PRIMARY KEY,
        dataSolicitação DATE NOT NULL
);
CREATE TABLE OficinaOS(
		idOficina INT,
        idOrdemServico INT,
        PRIMARY KEY (idOficina, idOrdemServico),
        CONSTRAINT fk_OrdemServico FOREIGN KEY (idOrdemServico) REFERENCES OrdemServico(idOrdemServico),
        CONSTRAINT fk_oficina FOREIGN KEY (idOficina) REFERENCES EquipeMecanica(idOficina)
);
CREATE TABLE Peca (
		idPeca INT AUTO_INCREMENT PRIMARY KEY,
        nomePeca VARCHAR(45) NOT NULL,
        valorUnitario FLOAT NOT NULL
);
CREATE TABLE Servico(
		idServico INT AUTO_INCREMENT PRIMARY KEY,
        nomeServico VARCHAR(45) NOT NULL,
        dataExecucao DATE NOT NULL,
        valorServico FLOAT NOT NULL
);
CREATE TABLE OSPeca (
		idOrdemServico INT,
        idPeca INT,
        qtdPeca INT NOT NULL,
        PRIMARY KEY(idOrdemServico, idPeca),
        CONSTRAINT fk_peca FOREIGN KEY (idPeca) REFERENCES Peca(idPeca),
        CONSTRAINT fk_Ordem_Servico_Peca FOREIGN KEY (idOrdemServico) REFERENCES OrdemServico(idOrdemServico)
        
);
CREATE TABLE OSServico (
    idOrdemServico INT,
    idServico INT,
    PRIMARY KEY (idOrdemServico, idServico),
    CONSTRAINT fk_OS_servico FOREIGN KEY (idServico) REFERENCES Servico (idServico),
    CONSTRAINT fk_Ordem_Servico FOREIGN KEY (idOrdemServico) REFERENCES OrdemServico (idOrdemServico)
);



ALTER TABLE Cliente AUTO_INCREMENT = 1; 
ALTER TABLE Veiculo AUTO_INCREMENT = 1; 
ALTER TABLE EquipeMecanica AUTO_INCREMENT = 1; 
ALTER TABLE OrdemServico AUTO_INCREMENT = 1;
ALTER TABLE Peca AUTO_INCREMENT = 1; 
ALTER TABLE Servico AUTO_INCREMENT = 1;

INSERT INTO Cliente (celular, email) VALUES
('81990001111', 'joao.silva@example.com'),
('81990002222', 'maria.oliveira@example.com'),
('81990003333', 'pedro.santos@example.com'),
('81990004444', 'ana.pereira@example.com'),
('81990005555', 'lucas.ferreira@example.com'),
('81990006666', 'mariana.almeida@example.com'),
('81990007777', 'carlos.souza@example.com'),
('81990008888', 'fernanda.lima@example.com'),
('81990009999', 'rafael.costa@example.com'),
('81990001000', 'juliana.araujo@example.com');

INSERT INTO PessoaJuridica (idCliente, cnpjCliente, clienteRazaoSocial) VALUES
(1, '12345678000101', 'Transporte Recife Ltda'),
(2, '23456789000112', 'Recife Logística S.A.'),
(3, '34567890000123', 'Transportadora Nordeste Ltda'),
(4, '45678901000134', 'Supermercado Boa Compra Ltda'),
(5, '56789012000145', 'Recife Atacadista S.A.'),
(6, '67890123000156', 'Construção e Cia Ltda'),
(7, '78901234000167', 'Materiais de Construção Recife Ltda'),
(8, '89012345000178', 'Loja de Ferragens Recife S.A.'),
(9, '90123456000189', 'Distribuidora Pernambuco Ltda'),
(10, '01234567000190', 'Supermercado Compre Mais Ltda');

INSERT INTO PessoaFisica (idCliente, cpfCliente, nomeCliente) VALUES
(1, '12345678901', 'João Silva'),
(2, '23456789012', 'Maria Oliveira'),
(3, '34567890123', 'Pedro Santos'),
(4, '45678901234', 'Ana Pereira'),
(5, '56789012345', 'Lucas Ferreira'),
(6, '67890123456', 'Mariana Almeida'),
(7, '78901234567', 'Carlos Souza'),
(8, '89012345678', 'Fernanda Lima'),
(9, '90123456789', 'Rafael Costa'),
(10, '01234567890', 'Juliana Araújo');

INSERT INTO Endereco (idCliente, logradouro, numLogradouro, bairroLogradouro, cidade, estado) VALUES
(1, 'Rua das Flores', 123, 'Boa Viagem', 'Recife', 'Pernambuco'),
(2, 'Avenida Recife', 456, 'Pina', 'Recife', 'Pernambuco'),
(3, 'Rua da Aurora', 789, 'Santo Amaro', 'Recife', 'Pernambuco'),
(4, 'Avenida Agamenon Magalhães', 101, 'Espinheiro', 'Recife', 'Pernambuco'),
(5, 'Rua Sete de Setembro', 202, 'Graças', 'Recife', 'Pernambuco'),
(6, 'Avenida Norte', 303, 'Rosarinho', 'Recife', 'Pernambuco'),
(7, 'Rua Padre Roma', 404, 'Tamarineira', 'Recife', 'Pernambuco'),
(8, 'Avenida Conselheiro Aguiar', 505, 'Boa Viagem', 'Recife', 'Pernambuco'),
(9, 'Rua do Príncipe', 606, 'Santo Amaro', 'Recife', 'Pernambuco'),
(10, 'Avenida Mascarenhas de Morais', 707, 'Imbiribeira', 'Recife', 'Pernambuco');

INSERT INTO Veiculo (idCliente, modelo, marca, tipo, anoLancamento, placa) VALUES
(1, 'Civic', 'Honda', 'Carro', '2018-03-01', 'ABC1234'),
(2, 'Corolla', 'Toyota', 'Carro', '2019-04-15', 'DEF5678'),
(3, 'Gol', 'Volkswagen', 'Carro', '2017-05-20', 'GHI9101'),
(4, 'Palio', 'Fiat', 'Carro', '2016-06-30', 'JKL1121'),
(5, 'HB20', 'Hyundai', 'Carro', '2020-07-11', 'MNO3141'),
(6, 'Corsa', 'Chevrolet', 'Carro', '2015-08-19', 'PQR5161'),
(7, 'Uno', 'Fiat', 'Carro', '2014-09-27', 'STU7181'),
(8, 'Kombi', 'Volkswagen', 'Carro', '2013-10-05', 'VWX9202'),
(9, 'Sandero', 'Renault', 'Carro', '2021-11-13', 'YZA1222'),
(10, 'Onix', 'Chevrolet', 'Carro', '2022-12-22', 'BCD2233');

INSERT INTO EquipeMecanica (idVeiculo, nomeOficina, nomePrestador, cnpjOficina, telefoneOficina, emailOficina) VALUES
(1, 'Oficina Central', 'José Mecânico', '12345678000101', '81990001111', 'oficinacentral@example.com');

INSERT INTO EnderecoOficina (idOficina, logradouroOficina, numLogOficina, bairroLogOficina, cidadeOficina, estadoOficina) VALUES
(1, 'Avenida Dantas Barreto', 800, 'Centro', 'Recife', 'Pernambuco');

INSERT INTO OrdemServico (dataSolicitação) VALUES
('2023-01-15'),
('2023-02-17'),
('2023-03-20'),
('2023-04-25'),
('2023-05-30'),
('2023-06-05'),
('2023-07-10'),
('2023-08-12'),
('2023-09-15'),
('2023-10-18');

INSERT INTO OficinaOS (idOficina, idOrdemServico) VALUES
(1, 1),
(1, 2),
(1, 3),
(1, 4),
(1, 5),
(1, 6),
(1, 7),
(1, 8),
(1, 9),
(1, 10);

INSERT INTO Peca (nomePeca, valorUnitario) VALUES
('Filtro de Óleo', 30.00),
('Pneu', 250.00),
('Velas', 45.00),
('Pastilha de Freio', 70.00),
('Correia Dentada', 120.00),
('Bateria', 450.00),
('Amortecedor', 300.00),
('Radiador', 400.00),
('Farol', 150.00),
('Lanterna Traseira', 90.00);

INSERT INTO Servico (nomeServico, dataExecucao, valorServico) VALUES
('Troca de Óleo', '2023-01-16', 80.00),
('Alinhamento', '2023-02-18', 100.00),
('Revisão Geral', '2023-03-21', 200.00),
('Troca de Filtro', '2023-04-26', 50.00),
('Balanceamento', '2023-05-31', 120.00),
('Troca de Pastilhas', '2023-06-06', 150.00),
('Troca de Pneu', '2023-07-11', 270.00),
('Reparo de Motor', '2023-08-13', 1000.00),
('Substituição de Correia', '2023-09-16', 400.00),
('Troca de Bateria', '2023-10-19', 500.00);

INSERT INTO OSPeca (idOrdemServico, idPeca, qtdPeca) VALUES
(1, 1, 1),
(2, 2, 4),
(3, 3, 4),
(4, 4, 2),
(5, 5, 1),
(6, 6, 1),
(7, 7, 2),
(8, 8, 1),
(9, 9, 2),
(10, 10, 2);

INSERT INTO OSServico (idOrdemServico, idServico) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7),
(8, 8),
(9, 9),
(10, 10);

DESC PessoaFisica;
-- 
SELECT * FROM Veiculo WHERE idVeiculo IN (3,5,7,9) ORDER BY anoLancamento ASC;

SELECT clienteRazaoSocial AS Razao_Social,
	   CONCAT(e.logradouro, ', ', e.numLogradouro, ', ', e.bairroLogradouro, ', ', e.cidade) AS Endereco_Completo
FROM Cliente c
INNER JOIN PessoaJuridica pj ON c.idCliente = pj.idCliente
INNER JOIN Endereco e ON c.idCliente = e.idCliente;

SELECT
    op.qtdPeca AS Quantidade_de_Pecas,
    ROUND(op.qtdPeca * p.valorUnitario, 2) AS Valor_Total,
    pf.cpfCliente AS Cpf_do_Cliente
FROM PessoaFisica pf
INNER JOIN Cliente c ON pf.idCliente = c.idCliente
INNER JOIN Veiculo v ON c.idCliente = v.idCliente
INNER JOIN EquipeMecanica e ON v.idVeiculo = e.idVeiculo
INNER JOIN OficinaOS o ON e.idOficina = o.idOficina
INNER JOIN OrdemServico os ON o.idOrdemServico = os.idOrdemServico
INNER JOIN OSPeca op ON os.idOrdemServico = op.idOrdemServico
INNER JOIN Peca p ON op.idPeca = p.idPeca
ORDER BY cpfCliente DESC;

SELECT
	pj.clienteRazaoSocial AS Razao_Social, pj.cnpjCliente AS CNPJ, CONCAT(v.marca,' ', v.modelo) AS Dados_do_Veiculo, v.placa as Placa, 
    e.nomePrestador AS Mecanico_Responsavel, e.nomeOficina AS Oficina, s.nomeServico AS Servico_Prestado, s.dataExecucao AS Data_de_Execucao, 
    s.valorServico AS Valor_do_Servico
FROM PessoaJuridica pj
INNER JOIN Cliente c ON pj.idCliente = c.idCliente
INNER JOIN Veiculo v ON c.idCliente = v.idCliente
INNER JOIN EquipeMecanica e ON v.idVeiculo = e.idVeiculo
INNER JOIN OficinaOS o ON e.idOficina = o.idOficina
INNER JOIN OrdemServico os ON o.idOrdemServico = os.idOrdemServico
INNER JOIN OSServico oss ON os.idOrdemServico = oss.idOrdemServico
INNER JOIN Servico s ON oss.idServico = s.idServico
WHERE nomeServico LIKE '%Troca de Óleo%';

SELECT
	pf.nomeCliente AS Nome_Completo, pf.cpfCliente AS CPF, v.marca, v.modelo, v.placa as Placa
FROM PessoaFisica pf
INNER JOIN Cliente c ON pf.idCliente = c.idCliente
INNER JOIN Veiculo v ON c.idCliente = v.idCliente
WHERE v.marca = 'Fiat' AND v.modelo = 'Palio';

SELECT
    v.marca AS Marca,
    v.modelo AS Modelo,
    s.nomeServico AS Servico_Prestado,
    COUNT(s.idServico) AS Total_Servicos
FROM Veiculo v
INNER JOIN EquipeMecanica e ON v.idVeiculo = e.idVeiculo
INNER JOIN OficinaOS o ON e.idOficina = o.idOficina
INNER JOIN OrdemServico os ON o.idOrdemServico = os.idOrdemServico
INNER JOIN OSServico oss ON os.idOrdemServico = oss.idOrdemServico
INNER JOIN Servico s ON oss.idServico = s.idServico
WHERE s.nomeServico = 'Balanceamento'
GROUP BY v.marca, v.modelo, s.nomeServico
HAVING v.marca = 'Honda' AND v.modelo = 'Civic';

