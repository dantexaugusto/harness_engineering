# IA no Terminal & Harness Engineering
### Fundamentos, Viés Open Source e Raio-X com o Pi

**Tese central:**
`Agente = Modelo (LLM) + Harness`

- O modelo raciocina; o **harness** garante contexto, ação, verificação e segurança.
- Vamos usar o **Pi** (`pi.dev`) como laboratório minimalista e encerrar com um caso prático real: **`pi-father-spinner`**.

---

## Bloco 1 — Do Chat Isolado ao Terminal Aberto

![[evolucao-chat-ao-terminal.jpg]]

- No chat web, a IA está **cega** para o projeto real.
- No terminal, tudo é texto (`stdin`/`stdout`) + acesso real a arquivos, `git` e testes.

---

## Crédito aos Pioneiros & Independência de Fornecedor

### Quem abriu o caminho
- **Anthropic & time do Claude Code** praticamente inauguraram o campo moderno de *Harness Engineering* e provaram (*Building Effective Agents*, 2024) que **loops simples no terminal** superam frameworks pesados.
- Convenções como `CLAUDE.md` inspiraram padrões abertos como **`AGENTS.md`**.

### Por que ir para Open Source & Anti-Lock-in?
- **Modelos mudam rápido:** separe a inteligência (LLM) da infraestrutura (harness).
- **Liberdade de provedor:** alterne entre APIs na nuvem e **modelos locais** (*open-weight*).
- **Zero caixa-preta:** o conhecimento do time vira ativo do repositório.

---

## Bloco 2 — O que é Harness Engineering?

![[metafora-carro-harness.jpg]]

- **Modelo (LLM) = Motor:** força bruta de raciocínio, mas não dirige sozinho.
- **Harness = Engenharia ao Redor:** volante (ferramentas), painel/sensores (contexto e testes) e freios/gaiola (segurança e limites).

---

## Do Motor Estacionário ao Automóvel (1860 $\rightarrow$ 1886)

![[evolucao-motor-ao-automovel.jpg]]

- **Lenoir (1860) & Otto (1876):** motor **útil, mas estacionário** na oficina $\rightarrow$ *como o **Chat Web** na bancada.*
- **Daimler, Maybach & Benz (1885–1886):** combustível portátil + **chassi, direção e freios** $\rightarrow$ *o momento **Harness Engineering**.*

---

## Eficiência: O Carro Certo para Cada Terreno

![[carro-certo-por-terreno.jpg]]

- **Não precisa de uma Ferrari para ir à padaria:** tarefas simples pedem modelo leve e harness mínimo.
- **Cidade vs. Trilha Off-Road:** código limpo aceita agilidade; legado crítico exige 4x4, freios e sensores reforçados.

---

## Bloco 3 — Raio-X do Pi por Dentro

![[arquitetura-pi-harness.svg]]

- **`pi-ai`:** suporte universal de motor (nuvem ou local via `Ollama`/`llama.cpp`, troca via `/model`).
- **`pi-agent-core`:** loop agêntico + compactação automática + *system prompt* **< 1.000 tokens**.
- **4 Ferramentas Núcleo:** `read`, `write`, `edit` e `bash`.

---

## Bloco 4 — Equipando o Carro para o Off-Road

![[extensibilidade-guardrails-pi.svg]]

- **Contexto:** `AGENTS.md`, `SYSTEM.md` e *Skills* carregadas sob demanda.
- **Extensões TS:** `pi.registerTool()`, comandos `/`, subagentes (*Planejador $\rightarrow$ Gerador $\rightarrow$ Avaliador*).
- **Freios & Sensores:** hooks de bloqueio, *human-in-the-loop* + testes, tipos e linters.

---

## O Efeito Escher: O Harness que Se Auto-Estende

![[escher-harness-autoextensao.jpg]]

- **Open Source + `read` / `edit` / `bash`:** o Pi lê o próprio código para **criar suas extensões, skills e corrigir seus próprios bugs**.
- **Quando liberar vs. travar:** superpoder na hora de evoluir a ferramenta; em produção, exige trava no SO (como veremos no `pi-father-spinner` com `chmod a-w`).

---

## Bloco 5 — Do Terminal à Automação (4 Modos do Pi)

| Modo | Uso principal |
| :--- | :--- |
| **Interativo (TUI)** | *Pair programming* humano no terminal |
| **Print / JSON** | Scripts e pipelines de CI/CD (*headless*) |
| **RPC** | Integração via `stdin`/`stdout` com outras UIs |
| **SDK (TypeScript)** | Embutir o agente dentro de produtos próprios |

---

## Caso Prático Real: `pi-father-spinner` (MIT)

![[caso-pratico-pi-father-spinner.svg]]

---

## O que o `pi-father-spinner` Prova na Prática

- **Eficiência e Isolamento:** *1 Projeto = 1 Bot no Telegram = 1 Sessão `tmux` = 1 Contexto Isolado* (reduz tokens e evita poluição cruzada).
- **Escopo Enxuto (`PI_CHILD_AGENT=1`):** filhos não carregam ferramentas de orquestração (`father_*`).
- **Extensibilidade Real:** extensões `father-spinner.ts`, `telegram.ts`, `tavily.ts` + skill `diode-tunneling`.
- **Guardrail no SO + Durabilidade:** `AGENTS.md` + **`chmod a-w`** via Ansible + `node:sqlite` WAL + auto-recovery com `pi -c`.
- **Anti-Lock-in:** Terraform/Ansible próprios + modelos intercambiáveis via `pi-ai`.

---

## Fechamento — Checklist *Agent-Ready* & Papel do Time

### Para o repositório
- Testes rápidos e determinísticos + erros claros no terminal.
- `AGENTS.md` curto e vivo na raiz + revisão humana de `git diff`.

### O novo papel do humano
- De *"digitador de sintaxe"* para **arquiteto de contexto, projetista de harness e revisor crítico**.
