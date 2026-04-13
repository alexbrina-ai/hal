# 🤖 HAL — Harness for Agentic Loops

Este repositório implementa o **HAL (Harness for Agentic Loops)**, uma prova de conceito (PoC) para o desenvolvimento de software por agentes de IA que trabalham de forma autônoma e iterativa até atingirem um nível de qualidade elevado.

---

## Arquitetura e Agentes

O Orquestrador `HAL` coordena de forma autônoma o seguinte fluxo:

**Planner ➔ Generator ➔ Evaluator 🔄 (Loop)**

*   **HAL Planner**: Inicializa o processo interpretando o prompt e convertendo-o em uma especificação formal de produto (`product-spec.md`), contendo todos os requisitos e critérios de aceitação.

*   **HAL Generator**: Atua como engenheiro de software, gerando e aprimorando ativamente o código para satisfazer os requisitos do produto.

*   **HAL Evaluator**: Funciona como o lado analítico e cético da rede, criticando e avaliando rigorosamente as entregas do *Generator*, solicitando ajustes e refinamentos até o nível de abstração/qualidade esperado ser alcançado.

Na pasta `session/{SESSION_ID}` são armazenados os documentos gerados e o log da interação em formato JSONL para persistência e *handoff* de contexto.

Se interrompido, basta retomar informando no prompt `retome a session {SESSION_ID}`, o `HAL` irá continuar de onde parou.

---

## Concepção

O sistema é inspirado nos conceitos apresentados nestes blog posts:

- https://www.anthropic.com/engineering/harness-design-long-running-apps
- https://www.anthropic.com/engineering/managed-agents

---

## Estrutura do Projeto

Os componentes do `HAL` estão organizados da seguinte forma:

*   `.github/agents/` — Prompts de sistema, personas e comportamentos dos agentes `HAL`.
*   `.hal/` — *Skills* específicas de domínio técnico e os templates-base para o contexto.

---

## Como utilizar

1. Clone este repositório e abra no seu VS Code.
2. Certifique-se de selecionar um modelo avançado em sua interface Copilot Chat (sugestão: Claude Sonnet 4.5+).
3. Selecione o agente `HAL` na lista de agentes.
4. Escreva um *prompt* conciso e direto sobre a nova feature (de 1 a 4 linhas) e veja os agentes trabalharem.

