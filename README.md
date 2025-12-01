# Documentação Oficial – Travelar

<div align="center">
    <p><strong>1. Tela Inicial</strong></p>
    <img src="https://raw.githubusercontent.com/Travelar-Spot/Travelar-docs/refs/heads/main/docs/assets/banner.png" alt="Banner" width="100%">
    <br><br>
</div>

Documentação central do ecossistema **Travelar**, organizada para fornecer uma visão completa do projeto, desde planejamento até arquitetura, modelagem de dados e integrações entre os microsserviços.
[Link para a Documentação](https://travelar-spot.github.io/Travelar-docs)

---

## Visão Geral

Este repositório reúne toda a documentação do **Travelar**, uma plataforma moderna de hospedagem que conecta viajantes e anfitriões, oferecendo funcionalidades como autenticação, gestão de propriedades e reservas.

Aqui você encontrará:

* Planejamento do produto
* Protótipo e decisões de design
* Arquitetura completa da solução
* Documentação do Backend, Frontend e API de Autenticação
* Modelagem do Banco de Dados
* Diagramas e navegação entre repositórios

## Repositórios Oficiais do Projeto

A arquitetura do Travelar segue o princípio de **microsserviços desacoplados**, cada um versionado em seu próprio repositório.

A documentação aqui presente abrange todos eles.

### API de Autenticação

Gerencia registro, login, hashing, emissão de JWT e proteção de rotas.
[Link para o repositório](https://github.com/Travelar-Spot/Travelar-Auth-API)

---

### Backend 

Responsável pela lógica de negócios principal: propriedades, reservas, busca, disponibilidade e integrações internas.
[Link para o repositório](https://github.com/Travelar-Spot/Travelar-Backend)

---

### Frontend 

Aplicação React + TypeScript que consome os microsserviços e entrega a interface do usuário.
[Link para o repositório](https://github.com/Travelar-Spot/Travelar-Frontend)

---

## Estrutura dos Conteúdos

A documentação foi pensada para facilitar a leitura, a manutenção e a navegação:

### **1. Início**

Resumo do projeto, objetivos e motivação.

### **2. Planejamento**

* Histórias de usuário
* Backlog completo
* Diagramas iniciais (UML, casos de uso, fluxo de navegação)

### **3. Design & UX**

* Protótipos
* Layouts
* Fluxos de interação

### **4. Arquitetura**

Documentações detalhadas dos três repositórios:

* **API de Autenticação**
* **Backend**
* **Frontend**

Cada uma contendo:

* Estrutura de pastas
* Tecnologias
* Exemplos
* Fluxos internos
* Endpoints / responsabilidades

### **5. Banco de Dados**

* Modelagem lógica
* Relacionamentos
* Diagrama ER (Mermaid)
* Esquema PostgreSQL

---

## Como Executar a Documentação Localmente

### 1. Instalar dependências

```bash
pip install mkdocs-material
```

### 2. Rodar servidor local

```bash
mkdocs serve
```

### 3. Acessar

```
http://localhost:8000
```

---
