# Avaliação · Sessão SESSION_ID · Sprint N

> Template para o HAL Evaluator. Preencha todas as seções. Scores vagos ou elogios genéricos são proibidos — toda pontuação deve ter justificativa referenciada.

---

## Veredicto

**[APROVADO | APROVADO COM RESSALVAS | REPROVADO]** — Score total: X/25

> Regra: ≥ 20 aprovado, 15–19 aprovado com ressalvas, < 15 reprovado.

---

## Scores por Dimensão

| Dimensão | Score | Justificativa |
|----------|-------|---------------|
| Conformidade com o contrato | /5 | _AC-N atendidos: ... AC-N não atendidos: ..._ |
| Corretude funcional | /5 | _Funciona nos casos: ... Falha nos casos: ..._ |
| Qualidade do código | /5 | _Problemas encontrados em `path/to/file.ext`: ..._ |
| Completude da entrega | /5 | _Seções presentes/ausentes: ..._ |
| Dívida técnica declarada | /5 | _Dívidas declaradas: ... Dívidas ocultas encontradas: ..._ |

---

## O Que Falhou

> Liste apenas falhas concretas com referência específica. Sem itens vagos.

- **[Dimensão]**: _O que especificamente falhou_ — _por que isso viola o critério AC-N ou a rubrica_
- **[Dimensão]**: _..._

---

## Ações Corretivas

> Para o Generator executar antes do próximo sprint. Ordem de prioridade.

1. _Ação específica e verificável — critério violado: AC-N_
2. _Ação específica e verificável_

---

## O Que Funcionou

> Apenas observações específicas com referência a arquivo ou comportamento concreto. Sem elogios genéricos.

- _`path/to/file.ext` implementa corretamente X porque Y_

---

## Decisão de Continuidade

- [ ] Avançar para Sprint N+1
- [ ] Generator corrige os itens sinalizados e resubmete este sprint
- [ ] Generator refaz o sprint do zero com este feedback como entrada
