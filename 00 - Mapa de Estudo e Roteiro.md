# IA no Terminal & Harness Engineering (Viés Open Source)

> **Tese central:** Um agente de código confiável não nasce apenas de um modelo inteligente — ele nasce da engenharia ao redor do modelo (**Harness Engineering**). Usando ferramentas abertas e o **Pi** como "raio-x", entendemos os fundamentos sem depender de caixas-pretas ou fornecedores únicos.

## Formato e Público
- **Formatos de apresentação:** [[Apresentacao - Visual (Canvas).canvas|Obsidian Canvas]] e [[Apresentacao - Slides.md|Slides Markdown]], apoiados pelas notas em `notas/`.
- **Público:** Time misto / geral em tecnologia (conceitos claros, analogias visuais e exemplos concretos).
- **Lente:** Open source, fundamentos e independência de fornecedor (*anti-vendor lock-in*).

---

## Roteiro de Estudo (5 Blocos)

- [x] **Bloco 1 — Origem do Termo, Do Chat ao Terminal, os Pioneiros e o Anti-Lock-in**
  - Nota: [[notas/01 - Do Chat ao Terminal e os Pioneiros|01 - Do Chat ao Terminal e os Pioneiros]]
  - Ilustrações: [[imagens/origem-termo-harness.jpg|origem-termo-harness.jpg]], [[imagens/evolucao-chat-ao-terminal.jpg|evolucao-chat-ao-terminal.jpg]]
  - *Temas:* Etimologia de *harness* (arreio/peitoral em cavalos e cães), evolução (Chat Web $\rightarrow$ IDE $\rightarrow$ Terminal), reconhecimento à **Anthropic / Claude Code** como pioneiros do *Harness Engineering*, e por que adotar uma arquitetura aberta sem lock-in.
- [x] **Bloco 2 — Fundamentos de Harness Engineering (O Raio-X com o Pi)**
  - Nota: [[notas/02 - O que e Harness Engineering e o Raio-X com Pi|02 - O que e Harness Engineering e o Raio-X com Pi]]
  - Ilustrações: [[imagens/metafora-carro-harness.jpg|metafora-carro-harness.jpg]], [[imagens/carro-certo-por-terreno.jpg|carro-certo-por-terreno.jpg]], [[imagens/evolucao-motor-ao-automovel.jpg|evolucao-motor-ao-automovel.jpg]], [[imagens/cpu-motor-camada-semantica.jpg|cpu-motor-camada-semantica.jpg]]
  - *Temas:* A equação `Agente = Modelo + Harness`, paralelo histórico do motor estacionário (Lenoir/Otto) ao automóvel (Benz/Daimler), analogia com o chip da **CPU** e a nova **camada semântica**, metáfora do motor vs. chassi/direção (incluindo *Ferrari na padaria* vs. *carro urbano vs. off-road* para eficiência e adequação ao objetivo), e por que o **Pi** (`pi.dev`) é o laboratório ideal pelo seu minimalismo radical.
- [x] **Bloco 3 — Anatomia do Harness por Dentro**
  - Nota: [[notas/03 - Anatomia do Harness por Dentro (Raio-X do Pi)|03 - Anatomia do Harness por Dentro (Raio-X do Pi)]]
  - Diagrama: [[imagens/arquitetura-pi-harness.svg|arquitetura-pi-harness.svg]]
  - *Temas:* As camadas do Pi (`pi-ai` multi-provedor e modelos locais, `pi-agent-core` e o loop agêntico, as **4 ferramentas núcleo** `read`/`write`/`edit`/`bash`, system prompt enxuto e compactação de contexto).
- [x] **Bloco 4 — Extensibilidade, Guardrails e Engenharia de Contexto**
  - Nota: [[notas/04 - Extensibilidade, Guardrails e Engenharia de Contexto|04 - Extensibilidade, Guardrails e Engenharia de Contexto]]
  - Diagrama: [[imagens/extensibilidade-guardrails-pi.svg|extensibilidade-guardrails-pi.svg]]
  - Ilustrações: [[imagens/escher_desenhando.webp|escher_desenhando.webp]], [[imagens/escher-harness-autoextensao.jpg|escher-harness-autoextensao.jpg]]
  - *Temas:* `AGENTS.md`, `SYSTEM.md`, *Skills*, extensões em TypeScript (`pi.registerTool`, hooks), auto-extensão e auto-correção (efeito Escher), permissões, testes como sensores e controle humano (*human-in-the-loop*).
- [x] **Bloco 5 — Caso Prático (`pi-father-spinner`), Modos de Execução e Papel do Time**
  - Nota: [[notas/05 - Caso Pratico (pi-father-spinner), Modos e Papel do Time|05 - Caso Pratico (pi-father-spinner), Modos e Papel do Time]]
  - Diagrama: [[imagens/caso-pratico-pi-father-spinner.svg|caso-pratico-pi-father-spinner.svg]]
  - *Temas:* Modos de execução do Pi (TUI, Print/JSON, RPC, SDK), estudo de caso real com o projeto open source **`pi-father-spinner`**, checklist *agent-ready* e o novo papel do time.
- [x] **Extra — Cola de Palco (Guia Rápido do Apresentador)**
  - Nota: [[notas/06 - Cola de Palco (Roteiro de Fala)|06 - Cola de Palco (Roteiro de Fala)]]
  - *Temas:* Roteiro em 12 pontos com todas as analogias, citações exatas, ganchos e transições para consultar durante a apresentação.
