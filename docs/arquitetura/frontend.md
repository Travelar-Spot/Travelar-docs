
# Frontend Travelar

## 1. Visão Geral

O **Travelar Frontend** é a interface visual da plataforma, responsável por oferecer uma experiência rica e interativa para hóspedes e proprietários. O projeto consome as APIs de Backend e Autenticação para fornecer funcionalidades de busca, reserva e gestão de propriedades. O foco da arquitetura é robustez e testabilidade, utilizando TypeScript extensivamente e uma suíte dedicada de testes End-to-End (E2E).

O Frontend do projeto encontra-se hospedado na plataforma **Vercel**.


[Link para o repositório](https://github.com/Travelar-Spot/Travelar-frontend)

## 2. Tecnologias utilizadas

A camada de apresentação utiliza bibliotecas modernas do ecossistema JavaScript/TypeScript.

| Tecnologia | Função / Uso |
|------------|--------------|
| React | Biblioteca principal para construção de interfaces |
| TypeScript | Linguagem base, configurada para compilação ES5/CommonJS nos testes |
| Cypress | Framework de testes automatizados End-to-End |
| Axios/Fetch | Comunicação HTTP com os microsserviços Backend e Auth |
| Node.js | Ambiente de execução para ferramentas de build e testes |

## 3. Estrutura do Projeto

A organização de pastas reflete a separação entre código fonte da aplicação e ambiente de testes.

```
travelar-frontend/
├── cypress/               # Ambiente de Testes E2E
│   ├── e2e/               # Especificações dos testes (spec files)
│   ├── support/           # Comandos personalizados e configurações
│   └── tsconfig.json      # Configuração TS específica para o Cypress 
├── src/                   # Código Fonte da Aplicação
│   ├── components/        # Componentes reutilizáveis (Botões, Cards)
│   ├── pages/             # Componentes de Página (Home, Login, Detalhes)
│   ├── services/          # Integração com APIs (Auth e Core)
│   ├── context/           # Gerenciamento de estado global
│   └── styles/            # Arquivos de estilização (CSS/Sass)
├── cypress.config.ts      # Configuração global do Cypress
├── package.json           # Dependências e Scripts
└── tsconfig.json          # Configuração base do TypeScript
```

## 4. Configuração de Testes (Cypress)

O projeto possui uma configuração avançada de TypeScript dedicada aos testes, garantindo que o Cypress interprete corretamente a tipagem e módulos.

O arquivo `cypress/tsconfig.json` define:

- **Target:** `es5` (Compatibilidade máxima)
- **Tipos:** Inclui definições de `cypress` e `node` para intellisense
- **Módulos:** Utiliza `commonjs` e resolução `node` para alinhar com o ambiente de execução do executor de testes
- **Interoperabilidade:** `esModuleInterop: true` para facilitar importações de libs externas
- **Performance:** 
  - `noEmit: true` (execução em memória, sem gerar arquivos físicos)
  - `skipLibCheck: true` (compilação mais rápida)

## 5. Integração e Fluxos

O frontend orquestra a comunicação entre os serviços:

- **Autenticação:** Envia credenciais para a Auth API, armazena o JWT (local storage/cookie) e o anexa aos headers das requisições subsequentes
- **Imóveis:** Consome a Backend API para listar cards de imóveis na Home e exibir detalhes completos na página do produto
- **Reservas:** Gerencia formulários de data e envia requisições de reserva autenticadas

## 6. Scripts Disponíveis

No arquivo `package.json`, os seguintes comandos são padronizados:

- `npm start` - Inicia o servidor de desenvolvimento local
- `npm run build` - Gera os arquivos estáticos otimizados para produção
- `npm run cypress:open` - Abre a interface interativa de testes do Cypress
- `npm run cypress:run` - Executa os testes E2E em modo headless (terminal)

## 7. Instalação e Execução

### 7.1. Pré-requisitos

- Node.js 18+
- NPM
- Git
- **Backend funcionando**

### 7.2. Instalação

### 7.2.1. Clonar o repositório

```bash
git clone https://github.com/seu-usuario/Travelar-frontend.git
cd Travelar-frontend
```

### 7.2.2. Instalar dependências

```bash
npm install
```

### 7.3. Configuração

### 7.3.1 Criar arquivo `.env`

```env
VITE_API_URL=http://localhost:3333
```

*Altere conforme porta real do backend.*

### 7.4. Executando

### 7.4.1. Modo Desenvolvimento

```bash
npm run dev
```

**Acessar:** http://localhost:5173

### 7.4.2. Build de produção

```bash
npm run build
```

### 7.4.3. Visualizar build

```bash
npm run preview
```

### 7.5. Testes (Cypress)

### 7.5.1 Modo interativo

```bash
npm run test:e2e:open
```

### 7.5.2 Headless

```bash
npm run test:e2e
```

### 7.6. Qualidade de Código

```bash
npm run lint              # Analisa o código
npm run lint:eslint       # ESLint
npm run lint:eslint:fix   # Corrige ESLint
npm run format            # Formata com Prettier
```
