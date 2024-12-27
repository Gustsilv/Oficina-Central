# Oficina Mecânica

Este repositório documenta a estrutura de um banco de dados para gestão de uma oficina mecânica, permitindo consulta eficiente de informações sobre clientes, veículos, serviços e muito mais.

## Minimundo

A oficina mecânica oferece serviços de manutenção e reparo para veículos, atendendo clientes pessoas físicas e jurídicas. Cada cliente possui informações detalhadas, como endereço, e pode ter um ou mais veículos registrados. Os serviços são realizados por equipes de mecânicos que pertencem a diferentes oficinas associadas. Para cada ordem de serviço, são registrados os serviços prestados e as peças utilizadas, permitindo um controle preciso dos custos e dos históricos de manutenção.

### Requisitos principais:

1. Gerenciar informações de clientes, incluindo dados pessoais e endereços.
2. Registrar veículos, associando-os aos clientes.
3. Manter registros detalhados de ordens de serviço, com serviços prestados e peças utilizadas.
4. Controlar as equipes mecânicas e suas respectivas oficinas.
5. Permitir consultas específicas, como histórico de manutenção de veículos e consumo de peças.

## Estrutura do Banco de Dados

O banco de dados inclui as seguintes tabelas principais:

- **Cliente**: Informações sobre clientes, podendo ser Pessoa Física ou Jurídica.
- **Endereço**: Dados de endereço dos clientes.
- **Veículo**: Detalhes dos veículos atendidos na oficina.
- **Equipe Mecânica**: Informações sobre os mecânicos e as oficinas.
- **Ordem de Serviço (OS)**: Registro de serviços realizados.
- **Peças e Serviços**: Informações sobre peças utilizadas e serviços prestados.

## Modelo Lógico

![Modelo lógico da Oficina Mecânica](URL_da_imagem "Título opcional")

## Consultas Frequentes

### 1. Detalhes dos veículos com os IDs 3, 5, 7 e 9 (ordem crescente de ano de lançamento)
```sql
SELECT * 
FROM Veiculo 
WHERE idVeiculo IN (3, 5, 7, 9) 
ORDER BY anoLancamento ASC;
```

### 2. Razões sociais e endereços completos dos clientes jurídicos
```sql
SELECT 
    clienteRazaoSocial AS Razao_Social,
    CONCAT(e.logradouro, ', ', e.numLogradouro, ', ', e.bairroLogradouro, ', ', e.cidade) AS Endereco_Completo
FROM Cliente c
INNER JOIN PessoaJuridica pj ON c.idCliente = pj.idCliente
INNER JOIN Endereco e ON c.idCliente = e.idCliente;
```

### 3. Quantidade e valor total de peças associadas a clientes físicos (ordem decrescente de CPF)
```sql
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
```

### 4. Clientes jurídicos que solicitaram troca de óleo (detalhes do serviço e veículo)
```sql
SELECT
    pj.clienteRazaoSocial AS Razao_Social, 
    pj.cnpjCliente AS CNPJ, 
    CONCAT(v.marca, ' ', v.modelo) AS Dados_do_Veiculo, 
    v.placa AS Placa, 
    e.nomePrestador AS Mecanico_Responsavel, 
    e.nomeOficina AS Oficina, 
    s.nomeServico AS Servico_Prestado, 
    s.dataExecucao AS Data_de_Execucao, 
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
```

### 5. Clientes físicos com veículos da marca Fiat, modelo Palio
```sql
SELECT
    pf.nomeCliente AS Nome_Completo, 
    pf.cpfCliente AS CPF, 
    v.marca, 
    v.modelo, 
    v.placa AS Placa
FROM PessoaFisica pf
INNER JOIN Cliente c ON pf.idCliente = c.idCliente
INNER JOIN Veiculo v ON c.idCliente = v.idCliente
WHERE v.marca = 'Fiat' AND v.modelo = 'Palio';
```

### 6. Total de serviços de balanceamento em veículos Honda Civic
```sql
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
```

## Como Usar

1. **Criação do Banco de Dados**:
   ```sql
   CREATE DATABASE OficinaMecanica;
   USE OficinaMecanica;
   ```
2. **Execução do Script**: Execute o script SQL fornecido para criar a estrutura e inserir os dados iniciais.
3. **Consultas**: Utilize as queries acima para responder às perguntas do sistema.

## Contribuição

Contribuições são bem-vindas! Envie sugestões ou abra um pull request com melhorias e novos recursos.
