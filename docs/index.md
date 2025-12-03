# Documentação Técnica Integral do Sistema de Gerenciamento de Oportunidades Acadêmicas

<!-- Índice Flutuante -->
<div id="toc-container">
  <h2>Índice</h2>
  <ul>
    <li><a href="#1-introdução">1. Introdução</a></li>
    <li><a href="#2-objetivo-geral">2. Objetivo Geral</a></li>
    <li><a href="#3-objetivos-específicos">3. Objetivos Específicos</a></li>
    <li><a href="#4-justificativa">4. Justificativa</a></li>
    <li><a href="#5-escopo-do-projeto">5. Escopo do Projeto</a></li>
    <li>
      <a href="#6-funcionalidades-do-sistema">6. Funcionalidades do Sistema</a>
      <ul>
        <li><a href="#61-funcionalidades-principais">6.1. Funcionalidades Principais</a></li>
        <li><a href="#62-funcionalidades-futuras">6.2. Funcionalidades Futuras</a></li>
      </ul>
    </li>
    <li>
      <a href="#7-modelagem-uml">7. Modelagem UML</a>
      <ul>
        <li><a href="#71-diagrama-de-casos-de-uso">7.1. Diagrama de Casos de Uso</a></li>
        <li><a href="#72-diagrama-de-classes">7.2. Diagrama de Classes</a></li>
        <li><a href="#73-diagrama-de-sequência">7.3. Diagrama de Sequência</a></li>
      </ul>
    </li>
    <li>
      <a href="#8-arquitetura-do-sistema">8. Arquitetura do Sistema</a>
      <ul>
        <li><a href="#81-frontend">8.1. Front-end</a></li>
        <li><a href="#82-backend">8.2. Back-end</a></li>
        <li><a href="#83-banco-de-dados">8.3. Banco de Dados</a></li>
      </ul>
    </li>
    <li>
      <a href="#9-requisitos">9. Requisitos</a>
      <ul>
        <li><a href="#91-requisitos-funcionais">9.1. Requisitos Funcionais</a></li>
        <li><a href="#92-requisitos-não-funcionais">9.2. Requisitos Não Funcionais</a></li>
      </ul>
    </li>
    <li><a href="#10-casos-de-uso-detalhados">10. Casos de Uso Detalhados</a></li>
    <li><a href="#11-conclusão">11. Conclusão</a></li>
  </ul>
</div>

<style>
#toc-container {
  position: fixed;
  top: 60px;
  right: 30px;
  width: 260px;
  max-height: 80vh;
  overflow-y: auto;
  padding: 15px 20px;
  background: #f7f7f7;
  border: 1px solid #d0d0d0;
  border-radius: 12px;
  box-shadow: 0 4px 14px rgba(0,0,0,0.12);
  font-size: 15px;
}
#toc-container ul { list-style: none; padding-left: 5px; }
#toc-container a { text-decoration: none; color: #0366d6; }
#toc-container a:hover { text-decoration: underline; }
</style>

---

# 1. Introdução

Este documento apresenta a documentação técnica integral do **Sistema de Gerenciamento de Oportunidades Acadêmicas (SGOA)**.  
A proposta do sistema é criar uma plataforma web estruturada para o gerenciamento centralizado de oportunidades acadêmicas, permitindo que estudantes, docentes e instituições registrem, consultem e acompanhem:

- processos seletivos,  
- cursos,  
- eventos,  
- editais,  
- bolsas,  
- programas de extensão e pesquisa,  
- entre outros.

O documento abrange desde a contextualização e justificativa até a definição completa dos requisitos funcionais e não funcionais, modelagem UML, arquitetura, fluxo operacional e especificações de cada caso de uso.

---

# 2. Objetivo Geral

Desenvolver um sistema computacional moderno, seguro e escalável para **gerenciar, catalogar, divulgar e acompanhar oportunidades acadêmicas**, garantindo fácil acesso e proporcionando transparência, organização e eficiência aos usuários envolvidos.

---

# 3. Objetivos Específicos

- Estabelecer uma plataforma padronizada e centralizada para registro de oportunidades.
- Permitir que estudantes tenham acesso rápido a informações validadas.
- Disponibilizar funcionalidades de filtragem e busca avançada.
- Permitir que instituições publiquem e atualizem oportunidades.
- Implementar notificações automatizadas e personalizadas.
- Assegurar integridade, segurança e rastreabilidade das informações.
- Garantir acessibilidade e responsividade em múltiplos dispositivos.

---

# 4. Justificativa

O volume crescente de oportunidades acadêmicas divulgadas em diferentes plataformas gera dificuldades quanto à organização, verificação e acompanhamento.

Atualmente, estudantes dependem de redes sociais, e-mails, sites isolados ou recebimento informal de informações — o que ocasiona:

- perda de prazos,  
- desencontro de informações,  
- inconsistência na comunicação,  
- dificuldade de análise comparativa.

O SGOA surge como solução centralizada, tecnicamente robusta e institucionalmente confiável, modernizando a forma como oportunidades são divulgadas e consumidas pela comunidade acadêmica.

