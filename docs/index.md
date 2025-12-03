

# Documentação Técnica Integral do Sistema NeedUK

**Versão:** 1.0.0
**Data:** 2025-12-03
**Autores:** Roger (Dev Líder), Luan (Analista de Documentação / Dev Auxiliar), Nicolly (Analista de Documentação), Fernanda (Analista de Documentação)
**Classificação:** Confidencial — Uso Interno / Acadêmico

---

## Sumário

1. [Introdução](#1-introdução)
2. [Visão Geral do Sistema](#2-visão-geral-do-sistema)
3. [Escopo e Limitações](#3-escopo-e-limitações)
4. [Público-Alvo e Perfis de Usuário](#4-público-alvo-e-perfis-de-usuário)
5. [Requisitos Detalhados](#5-requisitos-detalhados)

   * Funcionais
   * Não-funcionais
6. [Arquitetura Técnica e Lógica](#6-arquitetura-técnica-e-lógica)

   * Diagrama lógico (ASCII)
   * Componentes e responsabilidades
7. [Modelagem de Dados (ERD e Esquema)](#7-modelagem-de-dados-erd-e-esquema)

   * Tabelas detalhadas com campos, tipos e índices
   * Prisma schema (exemplo)
8. [API — Endpoints, Contratos e Exemplos](#8-api--endpoints-contratos-e-exemplos)

   * Autenticação
   * Perfis
   * Atividades
   * Vagas
   * Candidaturas
   * Notificações
   * Medalhas
   * Erros e códigos comuns
9. [Validação e Schemas (Zod)](#9-validação-e-schemas-zod)
10. [Fluxos Críticos (sequências passo a passo)](#10-fluxos-críticos-sequências-passo-a-passo)
11. [Segurança, Criptografia e Conformidade](#11-segurança-criptografia-e-conformidade)
12. [Qualidade, Testes e Métricas](#12-qualidade-testes-e-métricas)
13. [Infraestrutura, Deploy e CI/CD](#13-infraestrutura-deploy-e-cicd)
14. [Operações e Observabilidade](#14-operações-e-observabilidade)
15. [Backup, Recovery e Plano de Continuidade](#15-backup-recovery-e-plano-de-continuidade)
16. [Gestão de Projeto e Governança](#16-gestão-de-projeto-e-governança)
17. [Riscos, Mitigações e Registro](#17-riscos-mitigacões-e-registro)
18. [Roadmap e Evolução](#18-roadmap-e-evolução)
19. [Padrões de Código e Processos de Contribuição](#19-padrões-de-código-e-processos-de-contribuição)
20. [Anexos (Modelos de Documentos, Runbooks, Checklist)](#20-anexos-modelos-de-documentos-runbooks-checklist)

---

# 1. Introdução

### 1.1 Objetivo do Documento

Este documento tem por finalidade especificar de maneira formal, completa e técnica o sistema **NeedUK**. Descreve requisitos, arquitetura, design de dados, contratos de API, políticas de segurança, estratégias de teste, governança, operação e roadmap, de forma que terceiros (desenvolvedores, auditores, avaliadores ou futuros mantenedores) obtenham entendimento suficiente para manutenção, evolução e auditoria do sistema.

### 1.2 Alcance

Cobertura técnica e gerencial do produto NeedUK em sua versão atual (autenticação, perfis, atividades colaborativas, vagas e candidaturas, notificações em tempo real, dashboard responsivo). Inclui também planejamento pormenorizado para o módulo de Currículos (planejado). Não inclui: código-fonte exato (embora padrões e exemplos sejam fornecidos), nem configurações privadas de provedores (credenciais/segredos).

### 1.3 Padrões e Referências

* Padrões de requisitos: IEEE 830 / ISO/IEC 29110 (adotado como referência)
* Metodologia de gestão: Scrum híbrido com entregas quinzenais
* Segurança: OWASP Top 10 como baseline de mitigação

---

# 2. Visão Geral do Sistema

### 2.1 Propósito

Intermediar conexões entre estudantes, recrutadores e gestores universitários, suportando ciclos completos de autenticação, gerenciamento de perfis, atividades colaborativas, gestão de vagas, candidaturas e notificações em tempo real, para aumento de empregabilidade e colaboração acadêmica.

### 2.2 Principais Funcionalidades

* Autenticação unificada e sessões seguras (Better Auth)
* Perfis diferenciados (Aluno, Recrutador, Gestor) com edição completa
* Atividades colaborativas: criação, convites por e-mail, transferência de liderança, observações, links, medalhas
* Vagas: criação, rascunhos, filtros por curso, ordenação customizada, destaque de vagas aceitas
* Candidaturas: envio com carta, decisões dos recrutadores, histórico e notificações
* Notificações em tempo real e limpeza automática
* Dashboard responsivo por perfil
* Currículo: página planejada (especificada)

---

# 3. Escopo e Limitações

### 3.1 Escopo (o que está incluído)

* Back-end (API) com endpoints REST e validação via Zod
* Front-end em Next.js (App Router + Server Components)
* Persistência em PostgreSQL via Prisma (Supabase)
* Sistema de notificações em tempo real (WebSocket / Realtime do Supabase ou WebSocket custom)
* Autenticação com Better Auth
* Validações robustas e testes unitários/integrados

### 3.2 Limitações (o que NÃO está incluído nesta versão)

* Módulo de Currículos (em planejamento; descrição completa no roadmap)
* Integração nativa com redes sociais (LinkedIn, Google OAuth)
* Chat P2P (planejado para roadmap posterior)
* Sistema de matching baseado em ML (futuro)

---

# 4. Público-Alvo e Perfis de Usuário

### 4.1 Perfis

* **Aluno**: candidato às vagas, participa de atividades, inclui dados acadêmicos.
* **Recrutador**: publica vagas, gerencia candidaturas, toma decisões.
* **Gestor**: membro institucional com privilégios extra (conceder medalhas, administrar atividades).
* **Admin / DevOps**: manutenção de sistema, observabilidade e deploy.

### 4.2 Permissões Principais (RBAC)

* **Aluno:** visualizar vagas, candidatar-se, gerenciar perfil, participar de atividades.
* **Recrutador:** CRUD de vagas, avaliar candidaturas, enviar mensagens de decisão, visualizar métricas de suas vagas.
* **Gestor:** tudo de Recrutador + concessão de medalhas, gerenciamento ampliado de atividades.
* **Admin:** acesso administrativo (logs, limpeza, gerenciamento de usuários).

---

# 5. Requisitos Detalhados

## 5.1 Requisitos Funcionais (RF)

### RF01 — Autenticação e Sessões

* **Descrição:** O sistema deve permitir registro, login, logout e recuperação de senha.
* **Pré-condição:** Acesso ao endpoint de auth.
* **Critérios de Aceitação:**

  * Registro cria usuário e dispara verificação por e-mail.
  * Login retorna sessão válida (cookie HttpOnly + token).
  * Rotas protegidas retornam 401 para usuários não autenticados.
  * Implementação via Better Auth para sessions + refresh tokens.
* **Notas de Implementação:** Tokens com expiração curta (ex.: 15 min) + refresh seguro; logout deve invalidar o refresh no servidor.

### RF02 — Gestão de Perfis

* **Descrição:** Usuário deve visualizar e editar perfil (dados pessoais, formação, foto, contadores de medalhas).
* **Campos Mínimos:** nome, email, curso, semestre, bio, foto, links.
* **Critérios:** Alterações refletidas imediatamente (eventual consistency aceita para cache).

### RF03 — Atividades Colaborativas

* **Descrição:** Criar atividades com título, descrição, data/hora, visibilidade, liderança, convites por e-mail, observações, links.
* **Fluxos:** criar → convidar → aceitar → participar → observar → conceder medalha.
* **Regras:** Apenas líder pode transferir liderança; gestor pode conceder medalha.

### RF04 — Vagas

* **Descrição:** CRUD de vagas; rascunhos; filtros avançados (curso, palavra-chave, status); ordenação personalizada.
* **Regras Especiais:** Vagas aceitas por um candidato permanecem visíveis para o mesmo (mesmo se fechadas) e aparecem no topo.

### RF05 — Candidaturas

* **Descrição:** Envio com carta de apresentação e links; histórico de decisões; notificações; possibilidade de retirar candidatura.
* **Estados:** PENDENTE → ACEITO / RECUSADO → (opcional) RETIRADO.

### RF06 — Notificações em Tempo Real

* **Descrição:** Notificações de convites, decisões, medalhas e atividades.
* **Comportamento:** entrega instantânea via websocket; fallback para polling caso não haja conexão.
* **Limpeza:** Notificações lidas e com data superior a X dias (configurável) são elegíveis para limpeza automática.

### RF07 — Dashboard

* **Descrição:** Painel personalizado por perfil com resumo (vagas recentes, candidaturas, atividades, notificações).
* **Métricas:** contador de candidaturas, taxa de aceitação, atividades ativas.

### RF08 — Currículo (planejado)

* **Descrição:** CRUD de currículos estruturados; upload de arquivos; compartilhamento via link; export para PDF.
* **Status:** Em especificação detalhada (secção Roadmap).

## 5.2 Requisitos Não-Funcionais (RNF)

### RNF01 — Desempenho

* **Meta:** 95% das requisições devem responder < 300ms em condições normais (pico de 500 usuários simultâneos).

### RNF02 — Escalabilidade

* **Meta:** Sistema preparado para escalabilidade horizontal; conexões de DB com pool adequado; cache (Redis) para endpoints de leitura intensiva.

### RNF03 — Segurança

* **Requisitos:** HTTPS obrigatório, proteção CSRF, sanitização de inputs, hashing de senhas (bcrypt/argon2), proteção contra brute-force (rate limiting).

### RNF04 — Disponibilidade

* **SLA:** Disponibilidade alvo de 99% (monitoramento via UptimeRobot/Prometheus).

### RNF05 — Manutenibilidade

* **Requisitos:** Código em TypeScript com cobertura mínima de testes unitários de 70% para módulos críticos; PRs code-reviewed obrigatórios; linting via ESLint.

### RNF06 — Usabilidade / Acessibilidade

* Conformidade com WCAG 2.1 AA em telas principais (login, perfil, candidatura).

---

# 6. Arquitetura Técnica e Lógica

## 6.1 Visão Geral

Arquitetura **modular** com separação clara entre UI (Next.js), API (Server Components / API Routes), domínio (lib/*) e persistência (Prisma / PostgreSQL).

## 6.2 Diagrama Lógico (ASCII)

```
+-----------------------+         +------------------+         +--------------------+
|   Browser / Client    | <-----> |   Next.js App    | <-----> |    Supabase / DB   |
| (React, Tailwind UI)  | WebSocket| (Server/Client) |  REST   |  PostgreSQL + Realtime |
+-----------------------+         +------------------+         +--------------------+
        |  ^                              |  ^                        |  ^
        |  |                              |  |                        |  |
        v  |                              v  |                        v  |
+-----------------------+         +------------------+         +--------------------+
|  CDN / Assets (Vercel)|         |  Redis Cache     |         |  Object Storage    |
+-----------------------+         +------------------+         +--------------------+
```

## 6.3 Componentes e Responsabilidades

* **Frontend (Next.js)**

  * Server Components para SSR/SSG; App Router para navegação; pages para landing e auth flows.
  * Component library em `components/ui/` (botões, inputs, modais, toast, tabelas).
  * Hooks personalizados em `hooks/` (useAuth, useNotifications).

* **API / Backend**

  * API Routes em `src/app/api/` (auth, profile, activities, vacancies, notifications).
  * Business logic em `lib/` (serviços, domain).
  * Validation via Zod.

* **Banco de Dados (Supabase/Postgres + Prisma)**

  * Prisma Client gerado (`npx prisma generate`).
  * Esquema com entidades centrais (User, Vacancy, Application, Activity, Notification, Medal).

* **Realtime / Notificações**

  * Supabase Realtime ou WebSocket custom para notificações push.
  * Fallback: long-polling via SSE.

* **Cache**

  * Redis para cache de consultas frequentes e sessões (opcional).

* **Storage**

  * Supabase Storage ou S3 para uploads (profile images, CVs).

---

# 7. Modelagem de Dados (ERD e Esquema)

> Abaixo segue a **modelagem relacional** com campos, tipos sugeridos e índices. Inclui também um **exemplo de schema Prisma**.

## 7.1 Tabelas Principais (detalhadas)

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

## 7.2 Exemplo de Schema Prisma (simplificado)

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

# 8. API — Endpoints, Contratos e Exemplos

> As rotas são organizadas sob `src/app/api/...`. Preferir uso de Server Actions (Next.js) para operações seguras quando aplicável; manter REST para interoperabilidade.

## 8.1 Convenções

* Todos os endpoints retornam JSON.
* Padrão de sucesso: status 200/201 com corpo `{ success: true, data: ... }`.
* Erros: `{ success: false, error: { code: "...", message: "..." } }`.
* Autenticação: cookie HttpOnly + header `Authorization: Bearer <token>` para APIs externas.
* Paginação: query `?page=1&limit=20`.
* Filtros: `?course=Engenharia&status=ABERTA`.

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

# 10. Fluxos Críticos (sequências passo a passo)

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

## 11.5 Criptografia

* Senhas: bcrypt (cost >= 12) ou argon2.
* Dados sensíveis: criptografar campos sensíveis se aplicável (colchões legais).
* TLS 1.2+ obrigatório.

## 11.6 Compliance

* Proteção de PII (dados pessoais): políticas de retenção (ex.: purge após X anos se solicitado).
* Preparação para LGPD (Brasil) — processos de consentimento e mecanismos de deleção.

---

# 12. Qualidade, Testes e Métricas

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

## 13.1 Infraestrutura Recomendada

* **Hosting (Frontend/Server):** Vercel (Next.js) ou similar
* **DB / Realtime / Storage:** Supabase (Postgres, Realtime, Storage)
* **Cache / Jobs:** Redis (se necessário)
* **Queue / Background Jobs:** BullMQ / Redis para tasks (email, cleanup)
* **Secrets:** Vercel Secrets / HashiCorp Vault

## 13.2 Variáveis de Ambiente (essenciais)

* `DATABASE_URL`
* `NEXTAUTH_SECRET` (ou Better Auth secret)
* `JWT_SECRET`
* `SUPABASE_URL`, `SUPABASE_KEY`
* `REDIS_URL`
* `SENTRY_DSN` (observability)

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

---

# 15. Backup, Recovery e Plano de Continuidade

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

## 16.4 RACI (exemplo)

| Activity               | Responsible |  Accountable |           Consulted | Informed |
| ---------------------- | ----------: | -----------: | ------------------: | -------: |
| Roadmap prioritization |          PO | Stakeholders |            Dev Lead |     Team |
| Release to prod        |      DevOps |           PO |            Dev Lead |     Team |
| Security audit         |    Dev Lead |           PO | Security Consultant |     Team |

---

# 17. Riscos, Mitigações e Registro

## 17.1 Registro de Riscos (exemplos)

1. **Risco:** Falhas de segurança (exposição de dados).

   * **Impacto:** Alto
   * **Probabilidade:** Média
   * **Mitigação:** Revisões SAST, pentest, hardening, criptografia, política de segredos.

2. **Risco:** Baixa adoção pelos usuários.

   * **Impacto:** Médio
   * **Mitigação:** UX research, integração com universidades, campanhas de onboarding.

3. **Risco:** Gargalos de performance no DB.

   * **Impacto:** Alto
   * **Mitigação:** Índices, cache, otimização de queries, read replicas.

4. **Risco:** Perda de dados em deploy.

   * **Impacto:** Alto
   * **Mitigação:** migrations controladas, backups, rollback planejado.

---

# 18. Roadmap e Evolução

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

---

# 19. Padrões de Código e Processos de Contribuição

## 19.1 Branch Strategy

* `main` — produção (releases)
* `develop` — integração contínua (opcional)
* `feature/*`, `hotfix/*`, `bugfix/*` — branches temporárias

## 19.2 Pull Request (PR) Requirements

* PR title: `feat|fix(scope): description`
* Checklist: lint passing, tests passing, changelog entry, docs atualizadas, 2 approvals.

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

# 20. Anexos — Modelos, Runbooks e Checklists

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

# Conclusão

O presente documento foi elaborado para servir como referência única e fonte autoritativa sobre o sistema **NeedUK**, cobrindo todos os aspectos críticos de concepção, implementação, operação e evolução. Ele fornece base suficiente para desenvolvimento contínuo, auditoria de segurança, governança e entrega confiável do produto.

---


