
Resumo de comandos SQL

[[SQL/Tabelas]]

### DDL: linguagem de definição de dados

>Data Definition Language

Seu principal objetivo é definir a **estrutura dos dados** em um banco de dados. Com o uso desse grupo de comandos é possível **criar tabelas, definir campos, criar índices** e outras estruturas de dados. Os comandos que fazem parte desse grupo são o `CREATE`, `ALTER` e o `DROP`.

![[Pasted image 20240201111825.png]]


SQL SERVER

```sql 
CREATE DATABASE Base_Teste
ON (NAME = 'base_de_dados_teste.DAT',
        FILENAME = 'C:\TEMP\base_de_dados_teste.MDF',
        SIZE = 10MB,
        MAXSIZE = 50MB,
        FILEGROWTH = 5MB);
        
```


>https://learn.microsoft.com/pt-br/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver16&tabs=sqlpool


```sql
DROP DATABASE test;
```

>https://learn.microsoft.com/pt-br/sql/t-sql/statements/drop-database-transact-sql?view=sql-server-ver16


```sql
CREATE TABLE [tabela de clientes](
	[cpf] [char](11),
	[nome] [varchar] (150),
	[rua] [varchar] (150),
	[complemento] [varchar] (150),
	[bairro] [varchar] (150),
	[estado] [char] (2),
	[cep] [char] (8),
	[data de nascimento] [date],
	[idade] [smallint],
	[sexo] [char](1),
	[limite de credito][money],
	[volume minino][float],
	[primeira compra][bit]
);

CREATE TABLE [tabela de produtos](
	[codigo do produto] [varchar] (20) not null primary key,
	[nome do produto] [varchar] (50),
	[embalagem] [varchar] (50),
	[tamanho] [varchar] (50),
	[sabor] [varchar] (50),
	[preco de lista] [smallmoney]
);
```

Tipos de Dados


|**Categoria**|**Descrição**|**Exemplo**|**Comando**|
|---|---|---|---|
|Numéricos exatos|números de vários tamanhos que podem ser formatados|9.78 pode ser definida como `decimal(3,2)`|`bigint`, `int`, `smallint`,`tinyint`, `decimal(i,j)`, `numeric(i,j)`|
|Unidades monetárias|valores que representam dinheiro|685477.58 é saldo da conta bancária definida do tipo money|`money`, `smallmoney`|
|Numéricos aproximados|números de ponto flutuante com precisão|7.90 é do tipo float|`float` ou `real`|
|Cadeias de caracteres|Textos de tamanhos fixos|“modelagem” se encaixa em varchar(9)|`char(n)`|
||Texto de tamanho variável||`varchar(n)`|
|Valores lógicos|Termos que representa verdadeiro ou falso||`bit`|
|Datas|Datas, dias, mês, anos.|Calendário lunar, calendário comercial|`date` AAAA-MM-DD, `Smalldate`, `Datetimeoffset`|
||Tempo|10:59:13 é tipo time|`time`, `datetime`,|


Alter Table

```sql
ALTER TABLE	 [tabela de clientes] ALTER COLUMN [CPF][CHAR] NOT NULL;

ALTER TABLE [tabela de clientes] ADD CONSTRAINT pk_tabela_cliente 
PRIMARY KEY CLUSTERED ([cpf]);
```

>https://learn.microsoft.com/pt-br/sql/t-sql/statements/alter-table-transact-sql?view=sql-server-ver16



TRUNCATE
> https://learn.microsoft.com/pt-br/sql/t-sql/statements/truncate-table-transact-sql?view=sql-server-ver16





### DQL: linguagem de consulta de dados

>Data Query Language

SELECT

```SQL
SELECT * FROM [tabela de clientes];

SELECT [nome], [estado], [cpf] FROM [tabela de clientes];

/*ORDENANDO POR COLUNAS*/
SELECT [nome], [estado], [cpf] FROM [tabela de clientes] ORDER BY [nome];

/*RENOMEANDO AS COLUNAS*/
SELECT [nome] AS 'Nome do Cliente', [cpf] AS 'CPF', [estado] AS 'UF' FROM [tabela de clientes];

/*EXIBINDO VALORES ÚNICOS*/
SELECT DISTINCT [SABOR] FROM [tabela de produtos];

/*PESQUISA COM BASE EM FILTROS*/
SELECT [NOME] AS [NOME DO CLIENTE], [ESTADO] AS [UF], [PRIMEIRA COMPRA]
FROM [TABELA DE CLIENTES]
WHERE [PRIMEIRA COMPRA] = 1;

```


