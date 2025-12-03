# NeedUK - Documentação Técnica Integral

!!! info "Informações do Documento"
    **Versão:** 1.0.0  
    **Data:** 2025-12-03  
    **Autores:** Roger (Dev Líder), Luan (Analista de Documentação / Dev Auxiliar), Nicolly (Analista de Documentação), Fernanda (Analista de Documentação)  
    **Classificação:** Confidencial — Uso Interno / Acadêmico

## Bem-vindo

Este é o portal de documentação técnica integral do sistema **NeedUK**, uma plataforma que intermedia conexões entre estudantes, recrutadores e gestores universitários.

## Navegação Rápida

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } __Começando__

    ---

    Visão geral do sistema, arquitetura e primeiros passos

    [:octicons-arrow-right-24: Visão Geral](overview/introduction.md)

-   :material-api:{ .lg .middle } __API Reference__

    ---

    Documentação completa de endpoints, contratos e exemplos

    [:octicons-arrow-right-24: API](api/conventions.md)

-   :material-database:{ .lg .middle } __Modelagem de Dados__

    ---

    ERD, esquemas e estrutura do banco de dados

    [:octicons-arrow-right-24: Dados](data/erd.md)

-   :material-shield-check:{ .lg .middle } __Segurança__

    ---

    Diretrizes de segurança, autenticação e compliance

    [:octicons-arrow-right-24: Segurança](security/guidelines.md)

-   :material-cog:{ .lg .middle } __Operações__

    ---

    Monitoramento, logs e procedimentos operacionais

    [:octicons-arrow-right-24: Operações](operations/observability.md)

-   :material-book-open-variant:{ .lg .middle } __Contribuindo__

    ---

    Padrões de código e processo de contribuição

    [:octicons-arrow-right-24: Contribuir](contributing/code-standards.md)

</div>

## Sobre o Sistema

O **NeedUK** é uma plataforma completa que oferece:

- ✅ Autenticação unificada e sessões seguras
- 👥 Perfis diferenciados (Aluno, Recrutador, Gestor)
- 🤝 Atividades colaborativas com gestão de participantes
- 💼 Sistema completo de vagas e candidaturas
- 🔔 Notificações em tempo real
- 🏆 Sistema de medalhas e reconhecimento
- 📊 Dashboard personalizado por perfil

## Principais Funcionalidades

### Para Alunos
- Candidatura a vagas com carta de apresentação
- Participação em atividades colaborativas
- Gestão de perfil acadêmico
- Sistema de notificações em tempo real

### Para Recrutadores
- Criação e gestão de vagas
- Avaliação de candidaturas
- Métricas e dashboards
- Sistema de decisões e feedback

### Para Gestores
- Todas as funcionalidades de recrutador
- Concessão de medalhas
- Gestão ampliada de atividades
- Administração institucional

## Stack Tecnológica

| Camada | Tecnologia |
|--------|-----------|
| Frontend | Next.js 14+ (App Router), React, Tailwind CSS |
| Backend | Next.js API Routes, Server Components |
| Banco de Dados | PostgreSQL (Supabase) |
| ORM | Prisma |
| Autenticação | Better Auth |
| Validação | Zod |
| Realtime | Supabase Realtime / WebSocket |
| Storage | Supabase Storage |
| Deploy | Vercel |
| CI/CD | GitHub Actions |

## Suporte e Contato

Para dúvidas ou sugestões sobre esta documentação:

- 📧 Email: dev@needuk.com
- 💬 Slack: #needuk-dev
- 🐛 Issues: [GitHub Issues](https://github.com/needuk/issues)

## Atualizações Recentes

!!! success "v1.0.0 - 2025-12-03"
    - ✨ Documentação inicial completa
    - 📝 Todos os módulos principais documentados
    - 🔧 Configuração MkDocs estabelecida

---

<small>Esta documentação é mantida pela equipe de desenvolvimento do NeedUK e é atualizada continuamente.</small>
