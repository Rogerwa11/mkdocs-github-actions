# Arquitetura e Dados

## 6. Arquitetura Técnica

### 6.1 Diagrama Lógico

```mermaid
graph TD
    Client[Browser / Client<br>React + Tailwind] 
    Next[Next.js App<br>Server + Client]
    DB[(Supabase / DB<br>PostgreSQL)]
    Redis[(Redis Cache)]
    Storage[Object Storage]
    CDN((CDN Vercel))

    Client <-->|WebSocket| Next
    Next <-->|REST| DB
    Next <--> Redis
    Next <--> Storage
    Client -.-> CDN
