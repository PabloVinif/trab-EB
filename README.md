# trab-EB

Pablo Vinicius Formagio Lima - RA: 1993321-2

API de Autenticação com Controle de Papéis
Este projeto é uma API RESTful construída com NestJS para gerenciar autenticação de usuários via JWT e controle de acesso baseado em diferentes papéis (roles), como admin e user.

Primeiros Passos
Siga as instruções abaixo para configurar e executar o projeto em seu ambiente local.

1. Pré-requisitos
Node.js

NPM ou Yarn

PostgreSQL

2. Instalação e Configuração
git clone https://github.com/PabloViniF/trab-EB.git

# Instale todas as dependências
npm install

3. Banco de Dados e Variáveis de Ambiente
Crie um arquivo chamado .env na raiz do projeto. Ele deve conter as credenciais de acesso ao seu banco de dados PostgreSQL e o segredo do JWT.

Arquivo .env:

# Configurações do banco de dados PostgreSQL
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=postgres
DATABASE_PASSWORD=123456
DATABASE_NAME=auth_db

# Segredo para o JWT
JWT_SECRET=sua_chave_secreta_aqui

Antes de iniciar o servidor, acesse o PostgreSQL e crie o banco de dados com o nome que você definiu em DATABASE_NAME:

CREATE DATABASE auth_db;

4. Iniciando o servidor
Com tudo configurado, execute o comando abaixo para iniciar a aplicação em modo de desenvolvimento:

npm run start:dev

Como Usar a API
Abaixo estão os exemplos das principais requisições.

Cadastro de um novo usuário
Requisição:
POST /users/register

{
  "name": "João",
  "email": "joao@email.com",
  "password": "123456",
  "role": "user"
}

Login
Requisição:
POST /auth/login

{
  "email": "joao@email.com",
  "password": "123456"
}

Resposta em caso de sucesso:

{
  "access_token": "seu_token_jwt_gerado"
}

Acessando Endpoints Protegidos
Para acessar rotas que exigem autenticação, inclua o access_token no cabeçalho da requisição:
Authorization: Bearer seu_token_jwt_gerado

Rota restrita para admin
GET /users

Se um usuário com o papel user tentar acessar, receberá o seguinte erro:

{
  "statusCode": 403,
  "message": "Forbidden resource",
  "error": "Forbidden"
}

Rotas acessíveis por admin e user
GET /users/:id
PATCH /users/:id
DELETE /users/:id

Estrutura e Segurança
Arquitetura do Projeto
A estrutura de pastas principal foi organizada da seguinte forma:

src/
├── auth/         # Lógica de autenticação, guards e estratégias JWT
├── users/        # Lógica de CRUD de usuários (entidades, DTOs, etc.)
├── app.module.ts # Módulo raiz da aplicação
└── main.ts       # Ponto de entrada da aplicação

Controle de Acesso com Papéis (Roles)
O sistema de segurança utiliza o decorator @Roles() em conjunto com Guards para proteger os endpoints.

Exemplo de uso:

@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('admin') // Endpoint acessível apenas por administradores

Principais Tecnologias
Framework: NestJS

ORM: TypeORM

Banco de Dados: PostgreSQL

Autenticação: JWT

Criptografia: bcrypt