# Spec · Sessão SESSION_ID

> **Autor**: HAL Planner  
> **Input**: _Transcrição do prompt original do usuário (copie aqui sem edição)_  
> **Data**: YYYY-MM-DD

---

## 1. Visão do Produto

**Problema**: _Qual dor específica este produto resolve? Para quem?_

**Solução**: _Descrita em uma frase. Sem jargão técnico._

**Sucesso**: _Como sabemos que o produto funcionou? Métrica ou comportamento observável._

---

## 2. Público-Alvo

**Usuário primário**: _Persona principal (ex: desenvolvedor solo trabalhando offline)_

**Contexto de uso**: _Quando, onde e como este produto é usado (ex: terminal, browser, API)_

---

## 3. Features Priorizadas

> Use MoSCoW. Cada feature tem critério de sucesso verificável.  
> **Must Have**: sem isso o produto não existe.  
> **Should Have**: importante, mas o MVP sobrevive sem.  
> **Could Have**: nice-to-have para versões futuras.  
> **Won't Have**: explicitamente fora do escopo desta versão.

### Must Have

| # | Feature | Critério de Sucesso |
|---|---------|---------------------|
| 1 | _Nome da feature_ | _Condição verificável: "quando X, o sistema faz Y"_ |
| 2 | | |
| 3 | | |

### Should Have

| # | Feature | Critério de Sucesso |
|---|---------|---------------------|
| 1 | | |

### Could Have

- _Feature futura 1_
- _Feature futura 2_

### Won't Have (nesta versão)

- _Item excluído 1 e motivo_
- _Item excluído 2 e motivo_
- _Item excluído 3 e motivo_

---

## 4. Estratégia Técnica

> Alto nível apenas — sem código, sem funções, sem schemas.

**Stack recomendada**: _Linguagem, runtime, framework — com justificativa se não-óbvio_

**Arquitetura**: _Padrão estrutural (ex: CLI com subcomandos, SPA + API REST, pipeline de dados)_

**Persistência**: _Onde e como os dados ficam (ex: SQLite local, arquivo JSON, sem persistência)_

**Integrações externas**: _APIs, serviços, dependências de terceiros (ex: nenhuma / OpenAI API / GitHub API)_

**Skills relevantes consultadas**:
- [ ] `skills/tech-stack/SKILL.md` — convenções de stack
- [ ] `skills/visual-design/SKILL.md` — padrões de design visual

---

## 5. Oportunidades de IA

> Onde o produto pode usar geração, classificação, embeddings ou raciocínio de modelos para entregar valor diferenciado. Se não aplicável, escreva "Nenhuma nesta versão."

- _Oportunidade 1: descrição e valor esperado_

---

## 6. Fora do Escopo

> Mínimo 3 itens. Fronteiras explícitas previnem scope creep.

- **Sem X**: _motivo_
- **Sem Y**: _motivo_
- **Sem Z**: _motivo_

---

## 7. Suposições

> Toda decisão baseada em inferência (não em input do usuário) deve ser listada aqui.  
> Se uma suposição se provar errada, a spec deve ser revisada antes de continuar.

| # | Suposição | Impacto se falsa |
|---|-----------|-----------------|
| 1 | _ex: usuário tem Python 3.12+ instalado_ | _Generator precisará adicionar instruções de setup_ |
| 2 | | |

---

## 8. Próximos Passos

1. Passe este arquivo ao **HAL Generator** com a instrução: _"Leia `session/SESSION_ID/product-spec.md` e crie o `sprint-N-contract.md` para o Sprint 1 cobrindo os Must Have."_
2. O Generator e o Evaluator negociam o sprint-N-contract antes de qualquer implementação.
3. Sprints subsequentes cobrem Should Have na ordem de prioridade.
