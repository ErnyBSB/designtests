# MeuApp — CLAUDE.md (exemplo)

<!--
Este é um CLAUDE.md de exemplo para um produto fictício, ilustrando como
direcionar o trabalho visual do /design e de qualquer agente que leia este
arquivo. Adapte cada seção ao seu produto real. Veja GUIA.md para o racional
de cada escolha.
-->

## Projeto

SaaS de gestão financeira para pequenas clínicas. Público: administradores
não-técnicos, 35-60 anos, usam no desktop durante o expediente. Tom: confiável
e calmo, nunca "startup hype".

## Comandos

- Dev: `npm run dev` (porta 3000)
- Testes: `npm test` — rodar antes de qualquer commit
- Lint: `npm run lint`

## Design System

### Cores

- Primária: `#0F4C5C` (petrol) — ações principais, links
- Acento: `#E36414` (âmbar queimado) — APENAS alertas de vencimento e CTAs de
  cobrança. Nunca decorativo.
- Fundo: `#FAFAF8` / Superfícies: `#FFFFFF`
- Texto: `#1A1A1A` primário, `#5C5C5C` secundário
- Semânticas: sucesso `#2D6A4F`, erro `#9B2226`, aviso `#E36414`
- PROIBIDO: gradientes, roxo, glassmorphism, dark mode (não temos)

### Tipografia

- Display: "Fraunces", serif — só em h1/h2, weight 600
- Corpo: "Inter", 16px base, line-height 1.6
- Dados/tabelas: "IBM Plex Mono", 14px
- Escala fechada: 14 / 16 / 20 / 28 / 40px. Não inventar tamanhos.

### Espaçamento e forma

- Grid de 8px. Espaçamentos válidos: 8 / 16 / 24 / 32 / 48 / 64
- Border-radius: 6px em tudo (cards, botões, inputs). Nunca pill.
- Sombras: apenas `0 1px 3px rgba(0,0,0,0.08)`. Uma elevação só.

### Componentes existentes

- Biblioteca em `packages/ui/` (shadcn customizado)
- Botões: variantes `primary | secondary | ghost | destructive`
- SEMPRE reusar componentes de `packages/ui/` antes de criar novos
- Ao implementar um artboard do /design, referenciar `@packages/ui`
  explicitamente no prompt

### Padrões de layout

- Sidebar fixa 240px à esquerda, conteúdo max-width 1200px
- Tabelas densas são o padrão — nosso usuário quer ver muitos registros de uma
  vez, não cards espaçosos
- Empty states: ilustração pequena + uma frase + um CTA
- Tela de referência: `src/pages/Dashboard.tsx` — em dúvida, pareça com ela

### Voz da interface

- Português brasileiro, sentence case, voz ativa
- Botões dizem o que fazem: "Salvar alterações", não "OK"
- Erros explicam o que aconteceu e como resolver; nunca pedem desculpas,
  nunca são vagos
- Mesma ação mantém o mesmo nome no fluxo inteiro: botão "Publicar" gera
  toast "Publicado"

## Acessibilidade (piso de qualidade)

- Contraste mínimo AA em todo texto
- Foco de teclado visível em todos os elementos interativos
- `prefers-reduced-motion` respeitado — animações são progressivas, nunca
  essenciais
