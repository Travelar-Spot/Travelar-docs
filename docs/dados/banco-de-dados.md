# Documentação do Banco de Dados - Travelar

Este documento detalha o modelo Físico e DDL do banco de dados doprojeto Travelar.

---

## 1. Modelo Físico

<div style="text-align: center;">
<img src="https://github.com/Travelar-Spot/Travelar-docs\docs\dados\assets\modelo_fisico_travelar.png" alt="Modelo Físico" width="100%">
</div>

---

## 2. DDL

```sql
BEGIN;

CREATE TABLE IF NOT EXISTS public.avaliacoes (
  id integer NOT NULL DEFAULT nextval('avaliacoes_id_seq'::regclass),
  nota integer NOT NULL CHECK (nota >= 1 AND nota <= 5),
  comentario text,
  autorId integer,
  imovelId integer,
  CONSTRAINT avaliacoes_pkey PRIMARY KEY (id),
  CONSTRAINT FOREIGN KEY (autorId) REFERENCES public.usuarios(id),
  CONSTRAINT FOREIGN KEY (imovelId) REFERENCES public.imoveis(id)
);
CREATE TABLE IF NOT EXISTS public.imoveis (
  id integer NOT NULL DEFAULT nextval('imoveis_id_seq'::regclass),
  titulo character varying NOT NULL,
  descricao character varying NOT NULL,
  tipo USER-DEFINED NOT NULL,
  endereco character varying NOT NULL,
  cidade character varying NOT NULL,
  precoPorNoite numeric NOT NULL,
  capacidade integer NOT NULL,
  disponivel boolean NOT NULL DEFAULT true,
  foto character varying,
  proprietarioId integer,
  CONSTRAINT imoveis_pkey PRIMARY KEY (id),
  CONSTRAINT FOREIGN KEY (proprietarioId) REFERENCES public.usuarios(id)
);
CREATE TABLE IF NOT EXISTS public.reservas (
  id integer NOT NULL DEFAULT nextval('reservas_id_seq'::regclass),
  dataInicio date NOT NULL,
  dataFim date NOT NULL,
  valorTotal numeric NOT NULL,
  status USER-DEFINED NOT NULL DEFAULT 'PENDENTE'::reservas_status_enum,
  criadoEm timestamp without time zone NOT NULL DEFAULT now(),
  clienteId integer,
  imovelId integer,
  CONSTRAINT reservas_pkey PRIMARY KEY (id),
  CONSTRAINT  FOREIGN KEY (clienteId) REFERENCES public.usuarios(id),
  CONSTRAINT reservas_imovelId_fkey FOREIGN KEY (imovelId) REFERENCES public.imoveis(id)
);
CREATE TABLE IF NOT EXISTS public.usuarios (
  id integer NOT NULL DEFAULT nextval('usuarios_id_seq'::regclass),
  nome character varying NOT NULL,
  email character varying NOT NULL UNIQUE,
  telefone character varying NOT NULL,
  role USER-DEFINED NOT NULL,
  criadoEm timestamp without time zone NOT NULL DEFAULT now(),
  senhaHash character varying NOT NULL,
  foto text,
  CONSTRAINT usuarios_pkey PRIMARY KEY (id)
);

END;
```

---

## 3. Detalhamento das Tabelas (Entidades)

A seguir, um detalhamento de cada tabela, alinhado com o schema físico do banco de dados.

### 3.1. Tabela `usuarios`

Armazena as informações de todos os usuários cadastrados na plataforma, sejam eles clientes, proprietários ou ambos.

| Coluna      | Tipo de Dado   | Restrições / Descrição                                               |
|-------------|----------------|----------------------------------------------------------------------|
| `id`        | `INTEGER`      | Chave primária (PK), auto-incremento.                                |
| `nome`      | `VARCHAR`      | Nome completo do usuário. Obrigatório.                               |
| `email`     | `VARCHAR`      | Endereço de e-mail. Deve ser único (UNIQUE). Obrigatório.           |
| `telefone`  | `VARCHAR`      | Número de telefone para contato. Obrigatório.                        |
| `role`      | `USER-DEFINED` | Define o papel do usuário (ex: CLIENTE, PROPRIETARIO). Obrigatório. |
| `criadoEm`  | `TIMESTAMP`    | Data e hora de criação do registro. Default: `now()`.               |
| `senhaHash` | `VARCHAR`      | Hash da senha do usuário para autenticação segura. Obrigatório.     |
| `foto`      | `TEXT`         | URL ou caminho para a foto de perfil do usuário.                    |

