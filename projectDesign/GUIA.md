# GUIA — CLAUDE.md para direcionar o /design no Claude Code

Este guia explica como escrever um `CLAUDE.md` que direciona o trabalho visual do
comando `/design` (research preview) no Claude Code, e por extensão qualquer trabalho
de UI feito por agentes que leem o arquivo.

> **Contexto:** o `/design` gera artboards de UI editáveis (construídos sobre
> Artifacts) a partir de uma sessão do Claude Code. Ele lê o codebase e trata o
> design system documentado no `CLAUDE.md` (ou num arquivo de tema referenciado)
> como prioridade sobre as escolhas padrão do modelo. Por ser research preview,
> o comportamento pode mudar — este guia descreve boas práticas que funcionam
> com qualquer agente, `/design` incluído.

---

## 1. O princípio: restrição, não decoração

O modelo já sabe fazer "um dashboard bonito" — o problema é que o bonito padrão
dele converge para clichês de IA (gradientes, roxo, cards genéricos, fundo creme
com serifa e acento terracota).

O papel do `CLAUDE.md` é **fechar os eixos de decisão**: cada escolha visual que
você documenta é uma escolha que o agente não vai inventar. Quanto menos espaço
para adivinhação, mais os artboards nascem com a cara do seu produto.

**Regra prática:** escreva como se estivesse fazendo onboarding de um designer
freelancer que nunca viu seu produto e não pode fazer perguntas.

---

## 2. Estrutura recomendada

### Opção A — tudo no CLAUDE.md (projetos pequenos)

Uma seção `## Design System` dentro do próprio `CLAUDE.md`, cobrindo:

1. **Projeto e público** — o que é o produto, para quem, em que contexto de uso,
   qual o tom. Isso influencia dezenas de microdecisões (densidade, tamanho de
   fonte, quanto "flair" cabe) que nenhuma lista de cores captura.
2. **Cores** — valores hex nomeados, com o *papel* de cada uma. Inclua as
   semânticas (sucesso/erro/aviso) e as **proibições**.
3. **Tipografia** — fontes por papel (display, corpo, dados), escala fechada de
   tamanhos ("não inventar tamanhos").
4. **Espaçamento e forma** — grid base, valores válidos de espaçamento,
   border-radius único, política de sombras/elevação.
5. **Componentes existentes** — onde vivem no repo e a ordem "sempre reusar
   antes de criar".
6. **Padrões de layout** — estrutura canônica das telas (sidebar, larguras
   máximas, densidade de tabelas, empty states).
7. **Voz da interface** — idioma, capitalização, voz ativa, como erros falam.

### Opção B — CLAUDE.md aponta para arquivos de tema (projetos maiores)

O `CLAUDE.md` entra em *todo* contexto do agente, então cada linha custa.
Em projetos grandes, mantenha-o enxuto e aponte para os detalhes:

```markdown
## Design
Antes de qualquer trabalho visual ou /design, leia:
- design/THEME.md    — tokens, tipografia, regras visuais
- design/PATTERNS.md — layouts canônicos por tipo de tela
- packages/ui/README.md — inventário de componentes
Nunca introduza cor, fonte ou espaçamento fora de THEME.md.
```

Isso segue o mesmo princípio de *progressive disclosure* das skills: o mapa fica
sempre no contexto; o detalhe pesado só é lido quando o trabalho é visual.

---

## 3. O que faz diferença de verdade

**Negativas explícitas valem mais que positivas.**
`PROIBIDO: gradientes, roxo, glassmorphism` corta os clichês na raiz. Liste os
tiques visuais que você já viu a IA produzir e não quer no seu produto.

**Explique o porquê das regras.**
"Tabelas densas são o padrão porque nosso usuário quer ver muitos registros de
uma vez" generaliza melhor que a regra seca. Quando o agente encontrar uma tela
que você não previu, o racional guia a extrapolação; a regra isolada, não.

**Aponte para o código real.**
O `/design` lê o codebase, mas num research preview não confie que ele acha tudo
sozinho: documente onde a biblioteca de componentes vive (ex.: `packages/ui/`) e,
na hora de implementar o artboard escolhido, referencie-a explicitamente no
prompt (ex.: `@packages/ui`). Revise antes do merge como qualquer UI gerada por
IA: diff visual, snapshot e checagem de acessibilidade (contraste, ordem de
leitura).

**Um exemplo canônico ancora tudo.**
Se existe uma tela que é "a cara certa do produto", diga:
`A tela de referência é src/pages/Dashboard.tsx — em dúvida, pareça com ela.`
Modelos imitam um exemplar muito melhor do que interpolam regras abstratas.

**Contexto de produto vem antes dos tokens.**
Duas linhas sobre público e tom no topo da seção influenciam mais o resultado
do que dez linhas de hex codes.

---

## 4. Fluxo de trabalho sugerido

1. Comece com um `CLAUDE.md` mínimo (cores, fontes, proibições) e rode `/design`
   numa feature real.
2. Cada artboard que sair "errado", pergunte-se: **que regra não estava
   escrita?** Adicione-a. O `CLAUDE.md` cresce por acúmulo de correções, como um
   post-mortem contínuo.
3. Quando o sistema estabilizar, considere o `/design-sync` para sincronizar o
   design system entre o repositório e o Claude Design, evitando divergência
   entre o tema do repo e as edições feitas no editor de artboards.

---

## 5. Ressalvas

- O `/design` foi lançado em agosto/2026 como research preview: o quanto ele
  pesa cada seção do `CLAUDE.md` ainda é território empírico. A estrutura deste
  guia maximiza as chances com qualquer agente que leia o arquivo.
- Requisitos na época do lançamento: Claude Code CLI ≥ 2.1.183 ou app desktop
  ≥ 1.13576.0, sessão autenticada via `/login` (planos Pro, Max, Team,
  Enterprise). Os artboards abrem como páginas editáveis no claude.ai.

---

*Arquivo companheiro: veja `CLAUDE.md` neste diretório para um exemplo completo
e comentado.*
