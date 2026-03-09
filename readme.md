# Fullstack Product System

Projeto **Full Stack** desenvolvido como desafio técnico utilizando **Python (Flask)** no backend, **Redis + Worker** para processamento assíncrono, **PostgreSQL** como banco de dados e **Angular** no frontend.

O sistema implementa **autenticação de usuários** e um **CRUD de produtos**, onde operações de escrita são processadas de forma **assíncrona através de uma fila Redis**.

---

# Arquitetura do Sistema

O sistema segue uma arquitetura baseada em **fila e worker**, separando operações de leitura e escrita para melhorar escalabilidade e desacoplamento.

Fluxo da aplicação:

Frontend (Angular)
↓
Backend API (Flask)
↓
Redis Queue
↓
Worker (Python)
↓
PostgreSQL

## Funcionamento

1. O **Frontend Angular** envia requisições para a API.
2. A **API Flask** autentica o usuário e valida a requisição.
3. Operações de **create, update e delete** são enviadas para uma **fila Redis**.
4. Um **Worker em Python** consome a fila e executa as operações no banco.
5. Operações de **listagem (GET)** consultam diretamente o banco PostgreSQL.

Essa arquitetura desacopla o processamento das operações de escrita, permitindo maior controle e escalabilidade.

---

# Tecnologias Utilizadas

## Backend

* Python
* Flask
* SQLAlchemy
* PostgreSQL
* Redis

## Frontend

* Angular
* Typescript
* HttpClient

## Infraestrutura

* Docker (utilizado para rodar o Redis)

---

# Estrutura do Projeto

Exemplo simplificado da organização do projeto:

```
project-root
│
├── productsApi
│   ├── app.py
│   ├── routes
│   ├── models
│   ├── services
│   └── worker
│
├── frontend
│   └── angular-app
│
└── README.md
```

---

# Como Executar o Projeto

## 1 - Iniciar o Redis (Docker)

O Redis é utilizado como **fila de mensagens para processamento assíncrono**.

Execute o container:

```
docker run -d -p 6379:6379 --name redis redis
```

Verifique se o Redis está rodando:

```
docker ps
```

---

# 2 - Iniciar o Backend (Flask API)

Entre na pasta da API:

```
cd productsApi
```

Crie o ambiente virtual:

```
python -m venv .venv
```

Ative o ambiente virtual.

### Windows

```
.venv\Scripts\activate
```

### Linux / Mac

```
source .venv/bin/activate
```

Instale as dependências:

```
pip install -r requirements.txt
```

Inicie o servidor Flask:

```
python app.py
```

A API estará disponível em:

```
http://localhost:5000
```

---

# 3 - Iniciar o Worker

O Worker é responsável por consumir as mensagens da fila Redis e executar as operações no banco de dados.

Execute o worker:

```
python worker.py
```

O worker ficará escutando a fila e processando as operações pendentes.

---

# Credenciais de Acesso

Para facilitar os testes do sistema existe um usuário administrador padrão.

Usuário:

```
admin
```

Senha:

```
123
```

Esse usuário pode ser utilizado para realizar login e testar todas as funcionalidades da aplicação.

---

# Funcionalidades

O sistema possui as seguintes funcionalidades:

* Autenticação de usuário
* CRUD de produtos
* Processamento assíncrono de operações
* Fila Redis para desacoplamento
* Worker dedicado para escrita no banco
* Consulta direta ao banco para listagem de produtos

---

# Endpoints Principais

### Autenticação

POST

```
/login
```

Realiza autenticação e retorna token de acesso.

---

### Produtos

Criar produto

```
POST /products
```

Listar produtos

```
GET /products
```

Atualizar produto

```
PUT /products/{id}
```

Remover produto

```
DELETE /products/{id}
```

Operações de **create, update e delete** são enviadas para a fila Redis e processadas pelo worker.

---

# Observações Técnicas

Este projeto foi desenvolvido como **desafio técnico** com foco em demonstrar:

* Organização de código
* Arquitetura baseada em filas
* Processamento assíncrono
* Integração entre backend, worker e banco de dados
* Boas práticas de desenvolvimento fullstack

---

# Autor

Danilo Urze
