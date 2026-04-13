# HAL — Harness for Agentic Loops · Cérebro (HAL-1)

Este workspace usa o protocolo **HAL**, um meta-harness estruturado em 6 componentes que governa o comportamento de todos os agentes Copilot aqui dentro.

## Componentes do HAL

| ID | Nome | Recurso | Caminho |
|----|------|---------|----------|
| HAL-1 | Cérebro | `copilot-instructions.md` | `.github/copilot-instructions.md` ← este arquivo |
| HAL-2 | Orchestrator | Agent Mode | `.github/agents/hal.agent.md` |
| HAL-3 | Planner | Subagent (interno) | `.github/agents/hal-planner.agent.md` |
| HAL-4 | Generator | Subagent (interno) | `.github/agents/hal-generator.agent.md` |
| HAL-5 | Evaluator | Subagent (interno) | `.github/agents/hal-evaluator.agent.md` |
| HAL-6 | Skills | `SKILL.md` | `.hal/skills/<domínio>/SKILL.md` |
| HAL-7 | Sessão | Arquivos Markdown + JSONL | `session/SESSION_ID/` |

## Protocolo HAL

### Fluxo Padrão

```
Orchestrator (HAL)
    └── HAL Planner → product-spec.md
            └── HAL Generator → sprint-N-contract.md (proposta)
                    └── HAL Evaluator → sprint-N-contract-review-round-R.md (APROVADO?)
                            └── HAL Generator → sprint-N-delivery.md
                                    └── HAL Evaluator → sprint-N-evaluation.md
                                                    └── (loop se score < 20)
```

### Convenções de Artefatos

Todos os artefatos vivem em `session/SESSION_ID/` (data + número sequencial da sessão):

| Artefato | Arquivo | Autor |
|----------|---------|-------|
| Especificação de produto | `product-spec.md` | Planner |
| Contrato de sprint | `sprint-N-contract.md` | Generator + Evaluator |
| Entrega do Generator | `sprint-N-delivery.md` | Generator |
| Relatório de avaliação | `sprint-N-evaluation.md` | Evaluator |

- Artefatos são **append-only**: nunca sobrescreva, crie versões novas.
- Cada artefato começa com cabeçalho: `# [Tipo] · Sessão SESSION_ID · Sprint N`.
- O Evaluator lê o `sprint-N-contract.md` antes de avaliar — nunca avalia sem contrato.

### Handoff de Contexto

Quando o contexto crescer demais (> 80% da janela estimada), execute um **context reset**:
1. Salve o estado atual em `session/SESSION_ID/context-snapshot.md`.
2. Inicie nova sessão lendo apenas o snapshot + artefatos relevantes.
3. Nunca presuma que o contexto anterior está disponível.

### Comunicação Entre Agentes

- Agentes se comunicam **exclusivamente via arquivos** em `session/`. Nenhum agente invoca outro diretamente no mesmo contexto.
- O Generator **escreve** entregas; o Evaluator **lê** entregas. Nunca o contrário.
- Skills são lidas **sob demanda** pelo agente que as precisa, não globalmente.

### Como Iniciar uma Sessão

1. Selecione o agent **HAL** na lista de agents do Copilot Chat.
2. Descreva o produto/feature em 1–4 frases.
3. O orchestrator cria a sessão, executa o ciclo completo e reporta o resultado.
4. Para retomar sessão interrompida: diga `retomar SESSION_ID`.

Nenhuma intervenção manual é necessária entre as etapas. O orchestrator cuida do loop.

### Skills — Como Usar

Skills são pacotes de conhecimento de domínio em `skills/<domínio>/SKILL.md`. São lidas sob demanda — não injete globalmente.

| Skill | Caminho | Quando ler |
|-------|---------|-----------|
| Design visual | `.hal/skills/visual-design/SKILL.md` | Ao especificar ou implementar qualquer interface visual (web, CLI com output formatado) |
| Stack técnico | `.hal/skills/tech-stack/SKILL.md` | Ao recomendar stack (Planner) ou antes de escrever código (Generator) |

**Regra**: se o domínio da tarefa tem um `SKILL.md`, leia-o antes de agir. Nunca decida convenções de código ou design sem consultar a skill relevante.

## Princípios de Design

1. **Simplicidade primeiro**: complexidade só é adicionada com evidência de necessidade.
2. **Finalidade única**: cada componente faz exatamente uma coisa.
3. **Suposições explícitas**: as suposições abaixo documentam por que cada componente existe.
4. **Desacoplamento**: Cérebro, Ferramentas e Sessão evoluem de forma independente.
5. **Harnesses ficam obsoletos**: a cada novo modelo, revisar e remover scaffolding desnecessário.

## Suposições do Harness Atual

> Estas suposições justificam a existência de cada componente. Quando tornarem-se falsas, o componente correspondente deve ser removido.

| Suposição | Componente que mitiga |
|-----------|----------------------|
| Modelos perdem coerência em contextos muito longos | HAL-7 Sessão (log JSONL + retomada por arquivos) |
| Modelos tendem a avaliar seu próprio trabalho de forma enviesada | HAL-5 Evaluator (subagent separado, persona cética) |
| Critérios subjetivos produzem feedback inconsistente | HAL-5 Evaluator (rubricas objetivadas na persona) |
| Modelos não planejam e implementam bem ao mesmo tempo | HAL-3 e HAL-4 (separação de papéis via subagents) |

## Referências

- Templates de sessão: `.hal/templates/`
- Documentação dos artigos base: `docs/CONCEPTION.md`

## Skills Disponíveis

| Skill | Arquivo | Domínio |
|-------|---------|---------|
| Design Visual | `.hal/skills/visual-design/SKILL.md` | Hierarquia visual, componentes web, anti-padrões de UI, checklist de acessibilidade |
| Tech Stack | `.hal/skills/tech-stack/SKILL.md` | Stacks por tipo de projeto, convenções de código, segurança (OWASP), checklist de entrega |

## Agents Disponíveis

| Agent | Arquivo | Visível ao usuário |
|-------|---------|-------------------|
| HAL (Orchestrator) | `.github/agents/hal.agent.md` | Sim — único ponto de entrada |
| HAL Planner | `.github/agents/hal-planner.agent.md` | Não (interno) |
| HAL Generator | `.github/agents/hal-generator.agent.md` | Não (interno) |
| HAL Evaluator | `.github/agents/hal-evaluator.agent.md` | Não (interno) |

