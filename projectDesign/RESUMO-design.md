# RESUMO — O que é o /design do Claude Code e suas limitações

Resumo da discussão sobre o comando `/design` (research preview), lançado pela
Anthropic em agosto de 2026 para o Claude Code.

---

## 1. O que é

O `/design` traz o workflow de artboards do **Claude Design** (produto do
Anthropic Labs) para dentro do Claude Code, tanto no CLI quanto no app desktop,
construído sobre a infraestrutura de **Artifacts**.

O fluxo:

1. A partir de uma sessão do Claude Code, você roda algo como
   `/design a few options for {feature}`.
2. O Claude lê o codebase, tenta casar com o estilo da UI existente e gera
   **múltiplos artboards editáveis** — mocks compartilháveis publicados como
   Artifacts, que abrem como páginas editáveis no claude.ai.
3. Você escolhe a direção preferida, edita/refina o artboard.
4. Pede ao Claude que **implemente** o design escolhido em código.

A mudança é de **ordem do workflow**: em vez de "descrever → o agente escreve
código → corrigir visual depois" (que mistura ajustes de estrutura e estilo),
passa a existir um artefato de design comparável e confirmável **entre** o
requisito e o código. A decisão visual acontece antes de gastar implementação.

Importante: o `/design` **não introduz nova capacidade de modelo** — ele
reempacota o editor e o prompting do Claude Design como um comando dentro do
Claude Code.

---

## 2. Requisitos e disponibilidade (na época do lançamento)

- Planos: Pro, Max, Team e Enterprise (os mesmos onde Artifacts funcionam).
- Versões mínimas: Claude Code CLI ≥ 2.1.183 ou app desktop ≥ 1.13576.0.
- Sessão autenticada com conta claude.ai via `/login`.
- Os artboards abrem no navegador, em claude.ai.

---

## 3. Integração com design system

- Recomendado documentar cores, tipografia e espaçamento no `CLAUDE.md` do
  projeto ou num arquivo de tema referenciado por ele. O Claude trata o design
  system documentado como **prioridade sobre as escolhas padrão do modelo**,
  então os artboards nascem consistentes com a identidade do produto.
- O comando `/design-sync` sincroniza o design system nos dois sentidos entre
  o repositório e o Claude Design, evitando divergência entre o tema do repo e
  as edições feitas no editor de artboards.
- (Ver GUIA.md e CLAUDE.md de exemplo neste diretório para como estruturar
  esse direcionamento.)

---

## 4. Por que é uma "skill" e não outra categoria de ferramenta

A taxonomia real do Claude Code tem três camadas que costumam ser confundidas:

- **Tools**: capacidades em código (Read, Bash, publicar Artifact...). É onde
  vive a infraestrutura de verdade — por isso o requisito de versão mínima do
  binário.
- **Skills**: instruções em markdown carregadas no contexto; ensinam o modelo a
  orquestrar as tools num workflow. Não executam nada por si.
- **Slash commands**: apenas o mecanismo de invocação.

No `/design`, a infraestrutura (publicar artifact, autenticar, editor no
claude.ai) está na camada de tool; o **workflow** (gerar N variações, ler o
codebase antes, respeitar o CLAUDE.md, esperar a escolha, implementar) está na
camada de skill. Isso não é exceção: skills como `pptx` e `docx` também
dependem de infraestrutura por baixo — o `/design` só torna a dependência mais
visível porque ela é remota (servidores da Anthropic) em vez de local.

Vantagens práticas dessa escolha de empacotamento:

- **Iteração rápida**: mudar uma skill é editar markdown, sem release do CLI —
  valioso num research preview.
- **Interpretação flexível**: `/design` recebe linguagem natural, não flags
  parseáveis.
- **Composabilidade**: participa do mesmo ecossistema das demais skills e do
  progressive disclosure (só entra no contexto quando invocado).

A tensão legítima: a promessa implícita de uma skill é ser **portável** (um
SKILL.md que qualquer um copia, edita, redistribui). O `/design` quebra isso —
sem conta autenticada, plano certo e versão mínima, o markdown é letra morta.
Na prática surgiu uma categoria não-nomeada: a **skill first-party acoplada a
serviço**, que se veste de skill mas se comporta como feature de produto
(gating por plano, infraestrutura proprietária). Compare com a
`frontend-design`, que é uma skill "pura" — 100% conhecimento, aberta no
repositório público do Claude Code, editável à vontade.

Leitura: a Anthropic usa "skill" menos como categoria técnica rigorosa e mais
como **interface de usuário unificada** — tudo se invoca igual e segue o mesmo
modelo mental, ao custo da pureza taxonômica.

---

## 5. Limitações e ressalvas

1. **Research preview**: funcionalidade em fluxo — comportamento, interface e
   workflow podem mudar antes de estabilizar. Vale testar em projetos reais,
   mas sem assumir que a forma atual é final.
2. **Dependência de serviço**: exige autenticação, plano pago e infraestrutura
   remota da Anthropic. Não funciona offline nem é auto-hospedável; não é
   editável/fork-ável como uma skill comum.
3. **Matching de design system não garantido**: embora o Claude leia o codebase,
   não se deve presumir matching automático — referencie a biblioteca de
   componentes real (ex.: `@packages/ui`) explicitamente no prompt ao passar do
   artboard para a implementação.
4. **Revisão continua obrigatória**: mesmo padrão de qualquer UI gerada por IA —
   revisar o código gerado, rodar diff visual / snapshot (ex.: Storybook) e
   validar acessibilidade (contraste, ordem de leitura) antes do merge.
5. **Peso real do CLAUDE.md ainda é empírico**: a recomendação oficial é que o
   design system documentado tem prioridade, mas o quão fielmente cada seção é
   respeitada só se descobre testando — o produto tem dias de vida.
6. **Não substitui capacidade de design**: o modelo por trás é o mesmo; o que
   muda é o workflow. Os vieses estéticos padrão de IA continuam existindo e
   ainda precisam ser contidos por direcionamento (CLAUDE.md, tema, exemplos
   canônicos).

---

## 6. Relação com a skill `frontend-design`

São coisas distintas e complementares:

| | `frontend-design` | `/design` |
|---|---|---|
| Natureza | Conhecimento (princípios estéticos, processo, anti-clichês) | Workflow (artboards → escolha → edição → implementação) |
| Dependências | Nenhuma — markdown puro | Artifacts, conta claude.ai, plano pago, versão mínima |
| Editável pelo usuário | Sim (aberta no GitHub) | Não (acoplada a serviço) |
| Onde atua | Como o modelo decide visualmente | Em que ordem o trabalho acontece |

É provável que o `/design` use princípios como os da `frontend-design` por
baixo na geração dos artboards.

---

*Arquivos companheiros neste diretório: `GUIA.md` (como construir um CLAUDE.md
para direcionar o /design) e `CLAUDE.md` (exemplo completo comentado).*
