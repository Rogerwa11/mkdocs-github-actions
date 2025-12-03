

# Documentação Técnica Integral do Sistema NeedUK

**Versão:** 1.0.0  
**Data:** 2025-12-03  
**Autores:** Roger (Dev Líder), Luan (Analista de Documentação / Dev Auxiliar), Nicolly (Analista de Documentação), Fernanda (Analista de Documentação)  
**Classificação:** Confidencial — Uso Interno / Acadêmico

---

## Visão Executiva

O NeedUK é uma plataforma acadêmica de empregabilidade que conecta estudantes, recrutadores e gestores universitários. Esta documentação consolida decisões estratégicas, requisitos técnicos e padrões operacionais necessários para operação, auditoria e evolução do produto. Utilize o sumário temático abaixo para navegar rapidamente pelos tópicos conforme sua função (produto, arquitetura, segurança, operações ou governança).

---

## Sumário Temático

1. [Introdução](#1-introducao)
2. [Visão Geral do Sistema](#2-visao-geral-do-sistema)
3. [Escopo e Limitações](#3-escopo-e-limitacoes)
4. [Público-Alvo e Perfis de Usuário](#4-publico-alvo-e-perfis-de-usuario)
5. [Requisitos Detalhados](#5-requisitos-detalhados)  
   5.1 Funcionais · 5.2 Não Funcionais
6. [Arquitetura Técnica e Lógica](#6-arquitetura-tecnica-e-logica)  
   Diagrama · Componentes · Responsabilidades
7. [Modelagem de Dados (ERD e Esquema)](#7-modelagem-de-dados-erd-e-esquema)
8. [API, Endpoints, Contratos e Exemplos](#8-api-endpoints-contratos-e-exemplos)
9. [Validação e Schemas (Zod)](#9-validacao-e-schemas-zod)
10. [Fluxos Críticos](#10-fluxos-criticos)  
    Registro → Vagas → Candidatura · Atividades Colaborativas
11. [Segurança, Criptografia e Conformidade](#11-seguranca-criptografia-e-conformidade)
12. [Qualidade, Testes e Métricas](#12-qualidade-testes-e-metricas)
13. [Infraestrutura, Deploy e CI/CD](#13-infraestrutura-deploy-e-cicd)
14. [Operações e Observabilidade](#14-operacoes-e-observabilidade)
15. [Backup, Recovery e Continuidade](#15-backup-recovery-e-plano-de-continuidade)
16. [Gestão de Projeto e Governança](#16-gestao-de-projeto-e-governanca)
17. [Riscos, Mitigações e Registro](#17-riscos-mitigacoes-e-registro)
18. [Roadmap e Evolução](#18-roadmap-e-evolucao)
19. [Padrões de Código e Processos de Contribuição](#19-padroes-de-codigo-e-processos-de-contribuicao)
20. [Anexos (Modelos, Runbooks e Checklists)](#20-anexos-modelos-runbooks-e-checklists)

---

# 1. Introdução

> **Resumo:** descreve o propósito, alcance e padrões normativos que balizam toda a documentação técnica do NeedUK.

### 1.1 Objetivo do Documento

Consolidar uma visão única, formal e atualizada do sistema **NeedUK**, detalhando requisitos, arquitetura, design de dados, contratos de API, políticas de segurança, estratégias de qualidade e governança. O material serve como referência oficial para equipes de desenvolvimento, auditoria, segurança, operações e stakeholders envolvidos na evolução do produto.

### 1.2 Alcance

A documentação abrange a versão atual do NeedUK — autenticação, gestão de perfis, atividades colaborativas, vagas, candidaturas, notificações em tempo real e dashboards responsivos — e antecipa diretrizes do módulo de Currículos. Ficam fora de escopo o código-fonte integral e configurações sigilosas de provedores, embora melhores práticas e padrões sejam descritos.

### 1.3 Padrões e Referências

- Requisitos: IEEE 830 / ISO/IEC 29110  
- Gestão ágil: Scrum híbrido com sprints quinzenais  
- Segurança: OWASP Top 10 como baseline mínimo de mitigação

---

# 2. Visão Geral do Sistema

> **Resumo:** apresenta o posicionamento do NeedUK no ecossistema acadêmico-profissional e destaca funcionalidades-chave distribuídas por persona.

### 2.1 Propósito

Promover conexões qualificadas entre estudantes, recrutadores e gestores universitários. O sistema oferece autenticação segura, gestão de perfis completos, organização de atividades colaborativas, administração de vagas e candidaturas, além de notificações em tempo real que sustentam um ciclo contínuo de empregabilidade acadêmica.

### 2.2 Principais Funcionalidades

- Autenticação unificada com Better Auth e gerenciamento de sessões resiliente  
- Perfis segmentados (Aluno, Recrutador, Gestor) com edição abrangente  
- Atividades colaborativas com convites, transferência de liderança, observações e medalhas  
- Vagas com rascunhos, filtros avançados por curso e ordenação customizada  
- Candidaturas com carta de apresentação, histórico de decisões e notificações  
- Notificações em tempo real com políticas de limpeza automática  
- Dashboards responsivos por perfil com métricas e atalhos operacionais  
- Currículos estruturados (módulo planejado) com suporte futuro a exportação

---

# 3. Escopo e Limitações

> **Resumo:** delimita fronteiras funcionais e aponta evoluções planejadas para orientar expectativas de stakeholders.

### 3.1 Escopo Incluído

- Backend REST com validação via Zod  
- Frontend Next.js (App Router + Server/Client Components)  
- Persistência PostgreSQL (Supabase) via Prisma  
- Notificações em tempo real (Supabase Realtime ou WebSocket dedicado)  
- Autenticação e autorização com Better Auth  
- Validações, testes unitários e integrados obrigatórios

### 3.2 Fora de Escopo (versão atual)

- Módulo de Currículos (detalhado no roadmap)  
- Integração nativa com provedores sociais (LinkedIn, Google OAuth)  
- Chat ponto a ponto em tempo real  
- Recomendação inteligente de vagas baseada em ML

---

# 4. Público-Alvo e Perfis de Usuário

> **Resumo:** define personas, responsabilidades e níveis de acesso utilizados pelo modelo RBAC.

### 4.1 Perfis

- **Aluno:** gerencia currículo acadêmico, participa de atividades e se candidata a vagas.  
- **Recrutador:** publica oportunidades, analisa candidaturas e comunica decisões.  
- **Gestor:** representa a instituição, concede medalhas e modera atividades colaborativas.  
- **Admin / DevOps:** sustenta operações de infraestrutura, observabilidade e governança técnica.

### 4.2 Permissões (RBAC)

- **Aluno:** acesso a vagas, candidatura, gestão de perfil e participação em atividades.  
- **Recrutador:** CRUD de vagas, avaliação de candidaturas e consulta de métricas próprias.  
- **Gestor:** privilégios de Recrutador + concessão de medalhas e curadoria ampliada de atividades.  
- **Admin:** administração avançada (logs, manutenção de usuários, execução de rotinas críticas).

---

# 5. Requisitos Detalhados

> **Resumo:** consolida requisitos funcionais e não funcionais priorizados para garantir valor de negócio, qualidade e conformidade.

## 5.1 Requisitos Funcionais (RF)

!!! info "Mapa rápido dos RFs"
    | Código | Tema                       | Status | Responsável técnico |
    | ------ | -------------------------- | ------ | ------------------- |
    | RF01   | Autenticação e sessões     | Ativo  | Plataforma          |
    | RF02   | Gestão de perfis           | Ativo  | Frontend & API      |
    | RF03   | Atividades colaborativas   | Ativo  | Produto Colaborativo |
    | RF04   | Vagas                      | Ativo  | Recrutamento        |
    | RF05   | Candidaturas               | Ativo  | Recrutamento        |
    | RF06   | Notificações em tempo real | Ativo  | Realtime            |
    | RF07   | Dashboards                 | Ativo  | Analytics           |
    | RF08   | Currículos (planejado)     | Planejado | Produto Colaborativo |

### RF01 — Autenticação e Sessões

- **Descrição:** garantir registro, login, logout e recuperação de acesso.  
- **Pré-condição:** disponibilidade dos endpoints de autenticação.  
- **Critérios de aceitação:**  
  - Registro dispara e-mail de verificação.  
  - Login retorna sessão válida (cookie HttpOnly + token).  
  - Rotas protegidas negam acesso com 401 a usuários não autenticados.  
  - Implementação baseada em Better Auth para sessões + refresh tokens.  
- **Notas de implementação:** tokens curtos (≤ 15 min), refresh seguro, logout invalida refresh no servidor.

### RF02 — Gestão de Perfis

- **Descrição:** permitir visualização e edição de dados pessoais, acadêmicos e reputacionais.  
- **Campos mínimos:** nome, e-mail, curso, semestre, bio, foto, links relevantes.  
- **Critérios:** alterações refletidas imediatamente; admite consistência eventual para camadas de cache.

### RF03 — Atividades Colaborativas

- **Descrição:** criar e gerenciar atividades com título, descrição, horários, visibilidade e lideranças.  
- **Fluxo padrão:** criar → convidar → aceitar → participar → registrar observações/links → conceder medalhas.  
- **Regras:** somente o líder transfere liderança; gestores concedem medalhas.

### RF04 — Vagas

- **Descrição:** CRUD completo com rascunhos, filtros por curso/palavras-chave/status e ordenação customizada.  
- **Regras especiais:** vagas aceitas permanecem visíveis para o candidato (mesmo fechadas) e ganham destaque no topo.

### RF05 — Candidaturas

- **Descrição:** submissão com carta de apresentação, links e anexos; registro de histórico e notificações.  
- **Estados:** `PENDENTE` → `ACEITO` / `RECUSADO` → (opcional) `RETIRADO`.

### RF06 — Notificações em Tempo Real

- **Descrição:** comunicar convites, decisões, medalhas e eventos críticos em tempo real.  
- **Comportamento:** entrega via WebSocket com fallback para polling.  
- **Limpeza:** notificações lidas e antigas (X dias configurável) entram em rotina de limpeza automática.

### RF07 — Dashboards

- **Descrição:** painel adaptado por perfil com visão de vagas, candidaturas, atividades e alertas.  
- **Métricas:** contadores de candidaturas, taxa de aceitação, atividades ativas, notificações pendentes.

### RF08 — Currículos (Planejado)

- **Descrição:** CRUD de currículos estruturados, upload de arquivos, compartilhamento por link e exportação em PDF.  
- **Status:** em especificação detalhada (referência em Roadmap).

## 5.2 Requisitos Não Funcionais (RNF)

!!! note "Metas globais de RNF"
    - **Desempenho:** < 300 ms (p95) para 500 usuários simultâneos.  
    - **Disponibilidade:** 99% com monitoramento automatizado.  
    - **Segurança:** OWASP Top 10 como linha de base.  
    - **Qualidade:** cobertura de testes ≥ 70% nos módulos críticos.

### RNF01 — Desempenho

- **Meta:** 95% das requisições respondem em < 300 ms considerando até 500 usuários simultâneos.

### RNF02 — Escalabilidade

- **Meta:** arquitetura apta a escala horizontal, com pool de conexões e cache Redis para cenários de alta leitura.

### RNF03 — Segurança

- **Requisitos:** HTTPS obrigatório, proteção CSRF, sanitização de inputs, hashing (bcrypt/argon2), rate limiting anti brute-force.

### RNF04 — Disponibilidade

- **SLA:** alvo de 99% com monitoramento contínuo (UptimeRobot/Prometheus).

### RNF05 — Manutenibilidade

- **Requisitos:** código TypeScript com ≥ 70% de cobertura unitária nos módulos críticos, revisão obrigatória de PRs e linting via ESLint.

### RNF06 — Usabilidade e Acessibilidade

- **Requisitos:** conformidade com WCAG 2.1 AA em telas prioritárias (login, perfil, candidatura).

---

# 6. Arquitetura Técnica e Lógica

> **Resumo:** apresenta a macroarquitetura, os componentes principais e suas responsabilidades na plataforma.

## 6.1 Visão Geral

Arquitetura modular com fronteiras claras entre experiência do usuário (Next.js), lógica de domínio (camada `lib/*`), serviços de API (Server Components e API Routes) e persistência (Prisma + PostgreSQL).

## 6.2 Diagrama Lógico (ASCII)

```
+-----------------------+         +------------------+         +------------------------+
|   Browser / Client    | <-----> |    Next.js App   | <-----> |     Supabase / DB      |
| (React, Tailwind UI)  | WebSocket| (Server/Client) |  REST   | PostgreSQL + Realtime  |
+-----------------------+         +------------------+         +------------------------+
        |  ^                           |   ^                          |    ^
        |  |                           |   |                          |    |
        v  |                           v   |                          v    |
+-----------------------+         +------------------+         +------------------------+
|  CDN / Assets (Vercel)|         |   Redis Cache    |         |   Object Storage (S3)  |
+-----------------------+         +------------------+         +------------------------+
```

## 6.3 Componentes e Responsabilidades

- **Frontend (Next.js):**  
  Server Components para SSR/SSG, App Router para navegação, biblioteca de componentes em `components/ui/` e hooks em `hooks/` (`useAuth`, `useNotifications`).

- **API / Backend:**  
  Rotas em `src/app/api/` para domínios de autenticação, perfil, atividades, vagas e notificações. Regras de negócio concentradas em `lib/` com validações via Zod.

- **Banco de Dados:**  
  PostgreSQL (Supabase) orquestrado pelo Prisma Client, com entidades centrais `User`, `Vacancy`, `Application`, `Activity`, `Notification` e `Medal`.

- **Realtime / Notificações:**  
  Streaming via Supabase Realtime ou WebSocket dedicado, com fallback para SSE/long-polling.

- **Cache:**  
  Redis opcional para respostas intensivas de leitura e sessões.

- **Storage:**  
  Supabase Storage ou Amazon S3 para uploads de imagens de perfil e currículos.

!!! tip "Governança de componentes"
    Mantenha diagramas atualizados a cada entrega relevante e registre decisões arquiteturais no ADR correspondente (`docs/adr/`), assegurando rastreabilidade.

---

# 7. Modelagem de Dados (ERD e Esquema)

> **Resumo:** descreve a modelagem relacional, campos essenciais, índices e exemplo de implementação com Prisma.

## 7.1 Tabelas Principais

### `users`

* `id` UUID PK
* `email` VARCHAR UNIQUE NOT NULL
* `name` VARCHAR NOT NULL
* `password_hash` TEXT NULL (nullable se login via OAuth)
* `role` ENUM('ALUNO','RECRUTADOR','GESTOR','ADMIN') DEFAULT 'ALUNO'
* `course` VARCHAR NULL
* `bio` TEXT NULL
* `avatar_url` TEXT NULL
* `medal_count` INT DEFAULT 0
* `created_at` TIMESTAMP DEFAULT now()
* `updated_at` TIMESTAMP DEFAULT now()

**Índices:** `email (unique)`, `role`, `course`

---

!!! info "Convenções de nomenclatura"
    - Campos em inglês, snake_case no banco e camelCase no Prisma.  
    - Tabelas auditáveis devem conter `created_at` e `updated_at` com gatilhos automáticos.  
    - Foreign keys nomeadas com sufixo `_id`.

### `vacancies`

* `id` UUID PK
* `title` VARCHAR NOT NULL
* `description` TEXT NOT NULL
* `requirements` JSONB NULL
* `recruiter_id` UUID FK -> users(id)
* `target_course` VARCHAR NULL
* `status` ENUM('RASCUNHO','ABERTA','FECHADA','INATIVA') DEFAULT 'RASCUNHO'
* `is_featured` BOOLEAN DEFAULT false
* `created_at`, `updated_at`, `published_at` TIMESTAMPS

**Índices:** `recruiter_id`, `status`, `target_course`

---

### `applications` (candidaturas)

* `id` UUID PK
* `vacancy_id` UUID FK -> vacancies(id)
* `user_id` UUID FK -> users(id)
* `cover_letter` TEXT NULL
* `attachments` JSONB NULL (links/ids para arquivos)
* `status` ENUM('PENDENTE','ACEITO','RECUSADO','RETIRADO') DEFAULT 'PENDENTE'
* `applied_at` TIMESTAMP
* `decision_at` TIMESTAMP NULL
* `decision_by` UUID FK -> users(id) NULL

**Índices:** `vacancy_id`, `user_id`, `status`

---

### `activities`

* `id` UUID PK
* `title` VARCHAR
* `description` TEXT
* `leader_id` UUID FK -> users(id)
* `visibility` ENUM('PUBLICA','PRIVADA')
* `start_at` TIMESTAMP NULL
* `end_at` TIMESTAMP NULL

---

### `activity_participants`

* `id` UUID PK
* `activity_id` FK -> activities(id)
* `user_id` FK -> users(id)
* `role` ENUM('PARTICIPANTE','LIDER')
* `status` ENUM('PENDENTE','ACEITO','RECUSADO')
* `joined_at` TIMESTAMP

---

### `notifications`

* `id` UUID PK
* `user_id` FK -> users(id)
* `type` VARCHAR (eg. 'INVITE','CANDIDATE_DECISION','MEDAL_AWARDED')
* `payload` JSONB (dados contextualizados)
* `read` BOOLEAN DEFAULT false
* `created_at` TIMESTAMP

**Rotina de limpeza:** job `cleanup:notifications` -> remove notificações marcadas como lidas há mais de 1 hora (conforme README).

---

### `medals`

* `id` UUID PK
* `user_id` FK -> users(id)
* `name` VARCHAR
* `description` TEXT
* `awarded_by` UUID FK -> users(id)
* `awarded_at` TIMESTAMP

---

### `sessions` / `accounts` / `verifications`

* Estruturas auxiliares para Better Auth (conforme necessidade da biblioteca).

---

## 7.2 Exemplo de Schema Prisma

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id          String    @id @default(uuid())
  email       String    @unique
  name        String
  passwordHash String?
  role        Role      @default(ALUNO)
  course      String?
  bio         String?
  avatarUrl   String?
  medalCount  Int       @default(0)
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt

  vacancies   Vacancy[] @relation("RecruiterVacancies")
  applications Application[]
  activities  ActivityParticipant[]
  notifications Notification[]
  medals      Medal[]
}

enum Role {
  ALUNO
  RECRUTADOR
  GESTOR
  ADMIN
}

model Vacancy {
  id           String       @id @default(uuid())
  title        String
  description  String
  requirements Json?
  recruiterId  String
  recruiter    User         @relation("RecruiterVacancies", fields: [recruiterId], references: [id])
  targetCourse String?
  status       VacancyStatus @default(RASCUNHO)
  isFeatured   Boolean      @default(false)
  createdAt    DateTime     @default(now())
  updatedAt    DateTime     @updatedAt
  publishedAt  DateTime?

  applications Application[]
}

enum VacancyStatus {
  RASCUNHO
  ABERTA
  FECHADA
  INATIVA
}

model Application {
  id          String    @id @default(uuid())
  vacancy     Vacancy   @relation(fields: [vacancyId], references: [id])
  vacancyId   String
  user        User      @relation(fields: [userId], references: [id])
  userId      String
  coverLetter String?
  attachments Json?
  status      AppStatus @default(PENDENTE)
  appliedAt   DateTime  @default(now())
  decisionAt  DateTime?
  decisionBy  String?
}

enum AppStatus {
  PENDENTE
  ACEITO
  RECUSADO
  RETIRADO
}

model Notification {
  id        String   @id @default(uuid())
  user      User     @relation(fields: [userId], references: [id])
  userId    String
  type      String
  payload   Json
  read      Boolean  @default(false)
  createdAt DateTime @default(now())
}

model Medal {
  id         String   @id @default(uuid())
  user       User     @relation(fields: [userId], references: [id])
  userId     String
  name       String
  description String?
  awardedBy  String
  awardedAt  DateTime @default(now())
}
```

---

# 8. API, Endpoints, Contratos e Exemplos

> **Resumo:** cataloga convenções de API, endpoints prioritários e payloads de referência para integração e QA.

> As rotas são organizadas sob `src/app/api/...`. Priorize Server Actions (Next.js) quando aplicável e mantenha REST para interoperabilidade externa.

## 8.1 Convenções

* Todos os endpoints retornam JSON.
* Padrão de sucesso: status 200/201 com corpo `{ success: true, data: ... }`.
* Erros: `{ success: false, error: { code: "...", message: "..." } }`.
* Autenticação: cookie HttpOnly + header `Authorization: Bearer <token>` para APIs externas.
* Paginação: query `?page=1&limit=20`.
* Filtros: `?course=Engenharia&status=ABERTA`.

!!! warning "Autenticação para integrações externas"
    Para consumo por terceiros, exija `Authorization: Bearer <token>` emitido por canal seguro e restrinja escopos com base nas permissões RBAC.

---

## 8.2 Autenticação

### `POST /api/auth/register`

**Entrada (JSON):**

```json
{
  "email": "user@example.com",
  "password": "StrongP@ssw0rd",
  "name": "Nome Completo",
  "role": "ALUNO"
}
```

**Validações:** email válido, senha com política (min 8 chars, mix), role permitida.

**Resposta (201):**

```json
{
  "success": true,
  "data": { "id": "uuid", "email": "user@example.com", "verified": false }
}
```

---

### `POST /api/auth/login`

**Entrada:**

```json
{ "email": "user@example.com", "password": "StrongP@ssw0rd" }
```

**Resposta (200):**

* Cookie `session` HttpOnly
* Body:

```json
{
  "success": true,
  "data": {
    "user": { "id": "...", "email": "..." },
    "token": "jwt.token.here",
    "expiresIn": 900
  }
}
```

---

### `POST /api/auth/logout`

* Invalida sessão e refresh token.

---

## 8.3 Perfis

### `GET /api/profile/me`

* Retorna perfil do usuário autenticado.
* Resposta: `User` + contadores (medal_count, activities_count, applications_count).

### `PUT /api/profile/me`

* Atualiza campos permitidos (name, course, bio, avatar_url).
* Validação Zod.

### `POST /api/profile/upload-image`

* Upload multipart/form-data → armazena em Storage (retorna URL).

---

## 8.4 Atividades

### `POST /api/activities`

* Cria atividade (somente autenticados).
* Payload:

```json
{
  "title": "Projeto X",
  "description": "Descrição...",
  "visibility": "PUBLICA",
  "startAt": "2025-12-10T14:00:00Z",
  "endAt": "2025-12-10T16:00:00Z",
  "links": [{ "title": "Repo", "url": "https://..." }]
}
```

* Lógica: cria Activity + ActivityParticipant (lider).

### `POST /api/activities/:id/invite`

* Body: `{ "emails": ["a@b.com"], "message": "..." }`
* Envia convites via e-mail + cria ActivityInvitation registros.

### `PATCH /api/activities/:id/transfer-lead`

* Body: `{ "newLeaderId": "uuid" }`
* Somente liderança atual pode transferir.

---

## 8.5 Vagas

### `POST /api/vacancies`

* Criar vaga (recruiter only).
* Possíveis campos: title, description, requirements (JSON), targetCourse, status (RASCUNHO|ABERTA), isFeatured.

### `GET /api/vacancies`

* Query params: `page`, `limit`, `course`, `q`, `sort`, `onlyAcceptedForUser=true`
* Ordenação especial: vagas aceitas pelo usuário aparecem on-top, mesmo se fechadas.

### `PATCH /api/vacancies/:id/publish`

* Define `publishedAt` e status 'ABERTA'.

---

## 8.6 Candidaturas

### `POST /api/vacancies/:id/apply`

* Body: `{ "coverLetter": "...", "links": ["..."], "attachments": [<storage-ids>] }`
* Regras: impede duplicate application; cria Application registro; notifica recruiter.

### `PATCH /api/vacancies/:id/applications/:applicationId`

* Body: `{ "decision": "ACEITO", "notes": "Comentário interno" }`
* A ação gera `notification` para candidato com payload.

---

## 8.7 Notificações

### `GET /api/notifications?limit=20&page=1`

* Retorna notificações do usuário ordenadas por `created_at DESC`.

### `PATCH /api/notifications/:id/read`

* Marca como lida.

### `DELETE /api/notifications/cleanup`

* Roda job de limpeza (executado via script `npm run cleanup:notifications`).

---

## 8.8 Medalhas

### `GET /api/medals`

* Lista medalhas e meta-informações.

### `POST /api/medals/award`

* Body: `{ "userId": "uuid", "name": "Contribuição Exemplo", "reason": "..." }`
* Apenas gestores podem conceder.

---

## 8.9 Erros e Códigos Comuns

| HTTP | Código interno    | Significado                           |
| ---- | ----------------- | ------------------------------------- |
| 400  | `INVALID_INPUT`   | Payload inválido                      |
| 401  | `UNAUTHENTICATED` | Sessão expirada / inválida            |
| 403  | `FORBIDDEN`       | Acesso negado                         |
| 404  | `NOT_FOUND`       | Recurso não encontrado                |
| 409  | `CONFLICT`        | Conflito (ex.: candidatura duplicada) |
| 429  | `RATE_LIMIT`      | Excesso de requisições                |
| 500  | `INTERNAL_ERROR`  | Erro servidor                         |

---

# 9. Validação e Schemas (Zod)

> **Resumo:** define estratégia centralizada de validação com Zod para garantir contratos consistentes e segurança de entrada.

> Para centralização, criar pasta `src/lib/schemas/` com validações Zod por rota.

### Exemplo: Schema `CreateVacancySchema`

```ts
import { z } from "zod";

export const CreateVacancySchema = z.object({
  title: z.string().min(5).max(150),
  description: z.string().min(10),
  requirements: z.array(z.string()).optional(),
  targetCourse: z.string().optional().nullable(),
  status: z.enum(["RASCUNHO","ABERTA","FECHADA","INATIVA"]).default("RASCUNHO"),
  isFeatured: z.boolean().optional().default(false)
});
```

### Exemplo: `ApplySchema`

```ts
export const ApplySchema = z.object({
  coverLetter: z.string().min(20).max(5000).optional(),
  links: z.array(z.string().url()).optional(),
  attachments: z.array(z.string()).optional()
});
```

---

# 10. Fluxos Críticos

> **Resumo:** detalha sequências ponta a ponta que suportam jornadas de maior risco operacional ou de negócio.

## 10.1 Fluxo: Registro → Vaga → Candidatura → Decisão

1. Usuário registra-se (verify email).
2. Recrutador cria vaga e publica.
3. Aluno encontra vaga via buscas/filtros.
4. Aluno clica “Candidatar”, preenche carta e envia.
5. Sistema cria `application` e notifica recrutador.
6. Recrutador avalia e decide (ACEITO/RECUSADO).
7. Sistema registra decisão, seta `decision_at` e notifica candidato.
8. Se ACEITO, vaga é marcada para aquele candidato (visibilidade on-top mesmo fechada).

## 10.2 Fluxo: Atividade Colaborativa

1. Criador cria atividade (lider).
2. Envia convites por e-mail.
3. Convidados aceitam (ou declinam).
4. Participantes adicionam observações/links.
5. Gestor concede medalha a participante (opcional).
6. Sistema registra e notifica.

---

# 11. Segurança, Criptografia e Conformidade

> **Resumo:** estabelece diretrizes de proteção, mecanismos criptográficos e aderência normativa para manter o NeedUK confiável.

## 11.1 Diretrizes Gerais

* HTTPS em todas as comunicações.
* CSP (Content Security Policy) aplicado no frontend.
* Cookies: HttpOnly, Secure, SameSite=Strict para sessões.
* Política de CORS restrita por ambiente.

## 11.2 Armazenamento de Segredos

* Usar vault (HashiCorp Vault) ou secret manager do provedor (Vercel Secrets, Supabase Secrets).
* Nunca commitar arquivos com segredos.

## 11.3 Autenticação e Autorização

* Better Auth para sessões com refresh token (server-side).
* RBAC centralizado: middleware `requireRole(['RECRUTADOR'])`.

## 11.4 Proteção contra Ataques

* **SQL Injection:** Prisma evita queries concatenadas; validar inputs.
* **XSS:** Escape nas renderizações; sanitização de HTML (DOMPurify) ao permitir links/HTML.
* **CSRF:** Tokens anti-CSRF para formulários POST/PUT/DELETE.
* **Brute Force:** Rate limiting por IP + progressive delay; lockout após N tentativas.
* **Upload Malware:** escanear arquivos com serviço de antivírus (opcional) antes de disponibilizar.

!!! danger "Pontos de atenção"
    Incidentes de segurança devem ser reportados imediatamente ao canal `#security` e registrados em até 24h no inventário de riscos (Seção 17).

## 11.5 Criptografia

* Senhas: bcrypt (cost >= 12) ou argon2.
* Dados sensíveis: criptografar campos sensíveis se aplicável (colchões legais).
* TLS 1.2+ obrigatório.

## 11.6 Compliance

* Proteção de PII (dados pessoais): políticas de retenção (ex.: purge após X anos se solicitado).
* Preparação para LGPD (Brasil) — processos de consentimento e mecanismos de deleção.

---

# 12. Qualidade, Testes e Métricas

> **Resumo:** define estratégia de validação contínua, indicadores-chave e etapas de pipeline para garantir excelência operacional.

## 12.1 Estratégia de Testes

* **Unitários:** Jest/TS-Jest para lógica de negócio (meta 80% cobertura para módulos críticos).
* **Integração:** testes com banco em memória (pgmemory / testcontainers) para endpoints.
* **E2E:** Playwright para fluxos principais (login, candidatura, decisão).
* **Performance:** k6 para stress tests e metas de latência.
* **Segurança:** SCA (dependabot), SAST (SonarQube), pentest anual.

## 12.2 Métricas Principais (KPIs)

* Tempo médio de resposta (API)
* Erros 5xx por hora
* Taxa de conversão de candidaturas (aplicações / visualizações)
* Tempo médio até decisão de aplicação
* Uptime (%)

## 12.3 Pipelines de Teste

* PRs disparam pipelines: lint -> unit -> integration -> build -> deploy staging (se tudo OK).
* Merges na `main` disparam deploy para produção via CD.

---

# 13. Infraestrutura, Deploy e CI/CD

> **Resumo:** orienta configuração de ambientes, variáveis sensíveis e pipelines de entrega contínua.

## 13.1 Infraestrutura Recomendada

* **Hosting (Frontend/Server):** Vercel (Next.js) ou similar
* **DB / Realtime / Storage:** Supabase (Postgres, Realtime, Storage)
* **Cache / Jobs:** Redis (se necessário)
* **Queue / Background Jobs:** BullMQ / Redis para tasks (email, cleanup)
* **Secrets:** Vercel Secrets / HashiCorp Vault

## 13.2 Variáveis de Ambiente (essenciais)

| Variável          | Descrição                               | Ambientes |
| ----------------- | --------------------------------------- | --------- |
| `DATABASE_URL`    | Conexão PostgreSQL (Supabase)           | Todos     |
| `NEXTAUTH_SECRET` | Segredo Better Auth / NextAuth          | Todos     |
| `JWT_SECRET`      | Assinatura de tokens externos           | Todos     |
| `SUPABASE_URL`    | Endpoint da instância Supabase          | Todos     |
| `SUPABASE_KEY`    | Chave de serviço Supabase               | Server    |
| `REDIS_URL`       | Cache Redis / BullMQ (opcional)         | Server    |
| `SENTRY_DSN`      | Observabilidade (Sentry)                | Prod/Stg  |

!!! note "Rotação de segredos"
    Padronize rotação trimestral e registre as mudanças no runbook de segurança (Anexo 20.2).

## 13.3 CI/CD (exemplo GitHub Actions)

* **Workflows:** `ci.yml` (tests & lint), `staging.yml` (deploy staging), `production.yml` (deploy on merge to main with approvals).
* **Manual approvers:** PR to `main` requires 2 reviewers + passing CI.

## 13.4 Deploy Steps (produção)

1. Merge PR via Pull Request (2 approvals).
2. CI executa testes -> build.
3. CD realiza deploy para Vercel (instância de produção).
4. Migrations: `npx prisma migrate deploy` no momento do deploy.
5. Warm-up cache e health-checks.

---

# 14. Operações e Observabilidade

> **Resumo:** apresenta mecanismos de logging, tracing, monitoramento e runbooks para resposta a incidentes.

## 14.1 Logs

* Centralizar logs estruturados (JSON) via Winston/Pino enviados para um aggregator (Logflare / Datadog).
* Log levels: `error`, `warn`, `info`, `debug` (em produção manter `info` / `warn`).

## 14.2 Tracing

* Instrumentação com OpenTelemetry; exportadores para Jaeger / Datadog APM.

## 14.3 Monitoramento / Alertas

* Métricas via Prometheus / Grafana.
* Alertas: erro 5xx > threshold, latência média > threshold, DB connections nearing limit.

## 14.4 Runbooks (incidentes comuns)

* **DB connection pool exhausted:** inspecionar conexões, reiniciar workers, escalar DB.
* **High error rate 5xx:** checar deploys recentes, rolls back se necessário.
* **Email delivery failure:** checar provider (Sendgrid) e filas.

!!! info "Canal de resposta"
    Qualquer execução de runbook deve ser registrada no ticket correspondente (Jira) e comunicada no canal `#ops` com status a cada 30 minutos até estabilização.

---

# 15. Backup, Recovery e Plano de Continuidade

> **Resumo:** descreve políticas de backup, objetivos de recuperação e exercícios periódicos de resiliência.

## 15.1 Backup Policy

* **Full DB backup:** diário (snapshot) retido 30 dias.
* **Incremental WAL:** retido 7 dias.
* **Storage backups (uploads):** diário incremental, retido 30 dias.

## 15.2 Recovery Point Objective (RPO)

* RPO: 1 hora (configurável) — dependerá do WAL.

## 15.3 Recovery Time Objective (RTO)

* RTO: 4 horas para restore DB em instância alternativa (meta).

## 15.4 Testes de Disaster Recovery

* Exercícios semestrais para restore completo do DB e testes de failover.

---

# 16. Gestão de Projeto e Governança

> **Resumo:** define estrutura organizacional, rituais ágeis e responsabilidades para sustentação do roadmap.

## 16.1 Estrutura de Equipe

* **Product Owner (PO):** responsável pela priorização do backlog.
* **Scrum Master (SM):** remove impedimentos; facilita cerimônias.
* **Dev Líder:** Roger — arquitetura & code review.
* **Devs:** implementação de features.
* **QA:** scripts de testes automatizados & manuais.
* **Documentação:** Luan, Nicolly, Fernanda.

## 16.2 Cerimônias

* **Sprint Planning:** quinzenal, definição de compromissos.
* **Daily Standup:** 15 minutos (opcional)
* **Sprint Review & Demo:** ao final da sprint.
* **Retrospective:** melhoria contínua.

## 16.3 Artefatos

* **Product Backlog** (priorizado pelo PO)
* **Sprint Backlog** (itens comprometidos)
* **Definition of Done (DoD)**: código com testes, PR aprovado, documentação atualizada, build passando.

## 16.4 RACI 

| Activity               | Responsible |  Accountable |           Consulted | Informed |
| ---------------------- | ----------: | -----------: | ------------------: | -------: |
| Roadmap prioritization |          PO | Stakeholders |            Dev Lead |     Team |
| Release to prod        |      DevOps |           PO |            Dev Lead |     Team |
| Security audit         |    Dev Lead |           PO | Security Consultant |     Team |

---

# 17. Riscos, Mitigações e Registro

> **Resumo:** cataloga riscos prioritários, impactos e medidas preventivas associadas.

## 17.1 Registro de Riscos (exemplos)

| Risco                               | Impacto | Probabilidade | Mitigação principal                                         |
| ----------------------------------- | ------- | ------------- | ----------------------------------------------------------- |
| Exposição de dados sensíveis        | Alto    | Média         | SAST + pentest, hardening, criptografia, gestão de segredos |
| Baixa adoção do produto             | Médio   | Média         | Pesquisa UX, parcerias com universidades, onboarding guiado |
| Gargalos de performance no banco    | Alto    | Média         | Índices, cache, tuning de queries, read replicas            |
| Perda de dados durante deploy       | Alto    | Baixa         | Migrations controladas, backups, plano de rollback          |

---

# 18. Roadmap e Evolução

> **Resumo:** apresenta marcos entregues e incrementos planejados, orientando priorização e comunicação.

## Versão 1.0 (Entregue)

* Autenticação, perfis, vagas, candidaturas, atividades, notificações, dashboard.

## Versão 1.1 (Próximo)

* **Currículo:** CRUD de CVs, upload, export PDF, compartilhamento.

## Versão 1.2

* Métricas avançadas no dashboard, analytics para recrutadores.

## Versão 1.3

* Matching inteligente (recomendação de vagas).

## Versão 2.0

* Aplicativo móvel nativo (React Native) + chat interno.

!!! tip "Gestão de roadmap"
    Revise prioridades a cada trimestre e alinhe releases com a agenda acadêmica para maximizar adoção de alunos e parceiros.

---

# 19. Padrões de Código e Processos de Contribuição

> **Resumo:** estabelece regras de versionamento, estilo e revisão para contribuições seguras.

## 19.1 Branch Strategy

* `main` — produção (releases)
* `develop` — integração contínua (opcional)
* `feature/*`, `hotfix/*`, `bugfix/*` — branches temporárias

## 19.2 Pull Request (PR) Requirements

* PR title: `feat|fix(scope): description`
* Checklist: lint passing, tests passing, changelog entry, docs atualizadas, 2 approvals.

!!! important "Gates obrigatórios antes do merge"
    1. Pipeline `ci.yml` concluído com sucesso.  
    2. Revisão dupla concluída no GitHub.  
    3. Issue vinculada no Jira com status `Ready for Release`.

## 19.3 Code Style

* TypeScript + ESLint + Prettier — regras definidas em `/.eslintrc` e `/.prettierrc`.
* Commit messages no padrão Conventional Commits.

## 19.4 Review Checklist

* Segurança: validação de inputs, sanitização.
* Performance: avaliar queries, loops.
* Testes: unidade e integração.
* Documentação: atualizar docs relevantes.

## 19.5 Contribuição (CONTRIBUTING.md - resumo)

* Fork → feature branch → PR com descrição detalhada → review → merge com squash.

---

# 20. Anexos (Modelos, Runbooks e Checklists)

> **Resumo:** fornece artefatos complementares para execução operacional e governança.

## 20.1 Modelo de Pull Request (template)

```
## Descrição
- O que foi feito?
- Por que foi feito?

## Checklist
- [ ] Testes unitários adicionados
- [ ] Integração testada em staging
- [ ] Documentação atualizada
- [ ] Aprovação de 2 revisores
```

## 20.2 Runbook: Restore DB Rápido (exemplo)

1. Identificar snapshot mais recente.
2. Provisionar instância emergencial.
3. Restaurar snapshot.
4. Trocar endpoints / update DNS (se necessário).
5. Notificar stakeholders.

## 20.3 Checklist de Release

* [ ] Backups validados
* [ ] Migrations revisadas
* [ ] Smoke tests OK
* [ ] Rollback plan definido
* [ ] Stakeholders informados

---

!!! note "Uso dos anexos"
    Os anexos funcionam como scripts prontos para execução operacional. Ajuste-os conforme o contexto da release e armazene evidências em repositório controlado.

# Conclusão

Este material consolida as decisões técnicas e operacionais que sustentam o **NeedUK**, servindo como referência autorizada para desenvolvimento contínuo, auditorias, governança e expansão do produto. Atualizações devem seguir o fluxo de revisão descrito em Padrões de Código para preservar consistência e rastreabilidade.

---


