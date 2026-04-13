# SKILL · Visual Design

> **Domínio**: Design visual de interfaces — web, desktop e CLI  
> **Lido por**: HAL Planner (para orientar decisões de UX na spec) e HAL Generator (para implementar componentes visuais)  
> **Escopo**: Princípios estéticos, padrões de componentes, anti-padrões a evitar

---

## 1. Princípios Fundamentais

### Hierarquia Visual
- **Um foco por tela**: cada tela tem exatamente uma ação primária. Tudo mais é secundário.
- **Peso visual declara importância**: elementos maiores/mais contrastados = mais importantes. Nunca inverta isso.
- **Espaço em branco é conteúdo**: margem e padding não são desperdício — são o que separa sinal de ruído.

### Consistência
- **Paleta limitada**: máximo 2 cores de destaque + neutros. Cada cor tem função única (ex: azul = ação, vermelho = erro).
- **Tipografia monotônica**: máximo 2 famílias tipográficas. Uma para body, uma para headings — se necessário.
- **Grid implícito**: elementos se alinham a uma grade (ex: múltiplos de 4px ou 8px). Nunca posicione elementos arbitrariamente.

### Feedback
- **Estado visível**: toda ação do usuário tem resposta visual imediata (hover, active, loading, disabled).
- **Erros são específicos**: mensagens de erro dizem o que está errado E como corrigir. Nunca apenas "erro".
- **Loading não desaparece**: enquanto algo está processando, o usuário sabe. Nunca UI responsiva sem indicador de progresso.

---

## 2. Padrões por Tipo de Interface

### Web (HTML/CSS)

**Layout**
- Use CSS Grid para layout de página; Flexbox para alinhamento de componentes.
- Breakpoints mínimos: mobile (< 640px), tablet (640–1024px), desktop (> 1024px).
- Container máximo: 1280px com padding horizontal de 16px em mobile, 24px+ em desktop.

**Componentes**
- Botões: padding mínimo de 12px vertical, 20px horizontal. Height mínimo de 44px (acessibilidade touch).
- Inputs: sempre com label visível (nunca apenas placeholder). Estados: default, focus, error, disabled.
- Cards: border-radius consistente (ex: 8px). Sombra sutil apenas quando o card é interativo.
- Modais: fundo com overlay escuro (opacity 0.5). Fechamento via ESC e clique no overlay.

**Cores e Tipografia**
- Contraste mínimo WCAG AA: 4.5:1 para texto normal, 3:1 para texto grande (≥ 18px bold).
- Nunca use apenas cor para transmitir informação — adicione ícone ou texto auxiliar.
- Line-height de body: 1.5–1.6. Headings: 1.2–1.3.

### CLI (Terminal)

**Output**
- Use cores ANSI apenas para: sucesso (verde), erro (vermelho), aviso (amarelo), destaque (azul/ciano).
- Nunca colorize output que será pipado para outros processos — detecte TTY antes.
- Prefixos semânticos: `✓` para sucesso, `✗` para erro, `⚠` para aviso, `→` para progresso.

**Interatividade**
- Confirmações destrutivas exigem digitação explícita (ex: digitar "sim" ou o nome do recurso), não apenas Enter.
- Flags `--dry-run` para operações destrutivas. Flags `--quiet` e `--verbose` para controle de output.
- Help obrigatório: `--help` em todos os comandos e subcomandos.

---

## 3. Anti-Padrões — Nunca Faça

| Anti-padrão | Problema | Alternativa |
|-------------|----------|-------------|
| Texto sobre imagem sem overlay | Legibilidade quebrada | Overlay escuro + texto branco, ou texto fora da imagem |
| Botões sem estado disabled | Ações indisponíveis parecem disponíveis | Sempre desabilite + explique por quê |
| Scroll horizontal em mobile | Quebra a experiência | Use overflow-hidden ou refatore o layout |
| Modais empilhados | Desorientação | No máximo um modal por vez; use páginas/steps |
| Placeholder como label | Desaparece ao digitar | Sempre label visível acima do input |
| Ícones sem texto/tooltip | Ambiguidade de significado | Adicione label ou tooltip descritivo |
| Loaders sem timeout | UI parecer travada | Timeout de 30s com mensagem de erro |
| Cores da marca em texto de erro | Confusão semântica | Vermelho para erro — sem exceções |

---

## 4. Checklist de Revisão Visual

Antes de marcar qualquer entrega visual como pronta, verifique:

- [ ] Hierarquia: há exatamente uma ação primária por tela/seção?
- [ ] Contraste: todos os textos passam no critério WCAG AA?
- [ ] Estados: hover, focus, active, disabled e loading estão implementados em todos os elementos interativos?
- [ ] Erros: mensagens de erro são específicas e orientadas à solução?
- [ ] Responsividade: layout funciona em 375px (mobile mínimo) e 1440px (desktop padrão)?
- [ ] Consistência: cores, espaçamentos e border-radius são uniformes em toda a UI?
- [ ] Acessibilidade: inputs têm labels, imagens têm alt text, botões têm aria-label quando necessário?
