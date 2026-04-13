# SKILL · Tech Stack

> **Domínio**: Convenções de stack técnico, ferramentas preferidas e padrões de código  
> **Lido por**: HAL Planner (para recomendar stack na spec) e HAL Generator (para implementar dentro das convenções)  
> **Escopo**: Escolhas de linguagem/runtime, estrutura de projeto, padrões de qualidade de código

---

## 1. Princípios de Seleção de Stack

- **Boring tech first**: prefira tecnologias maduras e amplamente documentadas. Novidade tem custo de manutenção.
- **Mínimo de dependências**: cada dependência é uma superfície de ataque e um vetor de rot. Adicione apenas com justificativa clara.
- **Compatibilidade com o ambiente do usuário**: se o usuário não especificou, prefira runtimes com alta penetração (Python 3.11+, Node LTS, Go stable).
- **Sem lock-in gratuito**: evite frameworks que tornam a migração futura custosa sem benefício claro no curto prazo.

---

## 2. Stacks por Tipo de Projeto

### Backend / API

**Stack preferida**: Python 3.12 + FastAPI + Pydantic v2
- FastAPI para APIs REST com documentação OpenAPI automática
- Pydantic v2 para validação de dados e schemas
- `httpx` para chamadas HTTP assíncronas (não `requests` em código async)
- `pytest` + `httpx.AsyncClient` para testes de integração

**Alternativa TypeScript**: Node 22 LTS + Hono (leve) ou Express 5
- `zod` para validação de schema
- `vitest` para testes

**Banco de dados**:
- SQLite via `aiosqlite` + `SQLModel` para projetos locais/pequenos
- PostgreSQL via `asyncpg` + `SQLAlchemy 2.x` async para produção
- Nunca use ORM síncrono em aplicação async

### Frontend / Web

**Stack preferida**: React 19 + TypeScript + Vite
- Tailwind CSS v4 para estilos (utility-first, sem CSS customizado desnecessário)
- `react-query` (TanStack Query) para data fetching e cache de servidor
- `react-hook-form` + `zod` para formulários com validação
- `vitest` + `@testing-library/react` para testes de componente

**Bundler**: Vite (não Webpack, não CRA — obsoleto)

**Sem**: Redux para estado local (use `useState`/`useReducer`/`useContext`), CSS-in-JS em produção nova

### CLI / Scripts

**Python**: `typer` (baseado em Click) para CLIs com subcomandos e help automático
- `rich` para output formatado no terminal
- `pathlib.Path` (não `os.path`)

**Node**: `commander` ou `oclif` para CLIs complexas

**Shell**: Bash para scripts simples de automação. Sempre: `set -euo pipefail` no cabeçalho.

### Dados / ML / IA

**Stack preferida**: Python 3.12 + uv (gerenciador de pacotes)
- `pandas` para manipulação de dados tabulares
- `polars` como alternativa performática ao pandas para datasets grandes
- `openai` SDK para integração com modelos via API
- `langchain` apenas se o grafo de agentes for complexo — evite para uso simples de LLM
- Notebooks Jupyter para exploração; scripts Python para produção

---

## 3. Convenções de Código

### Universais (qualquer linguagem)

- **Funções fazem uma coisa**: se uma função precisa de mais de 20 linhas para fazer seu trabalho, provavelmente faz mais de uma coisa.
- **Nomes são documentação**: prefira `calculate_monthly_revenue()` a `calc()` ou `process()`.
- **Erros são explícitos**: nunca silencie exceções (`except: pass` é proibido). Logue ou re-lance com contexto.
- **Sem números mágicos**: constantes têm nome (`MAX_RETRY_COUNT = 3`, não `if retries > 3`).
- **Sem comentários que repetem o código**: `# incrementa i` sobre `i += 1` é ruído. Comente o *porquê*, não o *o quê*.

### Python

```python
# Estrutura de projeto mínima
projeto/
├── src/
│   └── projeto/
│       ├── __init__.py
│       └── main.py
├── tests/
│   └── test_main.py
├── pyproject.toml   # sempre pyproject.toml, nunca setup.py
└── README.md

# Type hints obrigatórios em funções públicas
def process_order(order_id: str, amount: float) -> dict[str, str]:
    ...

# f-strings (não % nem .format())
message = f"Order {order_id} processed: R${amount:.2f}"

# Pathlib em vez de os.path
config_path = Path.home() / ".config" / "app" / "config.json"
```

### TypeScript / JavaScript

```typescript
// tsconfig: strict: true sempre
// Sem 'any' explícito — use 'unknown' quando o tipo é incerto
function parseResponse(data: unknown): ResponseSchema {
    return ResponseSchema.parse(data); // zod
}

// Imports absolutos com alias (@/)
import { Button } from "@/components/ui/button";

// Async/await, nunca .then().catch() encadeado
const data = await fetchUser(id);

// Estrutura de projeto React
src/
├── components/    # componentes reutilizáveis
├── pages/         # ou app/ em Next.js
├── hooks/         # custom hooks
├── lib/           # utilitários e clientes de API
└── types/         # interfaces e tipos globais
```

---

## 4. Qualidade e Segurança

### Segurança (OWASP Top 10 — mínimo)

- **Nunca interpole input do usuário em SQL**: use prepared statements / ORM sempre.
- **Nunca logue dados sensíveis**: senhas, tokens, CPF/CNPJ, dados de cartão fora de logs.
- **Variáveis de ambiente para segredos**: nunca hardcode API keys, connection strings ou credenciais. Use `.env` + `python-dotenv` / `dotenv` node, com `.env` no `.gitignore`.
- **CORS restritivo**: em produção, whitelist explícita de origens. Nunca `*` em APIs autenticadas.
- **Validação na fronteira**: valide e sanitize todo input externo (HTTP, CLI args, arquivos) antes de processar.

### Testes

| Tipo | O que cobre | Ferramenta preferida |
|------|-------------|---------------------|
| Unitário | Funções puras, lógica de negócio | pytest / vitest |
| Integração | Endpoints de API, queries de banco | pytest + httpx / supertest |
| E2E | Fluxos completos do usuário | Playwright |

**Cobertura mínima**: 80% em código de negócio. Não force cobertura em boilerplate.

### Linting e Formatação

| Linguagem | Linter | Formatter |
|-----------|--------|-----------|
| Python | `ruff` (substitui flake8 + isort + pylint) | `ruff format` |
| TypeScript | `eslint` com `typescript-eslint` | `prettier` |
| CSS | `stylelint` | `prettier` |

**CI mínimo**: lint + type-check + testes em todo push. Sem merges com CI vermelho.

---

## 5. Checklist de Entrega de Código

Antes de marcar qualquer implementação como pronta:

- [ ] Type hints / tipos estáticos em todas as funções públicas?
- [ ] Nenhum segredo hardcoded? (API keys, senhas, tokens)
- [ ] Input externo validado na fronteira?
- [ ] Erros tratados explicitamente (sem `except: pass` ou `.catch(() => {})` vazios)?
- [ ] Testes cobrindo os critérios de aceitação do sprint-N-contract?
- [ ] Linter passa sem warnings?
- [ ] README atualizado se a interface pública mudou?
