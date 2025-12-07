# Projeto: PetCare - Sistema de Gestão para Pet Shop e Clínica Veterinária
**Disciplina:** Modelagem de Banco de Dados  
**Aluno:** Matheus Eduardo Silva Oliveira 
**Data:** 7/12/2025

---

## Sumário
1. Cenário  
2. Modelagem Conceitual (DER)  
3. Modelagem Lógica  
4. Modelagem Física (Implementação no Supabase)  
5. Dados (População)  
6. CRUD (Demonstração)  
7. Relatórios (Consultas SQL)  
8. Estrutura do Repositório  
9. Considerações Finais  

---

## Cenário

### Descrição da empresa

A **PetCare** é um pet shop e clínica veterinária fictícia localizada na cidade de Franca–SP, especializada no atendimento de animais domésticos como cães e gatos. A empresa oferece serviços de consultas veterinárias, vacinas, exames, banhos, tosas e venda de produtos.

Atualmente, a empresa enfrentava dificuldades no controle manual de informações, como cadastro de clientes, registro de pets, histórico de consultas, controle de pagamentos e geração de relatórios. As informações eram anotadas em planilhas e fichas físicas, o que causava perda de dados, demora no atendimento e falta de organização.

### Problema a ser resolvido

O sistema foi desenvolvido para:

- Organizar o cadastro de clientes e seus respectivos pets  
- Registrar consultas e atendimentos veterinários  
- Controlar pagamentos realizados  
- Armazenar o histórico médico dos animais  
- Gerar relatórios de desempenho e faturamento  

### Entidades identificadas

- Cliente  
- Pet  
- Veterinário  
- Consulta  
- Pagamento  
- Tratamento  

---

## Modelagem Conceitual (DER)

O Diagrama Entidade-Relacionamento (DER) foi desenvolvido para representar graficamente as entidades e seus relacionamentos.

### Principais relacionamentos:

- **Cliente (1) — (N) Pet**  
  Um cliente pode ter vários pets, mas cada pet pertence a apenas um cliente.

- **Pet (1) — (N) Consulta**  
  Um pet pode ter várias consultas registradas.

- **Veterinário (1) — (N) Consulta**  
  Um veterinário pode atender diversas consultas.

- **Consulta (N) — (N) Tratamento**  
  Uma consulta pode ter vários tratamentos e um tratamento pode estar presente em várias consultas, utilizando uma tabela associativa.

📌 O diagrama se encontra no arquivo:  
`/imagens/der.png`

---

## Modelagem Lógica

### Tabela: clientes
- `id_cliente` (PK, SERIAL)  
- `nome` (VARCHAR(100), NOT NULL)  
- `cpf` (VARCHAR(14), UNIQUE, NOT NULL)  
- `telefone` (VARCHAR(20))  
- `rua` (VARCHAR(100))  
- `numero` (VARCHAR(10))  
- `bairro` (VARCHAR(50))  
- `cidade` (VARCHAR(50))  
- `uf` (VARCHAR(2)) — atributo composto (endereço)

### Tabela: pets
- `id_pet` (PK, SERIAL)  
- `nome` (VARCHAR(100), NOT NULL)  
- `especie` (VARCHAR(50))  
- `raca` (VARCHAR(50))  
- `data_nascimento` (DATE)  
- `id_cliente` (FK → clientes.id_cliente)

### Tabela: veterinarios
- `id_veterinario` (PK, SERIAL)  
- `nome` (VARCHAR(100), NOT NULL)  
- `crmv` (VARCHAR(20), UNIQUE, NOT NULL)  
- `especialidade` (VARCHAR(100))

### Tabela: consultas
- `id_consulta` (PK, SERIAL)  
- `data_hora` (TIMESTAMP, NOT NULL)  
- `diagnostico` (TEXT)  
- `valor` (DECIMAL(10,2))  
- `id_pet` (FK → pets.id_pet)  
- `id_veterinario` (FK → veterinarios.id_veterinario)  
- `idade_pet` (DERIVADO) – calculado com base na `data_nascimento` do pet

### Tabela: tratamentos
- `id_tratamento` (PK, SERIAL)  
- `nome` (VARCHAR(100))  
- `descricao` (TEXT)

### Tabela associativa: consulta_tratamento
- `id_consulta` (FK)  
- `id_tratamento` (FK)

### Tabela: pagamentos
- `id_pagamento` (PK, SERIAL)  
- `id_consulta` (FK → consultas.id_consulta)  
- `valor_pago` (DECIMAL(10,2))  
- `data_pagamento` (DATE)  
- `metodo_pagamento` (VARCHAR(50))

---

## Modelagem Física (Implementação no Supabase)

O banco de dados foi implementado no Supabase utilizando PostgreSQL.

Para executar o projeto:

1. Acessar o painel do Supabase.
2. Abrir a opção **SQL Editor**.
3. Executar os arquivos:
   - `create_tables.sql`
   - `insert_data.sql`
   - `crud.sql`
   - `relatorios.sql`

Foram utilizadas as seguintes restrições:
- Chaves primárias com `SERIAL`  
- Chaves estrangeiras com `REFERENCES`  
- `NOT NULL` em campos obrigatórios  
- `UNIQUE` em campos únicos como CPF e CRMV

---

## Dados (População)

Foram inseridos no mínimo 500 registros em cada tabela.

Os dados foram gerados com:
- Mockaroo (gerador de dados fictícios)
- Scripts SQL automáticos

Todas as informações são fictícias e utilizadas apenas para fins educacionais.

---

## CRUD (Demonstração)

As operações de CRUD foram testadas diretamente no Supabase via SQL Editor.

### Exemplos implementados:

- **INSERT** – Inserção de cliente e pet  
- **SELECT** – Consulta de pets por cliente  
- **UPDATE** – Atualização de telefone do cliente  
- **DELETE** – Exclusão de pagamento específico  

📷 Prints das operações estão salvos em:  
`/imagens/crud/`

---

## Relatórios (Consultas SQL)

Foram desenvolvidas 10 consultas SQL utilizando:

- `SELECT`
- `WHERE`
- `ORDER BY`
- `JOIN`
- Funções de agregação (`COUNT`, `SUM`, `AVG`)

Exemplos de relatórios:

1. Faturamento mensal  
2. Pets cadastrados por espécie  
3. Veterinários com maior número de consultas  
4. Consultas realizadas por período  
5. Clientes que mais realizaram consultas  

Scripts disponíveis no arquivo:  
`/sql/relatorios.sql`

---

## Estrutura do Repositório

