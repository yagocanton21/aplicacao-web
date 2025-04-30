# Exercício 1: Inserindo Dados

### 1.Clientes

* Para inserir os dados na coleção `client` basta usar o seguinte comando:
```bash
db.client.insertMany([
  {
    "full_name": "Maria Silva",
    "cpf": "12345678901",
    "email": "maria.silva@email.com",
    "phone": "11987654321",
    "address": "Rua das Flores, 123",
    "city": "São Paulo",
    "state": "SP",
    "zip_code": "01001000",
    "created_by": "user_id_do_admin_1",
    "enterprise": null,
    "cnpj_enterprise": null,
    "description": "Cliente individual"
  },
  {
    "full_name": "Empresa Soluções Ltda",
    "cpf": null,
    "email": "contato@solucoes.com.br",
    "phone": "21998765432",
    "address": "Avenida Principal, 456",
    "city": "Rio de Janeiro",
    "state": "RJ",
    "zip_code": "20010020",
    "created_by": "user_id_do_manager_2",
    "enterprise": "Soluções Ltda",
    "cnpj_enterprise": "12345678000190",
    "description": "Cliente corporativo"
  }
])
```
## 2.Processos

* Para inserir os seguintes processos para os clientes na coleção `client_processes` basta usar o seguinte comando:
```bash
db.client_processes.insertMany([
  {
   "client_id": "id_do_cliente_maria_silva",
    "number": "PROC-2023-001",
    "value": 1500.00,
    "status": "ativo",
    "class": "Cobrança",
    "description": "Processo de cobrança referente a fatura em aberto.",
    "created_at": ISODate("2023-10-26T10:00:00Z")
  },
  {
    "client_id": "id_do_cliente_empresa_solucoes",
    "number": "PROC-2023-002",
    "value": 5500.50,
    "status": "em análise",
    "class": "Contratual",
    "description": "Análise de contrato de prestação de serviços.",
    "created_at": ISODate("2023-11-15T14:30:00Z")
  }
  ])
```
## 3.Eventos

* Para inserir os eventos na coleção `events` basta usar o seguinte comando:
```bash
db.events.insertMany([
  {
   "client_id": "id_do_cliente_maria_silva",
    "enterprise": null,
    "cnpj_enterprise": null,
    "freight": 50.00,
    "amount_of_cleaning": 2,
    "cleaning_date": "2023-12-10",
    "cost_of_each_cleanin": 200.00,
    "proposal_doc": "PROP-MS-001.pdf",
    "number_proposal": "PROP-2023-001-MS",
    "proposal_expiration_date": ISODate("2023-11-30T23:59:59Z"),
    "created_proposal_by": "user_id_do_sales_3",
    "address": "Rua das Camélias, 789",
    "city": "São Paulo",
    "state": "SP",
    "zip_code": "02002000",
    "proposal_status": "accepted"
  },
  {
    "client_id": "id_do_cliente_empresa_solucoes",
    "enterprise": "Soluções Ltda",
    "cnpj_enterprise": "12345678000190",
    "freight": 120.00,
    "amount_of_cleaning": 5,
    "cleaning_date": "2024-01-15",
    "cost_of_each_cleanin": 150.00,
    "proposal_doc": "PROP-ESL-002.pdf",
    "number_proposal": "PROP-2023-002-ESL",
    "proposal_expiration_date": ISODate("2023-12-20T23:59:59Z"),
    "created_proposal_by": "user_id_do_sales_3",
    "address": "Avenida das Palmeiras, 1011",
    "city": "Rio de Janeiro",
    "state": "RJ",
    "zip_code": "22020030",
    "proposal_status": "pending accepted"
  }
  ])
```

# Exercicio 2: Consultando dados

### 1. Clientes em São Paulo: Encontre todos os clientes que estão localizados na cidade de "São Paulo".

* Resposta: Para encontrar todos os `clients` que estão localizados na cidade de `São Paulo`, basta usar o comando:

```bash
db.client.find({city:{$eq: "São Paulo"}})
```

### 2. Processos com Valor Superior a: Liste todos os processos na coleção client_processes cujo valor (value) seja maior que 2000.

* Resposta: Para encontrar os processos com `valores` maiores que `2000.0`, é só usar o seguinte comando:

```bash
db.client_processes.find({value: {$gt:2000.0}})
```

### 3. Eventos com Proposta Pendente ou Aceita: Encontre todos os eventos onde o proposal_status seja "pending accepted" ou "accepted".

* Resposta: Para encontrar todos os eventos que o status seja `"pending accepted"` ou `"accepted"`, basta usar o seguinte comando:

```bash
db.events.find({proposal_status:{$in:["pendingaccepted", "accepted"]}})
```

### 4. Clientes Corporativos: Liste todos os clientes onde o campo enterprise não seja null. Exiba apenas os campos full_name e cnpj_enterprise.

* Resposta: Para listar todos os clientes que o campo `enterprise` não seja `null`, e exibir os campos `full_name` e `cnpj_enterprise`, basta usar o seguinte comando:
```bash
db.client.find(
  {enterprise: { $ne: null } }, 
  {full_name: 1, cnpj_enterprise: 1, _id: 0 }
)
```

### 5. Processos de Cobrança: Encontre todos os processos cuja class seja "Cobrança" e ordene-os por valor em ordem decrescente.

* Resposta: Para encontrar todos os processos que a classe seja `Cobrança` e ordenar ele em ordem PROC-2023-001decrescente`, basta usar o seguinte comando:
```bash
db.client_processes.find({class: "Cobrança"}).sort({value: -1})
```

# Exercício 3: Atualizando Dados

### 1. Atualizar Status do Processo: Altere o status do processo com number "PROC-2023-001" para "concluído".

* Resposta: Para atualizar o status do processo `number: PROC-2023-001` para `number: Concluido`, basta usar o seguinte comando:
```bash
db.client_processes.updateOne(
  {number: "PROC-2023-001"}, 
  {$set: {number: "Concluido"}}
  )
```
### 2. Adicionar Observação ao Evento: Adicione o campo note_doc com o valor "OBS-MS-001.txt" ao evento de Maria Silva.

* Resposta: Para adicionar o campo `note_doc` com o valor `OBS-MS-001.txt` ao evento de `Maria Silva`, basta usar o seguinte comando:
```bash
db.events.updateOne(
  {client_id: "id_do_cliente_maria_silva"},
  {$set:{note_doc: "OBS-MS-001.txt"}})
```

### 3. Incrementar Quantidade de Limpezas: Aumente em 1 a amount_of_cleaning para o evento da Empresa Soluções Ltda.

* Resposta: Para aumentar em `1` a `amount_of_cleaning` da `Empresa Soluções Ltda`, basta usar o seguinte comando:
```bash
db.events.updateOne(
  {client_id: "id_do_cliente_empresa_solucoes"}, 
  {$inc: {amount_of_cleaning: 1}}
  )
```

# Exercício 4: Excluindo Dados

### 1. Remover Processo: Remova o processo com number "PROC-2023-002".

* Resposta: Para remover o processo com o `number: "PROC-2023-002"`, basta usar o seguinte comando:
```bash
db.client_processes.deleteOne({number: "PROC-2023-002"})
```

### 2. Visualizar Índices: Liste todos os índices da coleção client.

* Resposta: Para lista todos os `índeces` da coleção `client` basta usar o seguinte comando:

```bash
db.client.getIndexes()
```