# Introdução ao NeedUK

!!! info "Metadados do Documento"
    **Versão:** 1.0.0  
    **Data:** 03/12/2025  
    **Classificação:** Confidencial — Uso Interno / Acadêmico  
    **Autores:** Roger (Dev Líder), Luan, Nicolly, Fernanda (Analistas)

## 1. Introdução

### 1.1 Objetivo do Documento
Este documento tem por finalidade especificar de maneira formal, completa e técnica o sistema **NeedUK**. Descreve requisitos, arquitetura, design de dados, contratos de API, políticas de segurança, estratégias de teste, governança, operação e roadmap.

### 1.2 Alcance
Cobertura técnica e gerencial do produto NeedUK em sua versão atual.
**Inclui:** Autenticação, perfis, atividades colaborativas, vagas, notificações, dashboard.
**Não inclui:** Código-fonte exato ou credenciais privadas.

### 1.3 Padrões e Referências
* **Requisitos:** IEEE 830 / ISO/IEC 29110 (referência)
* **Gestão:** Scrum híbrido (entregas quinzenais)
* **Segurança:** OWASP Top 10

---

## 2. Visão Geral do Sistema

### 2.1 Propósito
Intermediar conexões entre estudantes, recrutadores e gestores universitários, suportando ciclos completos de autenticação, gerenciamento de perfis, atividades colaborativas e gestão de vagas.

### 2.2 Principais Funcionalidades
* :white_check_mark: Autenticação unificada (Better Auth)
* :white_check_mark: Perfis diferenciados (Aluno, Recrutador, Gestor)
* :white_check_mark: Atividades colaborativas e Vagas
* :white_check_mark: Notificações em tempo real

---

## 3. Escopo e Limitações

### 3.1 Escopo (Incluído)
* Back-end (API REST + Zod)
* Front-end (Next.js App Router)
* Persistência (PostgreSQL + Prisma)
* Realtime (WebSocket/Supabase)

### 3.2 Limitações (Não incluído na v1.0)
* :x: Módulo de Currículos (Planejado)
* :x: Integração nativa redes sociais
* :x: Chat P2P
* :x: Matching via ML
