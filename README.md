# <p align="center">Correcção do Teste 2 (Prático) de Base de Dados Espacais</p>

## Número 1: 
<p> Uma empresa pretende registar informações sobre seus trabalhadores, incluindo o cargo que cada um exerce na
empresa. Para os funcionários, a empresa possui os seguintes dados: código do cliente, nome do cliente, género, data de
nascimento, morada; A empresa tem funcionários exercendo os seguintes cargos: Director, chefe do departamento, secretária, contabilista,
financeiro, e técnico de informática.</p>

### a)Crie uma base de dados que permita registar estes dados, respeitando o conceito de normalização de base de dados, de modo a garantir a sua integridade


```
-- tabela funcionario
CREATE TABLE funcionario (
  id_funcionario integer not NULL PRIMARY KEY,
  nome_funcionario VARCHAR(30) not null,
  genero VARCHAR(2) not null,
  data_nasc date,cod_bairro INTEGER not null REFERENCES bairros(id),
  cod_cargo integer not null references cargo(id_cargo)
);
```
```
--tabela cargo
create table cargo(
  id_cargo INTEGER not null PRIMARY key,
  designacao VARCHAR(30) not null
);
```

```
--inserccao de cargos
INSERT into cargo VALUES 
  (1, 'Director'),
  (2, 'Chefe do Departamento'), 
  (3, 'Secretaria'),
  (4, 'Contabilista'),
  (5, 'Financeiro'),
  (6, 'Tecnico de Informatica');
```

### b)Registe pelo menos 10 funcionários da empresa, com os diversos cargos</p>

```
insert into funcionario VALUES
  (1, 'Fernando Gomes', 'M', '2000-10-16',1 , 1),
  (2, 'Rafaela Rui', 'F', '1999-11-07', 1, 2),
  (3, 'Luiza Viriato', 'F', '1980-12-17', 1, 3),
  (4, 'Marcos Manuel', 'M', '1978-11-26', 6, 4),
  (5, 'Emilia Fernando', 'F', '2000-10-16', , 5),
  (6, 'Joao Bento', 'M', '2000-02-16', 16, 6),
  (7, 'Artur Gomes', 'M', '2000-03-16',12 , 5),
  (8, 'Jorge Gomes', 'M', '1995-04-16',12 , 4),
  (9, 'Elisa Gomes', 'F', '1996-05-16',21 , 3),
  (10, 'Maria Eduarda', 'F', '2000-10-16',6 , 2);
          
```

## 1.1. Associe a morada de cada um dos funcionários à uma localização geográfica (bairro)
### a) 3 funcionários são residentes do bairro Matola Gare; 2 são do Bairro Triunfo; 1 do bairro Polana Cimento; 2 do Bairro 3. De Fevereiro; e 1 do bairro Luís Cabral.
Essa associação foi feita na inserção dos funcionários

### b) Cada bairro pertence à um determinado distrito (Associe cada bairro à um distrito)
`update bairros set cod_distrito = '11' where id =1`  Matola Gare

`update bairros set cod_distrito = '13' where id =6` Bairro Triunfo

`update bairros set cod_distrito = '13' where id =12` Bairro 3 de Fevereiro

`update bairros set cod_distrito = '13' where id =16` Luis Cabral

`update bairros set cod_distrito = '13' where id =21` Polana Cimento B


##
## Número 2: Consultas
### a) Escrever instruções SQL para imprimir os funcionários que moram no Bairro da Matola Gare ou no Bairro 3 de Fevereiro;
```
select f.nome_funcionario as "Nome do Funcionario", f.genero as "Genero", b.full_name as "Nome do Bairro"
from funcionario f
inner join bairros b
on f.cod_bairro = b.id
WHERE full_name= 'MATOLA GARE' or full_name = 'B. 3 DE FEVEREIRO';
```

### b) Escrever instruções SQL para imprimir o número de funcionários que moram na Cidade da Matola;
```
--para contar
select count(*)
from (bairros b inner join funcionario f
	on f.cod_bairro = b.id)
inner join distritos d
on b.cod_distrito =  d.id
where distrito ='CIDADE_DA_MATOLA';

--para mostrar esses alunos
select f.nome_funcionario as "Nome do Funcionario" ,b.full_name as "Nome do Bairro", 
d.distrito as "Nome do Distrito"
from (bairros b inner join funcionario f
	on f.cod_bairro = b.id)
inner join distritos d
on b.cod_distrito =  d.id
where distrito ='CIDADE_DA_MATOLA';
```

### c) Escreva instruções SQL para imprimir todos funcionários que moram na Cidade da Matola e que sejam do sexo Feminino (Imprimir: nome do funcionário, género, nome do bairro, bairro(geometria), e nome do distrito, distrito (geometria));

```
select f.nome_funcionario as "Nome do Funcionario", f.genero as "Genero" ,
b.full_name as "Nome do Bairro", b.geom as "Geometria do Bairro",
d.distrito as "Nome do Distrito", d.geom as "Geometria do Distrito"
from (bairros b inner join funcionario f
	on f.cod_bairro = b.id)
inner join distritos d
on b.cod_distrito =  d.id
where distrito ='CIDADE_DA_MATOLA' and genero = 'F' ;
```

### d) Imprimir todos funcionários com idade superior à 20 anos e que moram na Cidade de Maputo (Imprimir: nome, idade, e Cidade de Maputo).

```
select f.nome_funcionario as "Nome do Funcionario", EXTRACT(YEAR FROM age(CAST(data_nasc as date))) as "Idade",
d.distrito as "Nome do Distrito"
from (bairros b inner join funcionario f
	on f.cod_bairro = b.id)
inner join distritos d
on b.cod_distrito =  d.id
where distrito ='CIDADE_DE_MAPUTO' and EXTRACT(YEAR FROM age(CAST(data_nasc as date))) > 20 ;
```
