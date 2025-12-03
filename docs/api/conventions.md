# Convenções da API

## Visão Geral

As rotas da API NeedUK são organizadas sob `src/app/api/...`. O sistema utiliza uma combinação de API Routes (Next.js) e Server Actions para operações seguras e eficientes.

!!! info "Tecnologias"
    - **Framework**: Next.js 14+ API Routes
    - **Validação**: Zod
    - **Autenticação**: Better Auth
    - **Formato**: REST JSON

## Princípios Gerais

### 1. Formato de Resposta

Todas as respostas da API seguem um formato padronizado:

#### Sucesso
```json
{
  "success": true,
  "data": {
    // ... dados da resposta
  }
}
```

#### Erro
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Descrição do erro",
    "details": {} // opcional
  }
}
```

### 2. Códigos de Status HTTP

| Código | Significado | Uso |
|--------|-------------|-----|
| 200 | OK | Requisição bem-sucedida |
| 201 | Created | Recurso criado com sucesso |
| 204 | No Content | Operação bem-sucedida sem retorno |
| 400 | Bad Request | Dados inválidos |
| 401 | Unauthorized | Não autenticado |
| 403 | Forbidden | Sem permissão |
| 404 | Not Found | Recurso não encontrado |
| 409 | Conflict | Conflito (ex: duplicação) |
| 429 | Too Many Requests | Rate limit excedido |
| 500 | Internal Server Error | Erro do servidor |

## Autenticação

### Métodos Suportados

#### 1. Cookie HttpOnly (Padrão)
```http
Cookie: session=<session-token>
```

#### 2. Bearer Token (APIs externas)
```http
Authorization: Bearer <jwt-token>
```

### Exemplo de Autenticação

=== "cURL"
    ```bash
    curl -X POST https://api.needuk.com/api/auth/login \
      -H "Content-Type: application/json" \
      -d '{"email":"user@example.com","password":"senha"}'
    ```

=== "JavaScript"
    ```javascript
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        email: 'user@example.com',
        password: 'senha'
      })
    });
    
    const data = await response.json();
    ```

=== "Python"
    ```python
    import requests
    
    response = requests.post(
        'https://api.needuk.com/api/auth/login',
        json={
            'email': 'user@example.com',
            'password': 'senha'
        }
    )
    
    data = response.json()
    ```

## Paginação

Todos os endpoints que retornam listas suportam paginação:

### Parâmetros de Query

| Parâmetro | Tipo | Padrão | Descrição |
|-----------|------|--------|-----------|
| `page` | number | 1 | Página atual |
| `limit` | number | 20 | Itens por página |

### Exemplo de Resposta Paginada

```json
{
  "success": true,
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "totalPages": 8,
      "hasNext": true,
      "hasPrev": false
    }
  }
}
```

### Exemplo de Uso

```http
GET /api/vacancies?page=2&limit=10
```

## Filtros

### Parâmetros Comuns

| Parâmetro | Descrição | Exemplo |
|-----------|-----------|---------|
| `q` | Busca por texto | `?q=desenvolvedor` |
| `course` | Filtro por curso | `?course=Engenharia` |
| `status` | Filtro por status | `?status=ABERTA` |
| `sort` | Ordenação | `?sort=createdAt:desc` |

### Sintaxe de Ordenação

```
?sort=<campo>:<direção>
```

Exemplos:
- `?sort=title:asc` - Título crescente
- `?sort=createdAt:desc` - Data decrescente
- `?sort=priority:desc,createdAt:desc` - Múltiplos campos

## Validação de Entrada

Todas as entradas são validadas usando schemas Zod:

### Exemplo de Validação

```typescript
import { z } from 'zod';

