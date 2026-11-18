# 05 — Caso Prático (`pi-father-spinner`), Modos do Pi e Papel do Time

Para fechar a apresentação, conectamos todos os fundamentos em três partes: **como o Pi se adapta da tela interativa à automação**, um **caso prático open source real** construído sobre ele (`pi-father-spinner`) e o **checklist prático para o time**.

---

## 1. Do Terminal à Automação: Os 4 Modos de Execução do Pi

Um bom harness não serve só para um humano digitar no terminal. Construído sobre uma sessão compartilhada (`AgentSession`), o Pi opera em quatro modos:

| Modo                 | Como funciona                                     | Onde faz sentido                                      |
| :------------------- | :------------------------------------------------ | :---------------------------------------------------- |
| **Interativo (TUI)** | Interface rica no terminal em tempo real.         | *Pair programming*, exploração, depuração guiada.     |
| **Print / JSON**     | Execução *headless* com saída estruturada.        | Scripts de automação, pipelines de CI/CD, relatórios. |
| **RPC**              | Protocolo JSON-line via `stdin`/`stdout`.         | Acoplar o agente a editores ou interfaces externas.   |
| **SDK (TypeScript)** | Importado diretamente como biblioteca em Node/TS. | Criar produtos, bots e orquestradores próprios.       |

---

## 2. Caso Prático Real: `pi-father-spinner` (Open Source / MIT)

Para provar que **Harness Engineering** com núcleo minimalista funciona na vida real, vejamos o projeto aberto **`pi-father-spinner`** — um orquestrador persistente e bridge Telegram multi-instância construído diretamente sobre `@earendil-works/pi-coding-agent`, `@mariozechner/pi-agent-core` e `@mariozechner/pi-ai`.

![[caso-pratico-pi-father-spinner.svg]]

### Como o `pi-father-spinner` aplica na prática tudo o que vimos:

1. **Isolamento de Contexto e Eficiência ("Sem Ferrari na padaria"):**
   - **1 Projeto = 1 Bot no Telegram = 1 Sessão `tmux` (`pi-<slug>`) = 1 Contexto Isolado.**
   - Em vez de misturar cinco repositórios na mesma conversa (o que polui o contexto e explode o gasto de tokens), cada projeto recebe um agente filho no seu próprio diretório.
   - Usa o guard **`PI_CHILD_AGENT=1`** para **não carregar** as ferramentas de orquestração (`father_*`) nos agentes filhos — mantendo cada filho leve e focado apenas no código do seu projeto.

2. **Extensibilidade Modular sobre o Núcleo do Pi (`pi-package`):**
   - [`extensions/father-spinner.ts`](file:///home/dante/projects/pi-father-spinner/extensions/father-spinner.ts): registra ferramentas de ciclo de vida (`father_spin_agent`, `father_bot_pool`, `father_list_agents`, `father_stop_agent`, `father_inspect_agent`, `father_reconcile_agents`) e comandos `/father-*`.
   - [`extensions/telegram.ts`](file:///home/dante/projects/pi-father-spinner/extensions/telegram.ts): transforma cada sessão em um bot dedicado no Telegram (streaming, envio de arquivos/imagens com `telegram_attach`, `setMyName`, `/compact`, `/status`).
   - [`extensions/tavily.ts`](file:///home/dante/projects/pi-father-spinner/extensions/tavily.ts): adiciona pesquisa web e extração de URLs (`tavily_search`, `tavily_extract`).
   - [`skills/diode-tunneling/SKILL.md`](file:///home/dante/projects/pi-father-spinner/skills/diode-tunneling/SKILL.md): skill sob demanda que ensina o agente a expor servidores locais com HTTPS via Diode CLI sem precisar pesquisar na internet.

3. **Guardrails Reais (Prompt + Sistema Operacional) e Durabilidade:**
   - **Instruções explícitas (`AGENTS.md`):** regras claras de operação (ex.: nunca rodar `pi <text>` via `bash` para evitar lançar subagentes descontrolados; nunca alterar o código do próprio orquestrador).
   - **Trava física no sistema (`chmod a-w`):** o playbook Ansible remove permissão de escrita do código do orquestrador em runtime — provando que **segurança de verdade está no ambiente, não só no prompt**.
   - **Recuperação durável:** estado em `node:sqlite` (WAL) com log de auditoria (`audit_log`); se a VM reiniciar ou uma sessão `tmux` cair, o orquestrador reconcilia e retoma com `pi -c` (`--continue`).

4. **Zero Vendor Lock-in de Ponta a Ponta:**
   - Infraestrutura 100% reproduzível via **Terraform + Ansible** (AWS Lightsail + `systemd`), conectando o mesmo harness a modelos intercambiáveis via `pi-ai` (ex.: `deepseek.v3.2` / `kimi-k3` via Bedrock ou modelos locais).

---

## 3. Checklist *Agent-Ready*: Como Preparar seu Projeto e Time

Independentemente da ferramenta usada, projetos onde agentes de IA brilham têm quatro características simples:

- [ ] **Verificação rápida e determinística:** um comando claro de teste, build e lint que devolva erro objetivo.
- [ ] **Mensagens de erro legíveis:** tipos fortes e logs claros guiam o agente no loop de correção.
- [ ] **`AGENTS.md` curto e vivo na raiz:** o essencial de stack, comandos e limites (sem virar uma enciclopédia).
- [ ] **Escopo pequeno e revisão humana:** tarefas bem delimitadas, commits frequentes e humano validando o `git diff`.

---

## 4. Mensagem Final: O Novo Papel do Time

> A IA no terminal não substitui a engenharia — **ela muda o nível de abstração do nosso trabalho.**

O papel do time deixa de ser "escrever cada linha de sintaxe manualmente" e passa a ser:
1. **Definir o objetivo e os critérios de aceite** (o que é "pronto"?).
2. **Projetar o terreno e o harness** (testes, contexto, guardrails, ferramentas certas para cada tarefa).
3. **Escolher o carro certo para a viagem** (eficiência de modelo e complexidade sem desperdício).
4. **Revisar criticamente o resultado** (arquitetura, segurança e impacto no negócio).
