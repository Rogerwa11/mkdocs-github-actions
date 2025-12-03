# Documento de Especificação e Arquitetura do Sistema NEEDUK


# **1. Introdução**

## **1.1. Propósito do Documento**

Este documento tem como finalidade apresentar, de forma abrangente, estruturada e tecnicamente rigorosa, toda a documentação referente ao Sistema de Gestão de Oportunidades, englobando arquitetura, requisitos, modelagem, funcionalidades, métodos de desenvolvimento, estratégias de segurança e demais artefatos essenciais ao entendimento e manutenção do sistema.

## **1.2. Escopo do Sistema**

O sistema destina-se ao gerenciamento completo de oportunidades, permitindo que alunos, professores e administradores interajam com atividades, eventos, vagas, currículos e fluxos relacionados.

Inclui:

* Cadastro e autenticação de usuários.
* Criação e gestão de oportunidades.
* Candidaturas.
* Permissões por papéis.
* Notificações.
* Dashboards.
* Gestão de currículos e perfis.

## **1.3. Público-Alvo**

Este documento é destinado a:

* Engenheiros de software
* Desenvolvedores backend e frontend
* Analistas de requisitos
* Professores e avaliadores acadêmicos
* Gestores de projeto
* Stakeholders

## **1.4. Definições e Abreviações**

| Termo | Significado                       |
| ----- | --------------------------------- |
| API   | Application Programming Interface |
| ER    | Entidade-Relacionamento           |
| UML   | Unified Modeling Language         |
| CRUD  | Create, Read, Update, Delete      |
| JWT   | JSON Web Token                    |
| RBAC  | Role-Based Access Control         |
| TAP   | Termo de Abertura do Projeto      |

---

# **2. Visão Geral do Sistema**

## **2.1. Descrição**

O sistema é uma plataforma web destinada ao gerenciamento e acompanhamento de oportunidades internas e externas, voltada ao público acadêmico. Ele provê meios estruturados para divulgação, pesquisa, registro e controle de participação em atividades, eventos e vagas.

## **2.2. Problemas Solucionados**

* Redução da perda de informações sobre oportunidades.
* Centralização de atividades acadêmicas.
* Facilitação da comunicação entre alunos e oportunidades no mercado.
* Organização de dados de forma segura e rastreável.

## **2.3. Benefícios**

* Transparência
* Governança da informação
* Facilidade no acompanhamento de trajetórias acadêmicas
* Engajamento institucional com o mercado de trabalho

---

# **3. Arquitetura do Sistema**

## **3.1. Arquitetura Geral**

Arquitetura baseada em serviços REST, em camadas:

* **Frontend**: SPA (Single Page Application)
* **Backend**: API REST modular
* **Banco de Dados**: Relacional
* **Autenticação**: JWT
* **Controle de Acesso**: RBAC
* **Monitoramento e Logs**: Middleware + persistência de auditoria

---

# **4. Tecnologias Utilizadas**

| Camada         | Tecnologia        |
| -------------- | ----------------- |
| Frontend       | React (SPA)       |
| Backend        | Node.js / Express |
| Banco de Dados | PostgreSQL        |
| Autenticação   | JWT               |
| Padronização   | ESLint / Prettier |
| Infraestrutura | Docker            |

---

# **5. Requisitos**

## **5.1. Requisitos Funcionais (RF)**

### **RF01 – Autenticação e Autorização**

* O sistema deve permitir login por email e senha.
* Deve gerar tokens JWT.
* Deve aplicar RBAC.

### **RF02 – Gestão de Oportunidades**

* CRUD completo de atividades, eventos e vagas.
* Permitir anexos.
* Permitir filtros avançados.

### **RF03 – Candidaturas**

* Usuários podem candidatar-se a oportunidades.
* Administradores podem aprovar ou recusar.

### **RF04 – Gestão de Currículos**