const CreateVacancySchema = z.object({
  title: z.string().min(5).max(150),
  description: z.string().min(10),
  requirements: z.array(z.string()).optional(),
  targetCourse: z.string().optional().nullable(),
  status: z.enum(['RASCUNHO', 'ABERTA', 'FECHADA']).default('RASCUNHO')
});
```

### Resposta de Erro de Validação

```json
{
  "success": false,
  "error": {
    "code": "INVALID_INPUT",
    "message": "Dados de entrada inválidos",
    "details": {
      "title": ["Título deve ter pelo menos 5 caracteres"],
      "description": ["Descrição é obrigatória"]
    }
  }
}
```

## Rate Limiting

### Limites Padrão

| Endpoint | Limite | Janela |
|----------|--------|--------|
| `/api/auth/login` | 5 requisições | 15 minutos |
| `/api/*` (geral) | 100 requisições | 1 minuto |
| `/api/*/public` | 30 requisições | 1 minuto |

### Headers de Rate Limit

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1638360000
```

### Resposta ao Exceder Limite

```json
{
  "success": false,
  "error": {
    "code": "RATE_LIMIT",
    "message": "Muitas requisições. Tente novamente em 30 segundos.",
    "retryAfter": 30
  }
}
```

## Versionamento

### Estratégia Atual

A API atualmente não possui versionamento explícito na URL. Mudanças breaking serão introduzidas através de:

1. Novos endpoints paralelos
2. Período de deprecação de 6 meses
3. Comunicação prévia aos consumidores

### Futuro

```http
GET /api/v2/vacancies
```

## CORS

### Configuração

```javascript
// next.config.js
module.exports = {
  async headers() {
    return [
      {
        source: '/api/:path*',
        headers: [
          { key: 'Access-Control-Allow-Origin', value: process.env.ALLOWED_ORIGINS },
          { key: 'Access-Control-Allow-Methods', value: 'GET,POST,PUT,DELETE,PATCH' },
          { key: 'Access-Control-Allow-Headers', value: 'Content-Type, Authorization' }
        ]
      }
    ];
  }
};
```

## Compressão

Todas as respostas suportam compressão:

```http
Accept-Encoding: gzip, deflate, br
```

## Idempotência

### Endpoints Idempotentes

| Método | Idempotente |
|--------|-------------|
| GET | ✅ Sim |
| PUT | ✅ Sim |
| DELETE | ✅ Sim |
| POST | ❌ Não* |
| PATCH | ⚠️ Depende |

*Alguns POSTs podem usar `Idempotency-Key` header

### Exemplo de Uso

```http
POST /api/applications
Idempotency-Key: unique-key-123
```

## Webhooks (Planejado)

!!! warning "Em Desenvolvimento"
    Funcionalidade de webhooks está planejada para v1.2

Estrutura planejada:
```json
{
  "event": "application.created",
  "data": { ... },
  "timestamp": "2025-12-03T10:00:00Z",
  "signature": "sha256=..."
}
```

## Melhores Práticas

### 1. Sempre Tratar Erros

```javascript
try {
  const response = await fetch('/api/endpoint');
  if (!response.ok) {
    throw new Error('Request failed');
  }
  const data = await response.json();
} catch (error) {
  console.error('API Error:', error);
}
```

### 2. Usar Timeouts

```javascript
const controller = new AbortController();
const timeout = setTimeout(() => controller.abort(), 5000);

fetch('/api/endpoint', { signal: controller.signal })
  .finally(() => clearTimeout(timeout));
```

### 3. Cache Apropriado

```javascript
// Cache de leitura (GET)
const response = await fetch('/api/profile/me', {
  cache: 'force-cache',
  next: { revalidate: 3600 } // 1 hora
});
```

### 4. Retry com Backoff

```javascript
async function fetchWithRetry(url, options, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fetch(url, options);
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise(resolve => setTimeout(resolve, 2 ** i * 1000));
    }
  }
}
```

## Próximos Passos

- [Autenticação](auth.md) - Endpoints de autenticação
- [Perfis](profiles.md) - Gestão de perfis
- [Vagas](vacancies.md) - API de vagas
- [Códigos de Erro](errors.md) - Lista completa de erros