---

# 5. Escopo do Projeto

O escopo contempla:

### ✔ Incluído (In Scope)
- Cadastro de oportunidades acadêmicas.
- Gestão de usuários (administradores, instituições, estudantes).
- Sistema de autenticação e autorização.
- Pesquisa avançada por filtros.
- Notificações por e-mail.
- Acompanhamento de oportunidades salvas.
- Interface web responsiva.

### ❌ Excluído (Out of Scope)
- Aplicativo mobile nativo (Android/iOS).
- Chatbot integrado.
- Sistema de recomendação por IA.
- Integração com plataformas externas de bolsas.

---

# 6. Funcionalidades do Sistema

## 6.1. Funcionalidades Principais

- Cadastro de oportunidades detalhadas.
- Edição e exclusão de oportunidades.
- Módulo de autenticação (login/registro).
- Painel administrativo para instituições.
- Sistema de busca avançada.
- Favoritar oportunidades.
- Receber alertas de publicação.
- Histórico de visualizações.

## 6.2. Funcionalidades Futuras

- Sistema de recomendação por machine learning.
- Módulo de inscrições integrado.
- Integração com SIGAA e outras plataformas.
- Aplicativo mobile nativo.
- Painel analítico com gráficos.

---

# 7. Modelagem UML

## 7.1. Diagrama de Casos de Uso

Descreve a interação entre os atores e o sistema.

### Atores:
- **Administrador**
- **Instituição**
- **Estudante**

### Principais casos:
- Cadastrar oportunidade
- Editar oportunidade
- Excluir oportunidade
- Pesquisar oportunidade
- Favoritar oportunidade
- Visualizar detalhes

(Os diagramas podem ser adicionados posteriormente em imagem ou PlantUML.)

---

## 7.2. Diagrama de Classes

Classes principais previstas:

- **Usuario**
- **Estudante**
- **Instituicao**
- **Administrador**
- **Oportunidade**
- **Categoria**
- **Notificação**

---

## 7.3. Diagrama de Sequência

Exemplo: fluxo de cadastro de oportunidade.

1. Instituição → Sistema: solicitar cadastro  
2. Sistema → Validação: verificar dados  
3. Validação → Sistema: aprovado  
4. Sistema → Banco: inserir registro  
5. Sistema → Instituição: confirmação  

---

# 8. Arquitetura do Sistema

## 8.1. Frontend

- Framework sugerido: **React**, **Next.js** ou **Vue.js**  
- Comunicação via APIs REST  
- Interface responsiva  
- Padrões de acessibilidade WCAG  

---

## 8.2. Backend

- Linguagens recomendadas: **Node.js**, **Python (Django/FastAPI)** ou **Java Spring**  
- Autenticação JWT  
- Validação robusta de dados  
- Logs e auditoria  

---

## 8.3. Banco de Dados

- Modelo relacional: **PostgreSQL** ou **MySQL**  
- Entidades normalizadas  
- Índices para buscas avançadas  
- Tabelas sugeridas:
  - usuarios
  - oportunidades
  - categorias
  - favoritos
  - notificacoes

---

# 9. Requisitos

## 9.1. Requisitos Funcionais (RF)

**RF001 — Cadastro de usuários**  
**RF002 — Login e autenticação**  
**RF003 — Cadastro de oportunidades**  
**RF004 — Edição de oportunidades**  
**RF005 — Exclusão de oportunidades**  
**RF006 — Busca por filtros**  
**RF007 — Favoritar oportunidades**  
**RF008 — Envio de notificações**  
**RF009 — Visualização de detalhes**  
**RF010 — Gerenciamento administrativo**

---

## 9.2. Requisitos Não Funcionais (RNF)

**RNF001 — Segurança:** criptografia, HTTPS, JWT  
**RNF002 — Desempenho:** resposta < 2s  
**RNF003 — Confiabilidade:** tolerância a falhas  
**RNF004 — Usabilidade:** interface intuitiva  
**RNF005 — Portabilidade:** funcionar em qualquer navegador  
**RNF006 — Escalabilidade:** suportar aumento de usuários  
**RNF007 — Acessibilidade:** compatível com tecnologias assistivas

---

# 10. Casos de Uso Detalhados

### Caso de Uso CU01 — Cadastrar Oportunidade

**Ator:** Instituição  
**Descrição:** Permite o cadastro de novas oportunidades.  
**Fluxo Principal:**
1. Instituição acessa painel.
2. Seleciona “Cadastrar Oportunidade”.
3. Preenche formulário.
4. Sistema valida dados.
5. Sistema salva registro.
6. Sistema confirma cadastro.

### Caso de Uso CU02 — Pesquisar Oportunidades

**Ator:** Estudante  
**Descrição:** Busca de oportunidades por categorias, tipo, prazo.  

---

# 11. Conclusão

Este documento formaliza, padroniza e descreve integralmente a concepção técnica do Sistema de Gerenciamento de Oportunidades Acadêmicas, servindo como base para desenvolvimento, manutenção e expansão futura.