* Upload de currículo.
* Atualização de dados.
* Visualização pelo administrador.

### **RF05 – Notificações**

* Alertas internos.
* Histórico de notificações.

### **RF06 – Dashboard Administrativo**

* Gráficos e indicadores.
* Estatísticas de candidaturas.

---

## **5.2. Requisitos Não Funcionais (RNF)**

| Código | Descrição                                                        |
| ------ | ---------------------------------------------------------------- |
| RNF01  | O sistema deve responder em até 2 segundos em condições normais. |
| RNF02  | Deve suportar concorrência mínima de 500 usuários simultâneos.   |
| RNF03  | Deve utilizar padrões de segurança OWASP.                        |
| RNF04  | Interface responsiva.                                            |
| RNF05  | Código padronizado conforme eslint.                              |
| RNF06  | Alta disponibilidade.                                            |

---

# **6. Modelagem de Dados**

## **6.1. Entidades Principais**

* Usuário
* Oportunidade
* Candidatura
* Notificação
* Currículo
* Log de Auditoria

## **6.2. Diagrama ER**

**Usuário (1) — (N) Oportunidades**
**Usuário (1) — (N) Candidaturas**
**Oportunidade (1) — (N) Candidaturas**
**Usuário (1) — (N) Notificações**

---

# **7. Casos de Uso**

## **UC01 – Autenticação**

**Atores:** Usuário
**Fluxo Principal:**

1. Usuário informa email e senha.
2. Sistema valida credenciais.
3. Sistema retorna token JWT.

**Fluxo Alternativo:**

* Credenciais inválidas → erro 401.

---

## **UC02 – Criar Oportunidade**

**Atores:** Administrador
**Fluxo Principal:**

1. Administrador acessa módulo de criação.
2. Preenche dados.
3. Sistema salva oportunidade.

---

## **UC03 – Candidatar-se**

**Atores:** Aluno

1. Aluno seleciona oportunidade.
2. Clica em candidatar-se.
3. Sistema registra e notifica administrador.

---

# **8. Gestão do Projeto**

## **8.1. Metodologia**

Metodologia híbrida baseada em Scrum:

* Sprints quinzenais
* Backlog priorizado
* Daily opcional

---

## **8.2. Estrutura Analítica do Projeto (EAP)**

```
1. Iniciação
   1.1 TAP
   1.2 Definição do escopo
2. Planejamento
   2.1 Requisitos
   2.2 Arquitetura
3. Execução
   3.1 Backend
   3.2 Frontend
   3.3 Banco de Dados
4. Encerramento
   4.1 Entrega
   4.2 Documentação Final
```

---

# **9. Estrutura da API**

## **9.1. Autenticação**

**POST /auth/login**

* email
* senha

**Resposta:**

```json
{
  "token": "jwt..."
}
```

---

## **9.2. Oportunidades**

**POST /oportunidades**
**GET /oportunidades**
**PUT /oportunidades/:id**
**DELETE /oportunidades/:id**

---

## **9.3. Candidaturas**

**POST /candidaturas**
**GET /candidaturas?userId=**
**PATCH /candidaturas/:id/status**

---

# **10. Segurança**

* Criptografia de senhas com bcrypt.
* Tokens com expiração curta.
* Sanitização de dados.
* Logs de auditoria para ações sensíveis.

---

# **11. Estratégia de Testes**

## **11.1. Tipos de Teste**

* Testes unitários
* Testes integrados
* Testes de carga
* Testes de segurança
* Testes de usabilidade

---

# **12. Roadmap**

## **12.1. Funcionalidades Entregues**

* Login
* CRUD de Oportunidades
* Candidaturas
* Permissões

## **12.2. Em Desenvolvimento**

* Currículos
* Dashboard

## **12.3. Futuras**

* Chat interno
* Integração com Sistemas Externos

🚀 NeedUK — Conectando talentos, empresas e universidades com tecnologia.