>https://learn.microsoft.com/pt-br/sql/t-sql/queries/select-transact-sql?view=sql-server-ver16






### DML: linguagem de manipulação de dados

> Data Manipulation Language

Aqui é possível **inserir, atualizar, excluir e selecionar dados** em uma tabela. Para essas aplicações, podemos utilizar o `INSERT`, `SELECT`, `DELETE` e o `UPDATE`.

![[Pasted image 20240201112001.png]]

INSERT

```sql
INSERT INTO [tabela de produtos] values (
	'1040107',
	'Ligth - 350 ml - Melância',
	'Lata',
	'350 ml',
	'Melancia',
	4.56
)

INSERT INTO [TABELA DE PRODUTOS]
([codigo do produto], [nome do produto], [embalagem], [tamanho], [preco de lista], [sabor])
VALUES
('5449310', 'Frescor do Verão - 350 ml - Limão', 'Lata', '350 ml',2.45, 'Limão'),
('1078680', 'Frescor do Verão - 350 ml - Manga', 'Lata', '350 ml',3.18, 'Manga');
```


> https://learn.microsoft.com/pt-br/sql/t-sql/statements/insert-transact-sql?view=sql-server-ver16

UPDATE

```sql
UPDATE [TABELA DE PRODUTOS]
SET [preco de lista] = [preco de lista] * 0.9
WHERE [embalagem] = 'Lata';


UPDATE [tabela de produtos] 
SET [embalagem] = 'Garrafa', [preco de lista] = 8.10
WHERE [codigo do produto] = '1088126';

```


> https://learn.microsoft.com/pt-br/sql/t-sql/queries/update-transact-sql?view=sql-server-ver16


DELETE

```SQL
DELETE from [tabela de produtos]
WHERE [codigo do produto] = '1004327';
```


WHERE

Operadores relacionais de funcionam normalmente em campos numéricos e em campos de texto funcionam ordenando alfabeticamente conforme tabela ASCII 

```sql
/*Operadores relacionais => < > = != */
SELECT [NOME] AS [NOME DO CLIENTE], [ESTADO] AS [UF], [PRIMEIRA COMPRA]
FROM [TABELA DE CLIENTES]
WHERE [PRIMEIRA COMPRA] = 1;

/* Diferente */
SELECT * FROM [tabela de produtos]
WHERE [embalagem] <> 'LATA';

/*organiza alfabeticamente(asc) filtra as strings que começam com letra maior*/
SELECT * FROM [tabela de produtos]
WHERE [embalagem] > 'LATA';

/*filtrando clientes nascidos apóes 31/12/1995*/
SELECT [nome], [data de nascimento] 
FROM [tabela de clientes]
WHERE [data de nascimento] > '1995-12-31';

/*Utilizando operador YEAR para extrair some o ano do timestamp e realizar o mesmo filtro anterior*/
SELECT [nome], [data de nascimento] 
FROM [tabela de clientes]
WHERE YEAR([data de nascimento]) > '1995';


SELECT [nome], [estado] 
FROM [tabela de clientes]
WHERE [estado]  <> 'SP' AND [estado] <> 'RJ';

SELECT [nome], [estado] 
FROM [tabela de clientes]
WHERE NOT([estado]  = 'SP' OR [estado] = 'RJ');
```

FILTROS COM AND e OR

```SQL
/*
Retornando clientes que residem em cobacabana ou tijuca
utilizando o operador OR
*/

SELECT [NOME], [BAIRRO] 
FROM [tabela de clientes]
WHERE [BAIRRO] = 'Copacabana' OR [BAIRRO] = 'Tijuca';

/* OPERADOR NOT*/
SELECT [nome], [estado] 
FROM [tabela de clientes]
WHERE NOT([estado]  = 'SP' OR [estado] = 'RJ');

/*consultando cliente que fizeram a primeira compra e residem em SP
utilzando o operador AND
*/
SELECT [nome], [estado], [primeira compra] 
FROM [tabela de clientes]
WHERE [primeira compra] = '1' AND [estado] = 'SP'

/*Diferente*/
SELECT [nome], [estado] 
FROM [tabela de clientes]
WHERE [estado]  <> 'SP' AND [estado] <> 'RJ';
```



### DCL: linguagem de controle de dados

> Data Control Language


GRANT
REVOKE 
DENY

### DTL ou TCL: linguagem de transação de dados

> Data Transaction Language 


BEGIN 
TRANSACTION
COMMIT 
ROLLBACK