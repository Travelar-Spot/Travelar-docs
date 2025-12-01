# Visão Geral do Projeto Travelar

<div align="center">
    <img src="https://raw.githubusercontent.com/Travelar-Spot/Travelar-docs/refs/heads/main/docs/assets/banner2.png" alt="Banner" width="100%">
    <br><br>
</div>

# Visão Geral do Projeto Travelar

O Travelar é uma plataforma de hospedagem projetada para conectar viajantes e anfitriões de maneira simples, intuitiva e segura. A plataforma oferece mecanismos de busca e filtros, cadastro e autenticação confiável, fluxo completo de reservas e avaliações pós-hospedagem, garantindo transparência e praticidade para ambos os perfis de usuários.

Seu desenvolvimento se apoia em uma arquitetura moderna e robusta, que integra de forma consistente o backend, responsável pelas regras de negócio e APIs, e o frontend, responsável pela interface visual responsiva e acessível.

Com testes automatizados, padronização de código, seguindo o Clean Code e SOLID e documentação clara, o Travelar busca entregar uma solução estável, escalável e preparada para futuras expansões.

Este projeto foi realizado com base no trabalho de Orientação a Objetos disponível em [OO-UNBGama](https://github.com/luciano-freitas-melo/OO-UNBGama).

## Destaques do Projeto

- **Funcionalidades Principais:** Autenticação de usuários, gestão de imóveis, fluxo completo de busca e reservas, e sistema de avaliações pós-hospedagem.

- **Público-Alvo:** Atende tanto hóspedes (foco na facilidade de reserva) quanto anfitriões (foco na administração de anúncios).

- **Escopo Técnico:** Inclui API Backend, API de autenticação, Frontend e testes automatizados.

- **Diretrizes de Qualidade:** O desenvolvimento segue padrões de Clean Code e SOLID para garantir um sistema seguro, testável e escalável.

- **Arquitetura do Sistema:** O sistema não é monolítico; ele opera através da orquestração de três componentes principais que se comunicam via protocolo HTTP/REST, garantindo desacoplamento de responsabilidades e escalabilidade independente.

## Organização do Código (Padrão Modular)

Internamente, a organização do código fonte adota uma Arquitetura Modular, padrão sugerido pelo professor Thiago. Ao invés de separar o código por camadas técnicas tradicionais (ex: uma pasta só para Controllers, outra só para Services), o projeto agrupa todos os arquivos relacionados a uma funcionalidade específica no mesmo diretório (ex: Imovel/, Reserva/). Isso facilita a manutenção e encapsula a lógica de cada domínio.

## 1. Estratégia de Microsserviços
A arquitetura divide o sistema em serviços especializados:

### 1.1. Travelar Auth API

Microserviço focado exclusivamente na gestão de identidades, hashing de senhas e emissão de tokens JWT, desacoplando dados sensíveis do restante da plataforma.

### 1.2. Travelar Backend

Núcleo responsável pelas regras de negócio (Imóveis, Reservas, Avaliações), organizado na arquitetura modular baseada em domínios citada acima.

## 2. Travelar Frontend
Interface moderna desenvolvida utilizando React, TypeScript e Vite. Atua como orquestradora, obtendo credenciais na Auth API para consumir os recursos protegidos do Backend Core.

## 3. Fluxo de Dados e Segurança
A comunicação é protegida via autenticação JWT (Bearer Token), onde o token assinado libera o acesso a rotas protegidas. A persistência de dados é garantida por um banco relacional PostgreSQL com integridade referencial rigorosa.

## 4. Infraestrutura e DevOps
Ambiente padronizado através do uso de Docker e Docker Compose para containerização da aplicação e banco de dados. O armazenamento de mídia é terceirizado para o serviço Cloudinary, otimizando a performance de entrega de imagens.

## 5. Estratégia de Qualidade e Testes
O projeto possui cobertura completa de testes em todas as camadas:

- **Backend:** Utiliza Jest para testes unitários, de integração e parametrizados.
- **Frontend:** Utiliza Cypress para testes automatizados End-to-End (E2E) que simulam a navegação real do usuário.

---