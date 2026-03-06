# Fullstack Product System

Projeto **Full Stack** desenvolvido como desafio técnico utilizando **Python (Flask)** no backend, **Redis + Worker** para processamento assíncrono, **PostgreSQL** como banco de dados e **Angular** no frontend.

O objetivo do sistema é implementar autenticação de usuários e um **CRUD de produtos**, onde operações de escrita são processadas de forma assíncrona através de uma fila.

---

# Arquitetura do Sistema

O sistema utiliza uma arquitetura baseada em **fila e worker**, separando operações de leitura e escrita.

Fluxo geral:

Frontend (Angular)  
↓  
Backend API (Flask)  
↓  
Redis Queue  
↓  
Worker (Python)  
↓  
PostgreSQL  

### Funcionamento

- O frontend se comunica com a **API Flask**
- Operações de **create, update e delete** não escrevem diretamente no banco
- A API coloca essas operações em uma **fila Redis**
- Um **worker separado** consome a fila e aplica as mudanças no banco
- A operação de **listagem de produtos** consulta diretamente o banco

Essa abordagem permite desacoplamento entre API e processamento de dados.

---

# Tecnologias Utilizadas

### Backend
- Python
- Flask
- SQLAlchemy
- PostgreSQL
- Redis

### Frontend
- Angular
- Typescript
- HttpClient

### Infraestrutura
- Docker (para Redis)


# Como executar o projeto

## 1. Iniciar o Redis (Docker)

O projeto utiliza Redis como fila de mensagens.  
Execute o Redis utilizando Docker.

```bash
docker run -d -p 6379:6379 --name redis redis


2. Iniciar o Backend (API Flask)

Entre na pasta da API:

cd productsApi

Crie o ambiente virtual:

python -m venv .venv

Ative o ambiente virtual.

Windows:

.venv\Scripts\activate