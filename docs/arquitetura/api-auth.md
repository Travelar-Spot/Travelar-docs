# API de Autenticação do Travelar

## 1. Visão Geral

A **Travelar Auth API** é um microserviço fundamental e independente dentro do ecossistema Travelar. Sua responsabilidade é centralizar toda a lógica de segurança, garantindo modularidade e escalabilidade ao isolar o gerenciamento de identidades dos demais serviços de negócio.

[Link para o repositório](https://github.com/Travelar-Spot/Travelar-Auth-API)

### Funções Primárias

- Criação e autenticação de contas de usuários
- Emissão, assinatura e validação de JSON Web Tokens (JWT)
- Proteção de rotas internas via middleware
- Segurança no armazenamento de credenciais (Hashing)

## 2. Tecnologias utilizadas

O projeto foi construído utilizando um conjunto robusto de ferramentas focadas em performance e tipagem estática.

| Tecnologia | Função / Uso |
|------------|--------------|
| Node.js | Ambiente de execução JavaScript server-side |
| Express | Framework web para roteamento e controle HTTP |
| TypeScript | Superset JS para tipagem estática e segurança de código |
| TypeORM | ORM para modelagem de entidades e migrações de banco |
| PostgreSQL | Banco de dados relacional para persistência de usuários |
| Passport.js | Middleware de autenticação (Estratégias JWT) |
| Bcrypt | Algoritmo de hashing para criptografia de senhas |
| Class-Validator | Validação de dados de entrada (DTOs) via decorators |

## 3. Arquitetura e Estrutura de Pastas

A estrutura segue uma organização em camadas, separando configurações, lógica de domínio e interfaces de acesso.

```
src/
├── api/
│   └── auth/                  # Módulo principal de Autenticação
│       ├── dto/               # Objetos de Transferência de Dados (Validação)
│       ├── authController.ts  # Camada de entrada (Request/Response)
│       ├── authRoutes.ts      # Definição dos endpoints
│       ├── authService.ts     # Regras de negócio (Login, Registro)
│       └── userEntity.ts      # Modelo ORM da tabela de usuários
├── config/
│   ├── env.config.ts          # Carregamento de variáveis de ambiente
│   └── passport.config.ts     # Estratégias de proteção de rotas
├── database/
│   └── data-source.ts         # Conexão TypeORM e configuração do PostgreSQL
├── middlewares/
│   └── validation.middleware.ts # Validação automática de DTOs
├── app.ts                     # Configuração do Express e Middlewares
└── server.ts                  # Inicialização do servidor
```

## 4. Endpoints da API

Detalhamento dos recursos expostos pelo serviço.

### 4.1 Registro de Usuário

**Endpoint:** `POST /auth/register`

**Descrição:** Cria uma nova conta no sistema, aplicando hash na senha antes da persistência.

**Body (JSON):**
```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "password": "senhaForte123"
}
```

**Resposta Sucesso (201 Created):** Retorna o ID e dados públicos do usuário criado.

### 4.2 Login

**Endpoint:** `POST /auth/login`

**Descrição:** Valida credenciais e retorna um token de acesso.

**Body (JSON):**
```json
{
  "email": "john.doe@example.com",
  "password": "senhaForte123"
}
```

**Resposta Sucesso (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### 4.3 Perfil do Usuário

**Endpoint:** `GET /auth/profile`

**Descrição:** Retorna os dados do usuário atual baseando-se no token enviado.

**Headers:** `Authorization: Bearer <token>`

## 5. Segurança e Dados

- **Criptografia:** Senhas nunca são salvas em texto puro. Utiliza-se bcrypt com salts para hashing.
- **Tokens:** O JWT contém apenas o ID e tempo de expiração, assinado com JWT_SECRET.
- **Banco de Dados:** A entidade User possui restrição de unicidade no campo email.

## 6. Variáveis de Ambiente

Para a execução correta, configure o arquivo `.env`:

| Variável | Descrição |
|----------|-----------|
| PORT | Porta de execução da API (Ex: 8000) |
| DB_HOST | Host do PostgreSQL |
| DB_USERNAME | Usuário do banco |
| DB_PASSWORD | Senha do banco |
| DB_DATABASE | Nome do banco de dados |
| JWT_SECRET | Chave privada para assinatura de tokens |

## 7. Instalação e Execução

Serviço responsável por:

- Autenticação
- Autorização (JWT)
- Persistência de usuários com PostgreSQL

### 7.1. Pré-requisitos

- Node.js 18+
- NPM/Yarn
- Git
- PostgreSQL
- Docker (opcional)

### 7.2. Instalação

### 7.2.1. Clonar o repositório

```bash
git clone https://github.com/Travelar-Spot/Travelar-Auth-API.git
cd travelar-auth-api
```

### 7.2.2. Instalar dependências

```bash
npm install
# ou
yarn install
```

### 7.3. Configuração do Ambiente

### 7.3.1. Arquivo `.env`

```env
PORT=3000

DB_HOST=localhost
DB_PORT=5432
DB_USERNAME=postgres
DB_PASSWORD=senha
DB_DATABASE=travelar_auth_db

JWT_SECRET=sua_chave_segura
```

### 7.4. Executando

### 7.4.1. Modo Desenvolvimento

```bash
npm run dev
```

### 7.4.2 Modo Produção

```bash
npm run build
npm start
```

### 7.5. Scripts Úteis

| Script | Descrição |
|--------|-----------|
| `npm run dev` | Desenvolvimento |
| `npm run build` | Compilar |
| `npm start` | Produção |
| `npm run lint` / `lint:fix` | Lint |
| `npm run format` | Prettier |