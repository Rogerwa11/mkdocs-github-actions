# Perfis e Requisitos

## 4. Público-Alvo e Perfis

| Perfil | Descrição | Permissões Principais |
| :--- | :--- | :--- |
| **Aluno** | Candidato às vagas, participante de atividades. | Visualizar vagas, candidatar-se, gerenciar perfil. |
| **Recrutador** | Publica vagas, gerencia candidaturas. | CRUD de vagas, avaliar candidaturas, visualizar métricas. |
| **Gestor** | Membro institucional. | Tudo de Recrutador + conceder medalhas, gerir atividades. |
| **Admin** | Manutenção do sistema. | Acesso administrativo, logs, limpeza. |

---

## 5. Requisitos Detalhados

### 5.1 Requisitos Funcionais (RF)

!!! success "RF01 — Autenticação e Sessões"
    **Descrição:** Registro, login, logout e recuperação de senha.  
    **Critérios:** Sessão segura (HttpOnly), implementação via Better Auth.

!!! success "RF02 — Gestão de Perfis"
    **Descrição:** Edição de dados pessoais, formação, foto, bio.  
    **Critérios:** Alterações refletidas imediatamente.

!!! success "RF03 — Atividades Colaborativas"
    **Descrição:** Criar atividades, convites por e-mail, transferência de liderança.  
    **Fluxo:** Criar → Convidar → Aceitar → Participar → Observar → Medalha.

!!! success "RF04 — Vagas"
    **Descrição:** CRUD de vagas, filtros avançados, ordenação customizada.  
    **Regra:** Vagas aceitas permanecem visíveis ao candidato no topo.

!!! success "RF05 — Candidaturas"
    **Descrição:** Envio com carta, histórico, retirada de candidatura.  
    **Estados:** `PENDENTE` → `ACEITO` / `RECUSADO` → `RETIRADO`.

!!! success "RF06 — Notificações Realtime"
    **Descrição:** WebSocket para convites e decisões. Limpeza automática configurável.

### 5.2 Requisitos Não-Funcionais (RNF)

* **RNF01 - Desempenho:** 95% das reqs < 300ms.
* **RNF02 - Escalabilidade:** Horizontal, Pool de conexões, Cache Redis.
* **RNF03 - Segurança:** HTTPS, CSRF, Hash (bcrypt), Rate Limiting.
* **RNF04 - Disponibilidade:** SLA 99%.
* **RNF05 - Manutenibilidade:** TypeScript, Cobertura de testes > 70%.