### 3.2. Tabela `imoveis`

Contém todos os imóveis cadastrados e disponíveis (ou não) na plataforma.

| Coluna           | Tipo de Dado   | Restrições / Descrição                                           |
|------------------|----------------|------------------------------------------------------------------|
| `id`             | `INTEGER`      | Chave primária (PK), auto-incremento.                            |
| `titulo`         | `VARCHAR`      | Título do anúncio do imóvel. Obrigatório.                        |
| `descricao`      | `VARCHAR`      | Descrição detalhada do imóvel. Obrigatório.                      |
| `tipo`           | `USER-DEFINED` | Tipo do imóvel (ex: CASA, APARTAMENTO). Obrigatório.            |
| `endereco`       | `VARCHAR`      | Endereço completo do imóvel. Obrigatório.                        |
| `cidade`         | `VARCHAR`      | Cidade onde o imóvel está localizado. Obrigatório.               |
| `precoPorNoite`  | `NUMERIC`      | Valor da diária do aluguel. Obrigatório.                         |
| `capacidade`     | `INTEGER`      | Número máximo de hóspedes que o imóvel suporta. Obrigatório.     |
| `disponivel`     | `BOOLEAN`      | Indica disponibilidade. Padrão: `true`.                          |
| `foto`           | `VARCHAR`      | URL ou caminho para a foto principal do imóvel.                  |
| `proprietarioId` | `INTEGER`      | Chave estrangeira (FK) referenciando `usuarios(id)`.             |

### 3.3. Tabela `reservas`

Registra todas as solicitações de reservas feitas pelos clientes.

| Coluna       | Tipo de Dado   | Restrições / Descrição                                               |
|--------------|----------------|----------------------------------------------------------------------|
| `id`         | `INTEGER`      | Chave primária (PK), auto-incremento.                                |
| `dataInicio` | `DATE`         | Data de início da hospedagem. Obrigatório.                           |
| `dataFim`    | `DATE`         | Data de término da hospedagem. Obrigatório.                          |
| `valorTotal` | `NUMERIC`      | Valor total calculado da reserva. Obrigatório.                       |
| `status`     | `USER-DEFINED` | Status da reserva (ex: PENDENTE, CONFIRMADA). Padrão: `'PENDENTE'`. |
| `criadoEm`   | `TIMESTAMP`    | Data e hora em que a reserva foi solicitada. Default: `now()`.      |
| `clienteId`  | `INTEGER`      | Chave estrangeira (FK) referenciando `usuarios(id)`.                 |
| `imovelId`   | `INTEGER`      | Chave estrangeira (FK) referenciando `imoveis(id)`.                  |

### 3.4. Tabela `avaliacoes`

Guarda as avaliações (notas e comentários) que os clientes fazem sobre os imóveis.

| Coluna       | Tipo de Dado | Restrições / Descrição                                       |
|--------------|--------------|--------------------------------------------------------------|
| `id`         | `INTEGER`    | Chave primária (PK), auto-incremento.                        |
| `nota`       | `INTEGER`    | Nota da avaliação. Restrição: `>= 1` e `<= 5`. Obrigatório. |
| `comentario` | `TEXT`       | Comentário descritivo da avaliação.                          |
| `autorId`    | `INTEGER`    | Chave estrangeira (FK) referenciando `usuarios(id)`.         |
| `imovelId`   | `INTEGER`    | Chave estrangeira (FK) referenciando `imoveis(id)`.          |

---

## 4. Relacionamentos

- **Usuário e Imóvel**: Um `USUARIO` pode cadastrar múltiplos `IMOVEIS` (proprietário), mas cada `IMOVEL` pertence a um único `USUARIO` (`proprietarioId`).
- **Usuário e Reserva**: Um `USUARIO` pode realizar múltiplas `RESERVAS` (cliente), mas cada `RESERVA` está vinculada a um único `USUARIO` (`clienteId`).
- **Imóvel e Reserva**: Um `IMOVEL` pode ter múltiplas `RESERVAS`, mas cada `RESERVA` refere-se a um único `IMOVEL` (`imovelId`).
- **Usuário e Avaliação**: Um `USUARIO` pode escrever múltiplas `AVALIACOES`, mas cada `AVALIACAO` possui um único autor (`autorId`).
- **Imóvel e Avaliação**: Um `IMOVEL` pode receber múltiplas `AVALIACOES`, mas cada `AVALIACAO` pertence a um único `IMOVEL` (`imovelId`).

---