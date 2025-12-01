# Backend Travelar 

## 1. Visão Geral do Projeto

O **Travelar Backend** é o núcleo de regras de negócios da plataforma. Diferente da Auth API, este serviço gerencia as entidades principais do domínio de hospedagem: Imóveis, Reservas, Avaliações e Upload de Mídia. Ele é construído para ser robusto e testável, integrando-se com serviços externos (Cloudinary para imagens) e utilizando containerização (Docker) para facilitar o deploy.

O Backend do projeto encontra-se hospedado na plataforma **Render**.


[Link para o repositório](https://github.com/Travelar-Spot/Travelar-backend)

### Principais Responsabilidades

- CRUD completo de Imóveis e gerenciamento de disponibilidade
- Processamento de Reservas (Check-in/Check-out)
- Sistema de Avaliações e Comentários
- Upload e gestão de imagens de imóveis via Cloudinary
- Documentação automática via Swagger

## 2. Tecnologias Utilizadas

A stack tecnológica é consistente com o ecossistema, adicionando ferramentas específicas para testes e documentação.

| Tecnologia | Categoria | Descrição e Uso |
|------------|-----------|-----------------|
| Node.js & TypeScript | Core | Base do desenvolvimento backend |
| TypeORM | ORM | Gerenciamento de entidades e relações complexas (1:N, N:N) |
| PostgreSQL | Banco de Dados | Persistência relacional |
| Jest | Testes | Framework completo para testes Unitários, Integração e Parametrizados |
| Swagger | Documentação | Geração de documentação interativa da API (swagger.json) |
| Docker | Infraestrutura | Containerização da aplicação e do banco de dados via docker-compose |
| Cloudinary | Mídia | Serviço externo para armazenamento e otimização de imagens |

## 3. Estrutura de Pastas e Arquitetura

O projeto adota uma arquitetura baseada em **Domínios/Módulos**, onde cada entidade principal possui sua própria pasta contendo toda sua lógica (Controller, Service, Entity, Routes, Tests).

```
Travelar-backend/
├── src/
│   ├── config/                # Configurações de serviços externos
│   │   ├── cloudinary.ts      # Setup do Cloudinary
│   │   ├── env.config.ts      # Variáveis de ambiente
│   │   └── passport.config.ts # Autenticação
│   ├── database/
│   │   └── data-source.ts     # Conexão TypeORM
│   ├── Imovel/                # Módulo de Imóveis
│   │   ├── controller.ts      # Lógica de entrada/saída HTTP
│   │   ├── entity.ts          # Definição da tabela Imóveis
│   │   ├── routes.ts          # Rotas /imoveis
│   │   ├── service.ts         # Regras de negócio (Criação, busca com filtros)
│   │   └── Testes/            # Bateria de testes do módulo
│   │       ├── integracao.test.ts
│   │       ├── parametrizados.test.ts
│   │       └── unitarios.test.ts
│   ├── Reserva/               # Módulo de Reservas (estrutura idêntica ao Imovel)
│   ├── Avaliacao/             # Módulo de Avaliações (estrutura idêntica ao Imovel)
│   ├── Usuario/               # Módulo de Gestão de Perfil de Usuário
│   ├── UploadImagens/         # Rotas utilitárias para upload
│   │   └── routes.ts
│   ├── swagger/               # Definições da API
│   │   └── swagger.json
│   ├── app.ts                 # Setup do App Express
│   └── server.ts              # Inicialização
├── coverage/                  # Relatórios de cobertura de testes Jest
├── docker-compose.yml         # Orquestração de containers
├── Dockerfile                 # Imagem Docker da aplicação
├── jest.config.js             # Configuração global de testes
└── package.json
```

## 4. Endpoints da API

Abaixo estão os principais grupos de endpoints gerenciados pelo Core Backend.

### 4.1. Imóveis (/imoveis)

Gerencia o catálogo de propriedades.

- **POST /imoveis** - Cadastra novo imóvel (Requer auth)
- **GET /imoveis** - Busca pública com filtros (cidade, preço, datas)
- **GET /imoveis/{id}** - Detalhes de um imóvel específico
- **PUT /imoveis/{id}** - Atualiza dados do imóvel (Apenas proprietário)
- **DELETE /imoveis/{id}** - Remove imóvel do sistema

### 4.2. Reservas (/reservas)

Controla o fluxo de aluguel.

- **POST /reservas** - Cria uma intenção de reserva
- **GET /reservas** - Lista reservas do usuário logado
- **PUT /reservas/{id}** - Atualiza datas ou status da reserva
- **DELETE /reservas/{id}** - Cancela uma reserva

### 4.3. Avaliações (/avaliacoes)

Sistema de feedback.

- **POST /avaliacoes** - Cria avaliação para uma estadia concluída
- **GET /avaliacoes/imovel/{id}** - Lista avaliações de uma propriedade

### 4.4. Upload (/upload)

- **POST /upload** - Recebe multipart/form-data, envia para o Cloudinary e retorna a URL segura da imagem

## 5. Estratégia de Testes

O projeto possui uma cultura forte de qualidade de software, utilizando Jest para três níveis de testes, localizados dentro da pasta `Testes` de cada módulo:

### 5.1. Tipos de Testes

- **Testes Unitários:** Testam funções isoladas (ex: validação de data, cálculo de preço) sem tocar no banco de dados
- **Testes de Integração:** Validam o fluxo completo desde a rota, passando pelo Controller e Service, até a persistência no banco de dados (usando um banco de teste)
- **Testes Parametrizados:** Executam o mesmo teste com múltiplos conjuntos de dados de entrada para cobrir edge-cases (ex: datas inválidas, valores negativos)

### 5.2. Comandos de Teste

- `npm test` - Executa toda a suíte
- `npm run test:coverage` - Gera relatório de cobertura de código (pasta `/coverage`)

## 6. Instalação e Execução
Este documento descreve como instalar, configurar e executar o **backend do projeto Travelar usando apenas Docker e Docker Compose**, incluindo:

- API Backend (Node.js + TypeScript)
- Banco de dados PostgreSQL
- PgAdmin (interface gráfica)
- Documentação Swagger

---

### 6.1. Pré-requisitos

Certifique-se de ter instalado:

- **Docker**
- **Docker Compose**
- Git
---

### 6.2. Clonar o repositório

```bash
git clone https://github.com/Travelar-Spot/Travelar-backend.git
cd Travelar-backend
```

---

### 6.3. Arquivo `.env` 

Deixe o arquivo .env assim:

```env
PORT=3000
NODE_ENV=production

DB_HOST=db
DB_PORT=5432
DB_DATABASE=travelar
DB_USERNAME=developer
DB_PASSWORD=password_exemplo

JWT_SECRET=sua_chave_jwt
```

---

### 6.4. Executando o Projeto

### 6.1 Construir e subir tudo

```bash
docker compose up --build
```

### 6.2 Rodar em segundo plano

```bash
docker compose up -d
```

### 6.3 Parar

```bash
docker compose down
```

---

### 6.5. Acessos importantes

| Serviço           | URL                                                              |
|-------------------|------------------------------------------------------------------|
| **Backend (API)** | [http://localhost:3000](http://localhost:3000)                   |
| **Swagger**       | [http://localhost:3000/api-docs](http://localhost:3000/api-docs) |
| **PgAdmin**       | [http://localhost:5050](http://localhost:5050)                   |
| **PostgreSQL**    | localhost:5432                                                   |

PgAdmin login (padrão configurado):

```
Email: admin@admin.com
Senha: admin
```

---

### 6.6. Executar Migrations (dentro do container)

### 6.6.1 Acessar o container

```bash
docker exec -it travelar-backend sh
```

### 6.6.2 Rodar migrations

```bash
npm run typeorm -- migration:run -d src/database/data-source.ts
```

---

### 6.7. Logs

### Backend

```bash
docker logs -f travelar-backend
```

### Postgres

```bash
docker logs -f travelar-db
```

---

### 6.8. Testes (fora do Docker)

_(Recomendado rodar localmente se quiser relatórios)_

```bash
npm test
npm run test:watch
npm run test:coverage
```
