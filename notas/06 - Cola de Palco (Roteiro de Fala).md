# 06 — Cola de Palco (Roteiro Rápido do Apresentador)

Guia enxuto para consultar em uma segunda tela ou tablet durante a apresentação ([Slides](file:///home/dante/obsidian_vaults/harness_engineering/Apresentacao%20-%20Slides.md) ou [Canvas](file:///home/dante/obsidian_vaults/harness_engineering/Apresentacao%20-%20Visual%20%28Canvas%29.canvas)), garantindo que nenhum conceito-chave ou analogia fique de fora.

---

## Abertura & Bloco 1 — Origem, Terminal, Pioneiros e Anti-Lock-in

1. **Origem do termo "Harness" (`origem-termo-harness.jpg`):**
   - *Fala-chave:* Para quem fala português, *"harness"* soa abstrato — em inglês é o **arreio do cavalo** ou **peitoral do cão**. Serve para **canalizar força bruta em trabalho útil, guiado e seguro**.
2. **Do Chat ao Terminal (`evolucao-chat-ao-terminal.jpg`):**
   - *Fala-chave:* No navegador, a IA está cega. No terminal (`stdin`/`stdout`), ela lê arquivos, roda `git` e vê erros reais de testes.
3. **Crédito aos Pioneiros & Viés Open Source:**
   - *Fala-chave:* É justo dar crédito à **Anthropic e ao time do Claude Code**, que praticamente inauguraram o campo moderno de *Harness Engineering* (*Building Effective Agents*, `CLAUDE.md` $\rightarrow$ `AGENTS.md`).
   - *Virada:* Mas nosso foco aqui é **open source, fundamentos e zero vendor lock-in** — separar o *modelo* do *harness*.

---

## Bloco 2 — Metáforas: Carro, História, Eficiência e Camada Semântica

4. **Equação & Carro (`metafora-carro-harness.jpg`):**
   - *Fala-chave:* `Agente = Modelo (LLM) + Harness`. O LLM é só o motor; o harness é chassi, direção, painel/sensores e freios.
5. **Paralelo Histórico Real (`evolucao-motor-ao-automovel.jpg`):**
   - *Fala-chave:* O motor a combustão **não começou fazendo carro**! Huygens (1680) foi pólvora sem controle; **Lenoir (1860) e Otto (1876)** já tinham utilidade comercial clara, mas **estacionária** (preso ao gás encanado da oficina, tocando prensas e teares) — exatamente como o **Chat Web**. Só com **Benz & Daimler (1885–1886)** veio chassi, direção e combustível portátil.
   - *Frase de ouro:* *"Assim como o motor de Lenoir e Otto já era muito útil preso numa bancada de oficina antes de virar automóvel, o LLM no chat web também já era útil. O que transformou um motor estacionário em um carro foi a engenharia de chassi, carburador, direção e freios ao redor dele — exatamente o papel do **Harness Engineering** para transformar um LLM em um agente de código."*
6. **Eficiência & Terreno (`carro-certo-por-terreno.jpg`):**
   - *Fala-chave:* **"Não preciso de uma Ferrari para ir até a padaria da esquina comprar pão."** E **"o carro para uma trilha de barro off-road é diferente de um carro para a cidade."** Adequar modelo + complexidade do harness ao objetivo economiza tokens, custo e tempo.
7. **Analogia da CPU & Camada Semântica (`cpu-motor-camada-semantica.jpg`):**
   - *Fala-chave:* Assim como o **chip da CPU** (transistores miniaturizados) precisou de placa-mãe, memória, sistema operacional, compiladores e interfaces para criar a era digital, o LLM é o "motor" em **fase inicial** de uma **camada semântica** nunca antes explorada — com um campo imenso de engenharia para **canalizar e transduzir** essa energia motriz.

---

## Bloco 3 — Raio-X Interno do Pi (`pi.dev`)

8. **Por que o Pi e como é por dentro (`arquitetura-pi-harness.svg`):**
   - *Fala-chave:* Escolhemos o **Pi** como raio-x pelo seu **minimalismo radical**: `pi-ai` (troca livre de modelos nuvem/locais com `/model`), `pi-agent-core` (*system prompt* **< 1.000 tokens** + compactação) e apenas **4 ferramentas** (`read`, `write`, `edit`, `bash`).
9. **Ecossistema aberto irmão — LangChain, LangGraph & `deepagents`:**
   - *Fala-chave:* Vale citar que o mesmo movimento aberto existe em **LangChain (framework) $\rightarrow$ LangGraph (runtime em grafos com checkpointing e HITL) $\rightarrow$ `deepagents` (harness oficial com `write_todos`, filesystem virtual, subagentes e CLI `dcode`)**. O Pi brilha como raio-x minimalista; o LangGraph/`deepagents` como runtime de grafos e harness "baterias inclusas" em Python/TS.

---

## Bloco 4 — Extensibilidade, Efeito Escher e Guardrails

10. **Equipando para o Off-Road (`extensibilidade-guardrails-pi.svg`):**
    - *Fala-chave:* Quando o projeto exige, acoplamos `AGENTS.md`, *Skills*, extensões TypeScript (`pi.registerTool()`), *hooks* de segurança e testes como sensores objetivos.
11. **O Efeito Escher (`escher_desenhando.webp` + `escher-harness-autoextensao.jpg`):**
    - *Fala-chave:* Por ter **código aberto** e operar com `read` / `edit` / `bash`, o Pi lê o próprio código para **criar suas extensões e corrigir seus próprios bugs** — como as mãos de Escher desenhando a si mesmas.
    - *Gancho de tensão:* Mas em produção, você quer um agente alterando a si mesmo sem limite? Isso leva direto ao caso prático.

---

## Bloco 5 — Casos Práticos (`pi-custom-extensions` & `pi-father-spinner`) & Fechamento

12. **Modos do Pi + `pi-custom-extensions`:**
    - *Fala-chave:* Mostre os 4 modos (TUI, Print/JSON, RPC, SDK) e conecte direto ao **`pi-custom-extensions`**: um repositório MIT com grupos de extensões (`subagentsV1`, `subagents_OpenRouter_multimodel`, `subagents_Bedrock_multimodel`), templates (`/feature`, `/review`, `/wr`), skill `squad-workflows` e subagentes rodando via **`pi --mode json`**.
    - *Conexão com "sem Ferrari na padaria":* cada papel usa o modelo certo (`scout` rápido, `planner`, `worker` mais forte, `tester` e `reviewer` enxutos).
13. **`pi-father-spinner` (`caso-pratico-pi-father-spinner.svg`):**
    - *Fala-chave:* Projeto open source (MIT) construído sobre o Pi para gerenciar vários projetos pelo Telegram: **1 Projeto = 1 Bot = 1 Sessão `tmux` = 1 Contexto Isolado**.
    - *Destaques técnicos que fecham todos os blocos:*
      - `PI_CHILD_AGENT=1` (eficiência: filhos enxutos sem ferramentas de orquestração).
      - Extensões TS (`father-spinner.ts`, `telegram.ts`, `tavily.ts`) + skill `diode-tunneling`.
      - **`chmod a-w` via Ansible no SO** (trava real para o Efeito Escher em runtime!) + `node:sqlite` WAL e recuperação com `pi -c`.
      - Anti-lock-in com Terraform/Ansible e troca de modelos via `pi-ai`.
14. **Fechamento:**
    - *Fala-chave:* O papel do time sobe de nível: de "digitador de sintaxe" para **arquiteto de contexto, projetista de harness e revisor crítico**.
