# 04 — Extensibilidade, Guardrails e Engenharia de Contexto

Se o núcleo do **Pi** é um carro urbano enxuto (`read`, `write`, `edit`, `bash` + prompt mínimo), projetos grandes, códigos legados e ambientes críticos são a **trilha off-road**: exigem mapa, sensores, freios reforçados e acessórios específicos.

No Pi, tudo isso entra **de forma explícita e modular**, sem transformar o núcleo em uma caixa-preta.

![[extensibilidade-guardrails-pi.svg]]

---

## 1. Engenharia de Contexto: O Mapa da Trilha

O modelo é novo a cada sessão — ele não conhece o histórico da sua empresa nem como seu projeto roda. Em vez de digitar as mesmas explicações todo dia, estruturamos o contexto em camadas:

- **`AGENTS.md` (na raiz do repositório):** O manual curto do projeto. Diz ao agente qual é a stack, onde ficam os módulos principais, quais comandos rodam testes/linters e o que evitar. Funciona como um padrão aberto portátil entre diferentes agentes.
- **`SYSTEM.md` (global ou por projeto):** Permite ajustar ou substituir instruções de comportamento base do agente para a realidade do time.
- **Skills e Templates de Prompt:** Guias especializados (ex.: *checklist de revisão de PR*, *padrão de migração de banco*, *como abrir uma feature*) que ficam guardados e só entram na janela de contexto **quando a tarefa exige**, evitando poluir o raciocínio com informação irrelevante.

> **Regra de ouro de Context Engineering:** Contexto bom é contexto **relevante e enxuto**. Encher a janela com documentos longos e regras contraditórias piora a qualidade das respostas.

---

## 2. Extensões em TypeScript: Os Acessórios Off-Road

Quando as 4 ferramentas nativas não bastam, o Pi permite acoplar extensões em TypeScript (`.pi/extensions/` no projeto ou `~/.pi/agent/extensions/` globalmente) que rodam no próprio processo do agente:

- **Novas ferramentas (`pi.registerTool()`):** Expor ações específicas para o LLM — como consultar um banco de homologação, buscar na web, acionar uma API interna ou fazer perguntas estruturadas ao usuário via TUI (botões/caixas de seleção em vez de texto solto).
- **Comandos customizados (`/comando`) e UI:** Criar atalhos de fluxo para o time dentro do próprio terminal.
- **Padrões Multiagente (quando o terreno pede):** Em tarefas longas ou complexas, é possível estender o loop simples para arquiteturas como **Planejador $\rightarrow$ Gerador $\rightarrow$ Avaliador** (inspiradas nas pesquisas da Anthropic para evitar que o próprio agente aprove código quebrado por "viés de autoavaliação").
- **Pi Packages:** Tudo isso (extensões, skills e templates) pode ser versionado e compartilhado com a equipe via **Git** ou **npm**.

---

## 3. Guardrails e Freios: Segurança e Controle Humano

Dar acesso ao shell (`bash`) sem freios em um projeto sensível é arriscado. Em um bom harness, segurança não depende da "boa vontade" do modelo — ela é imposta por **código determinístico ao redor dele**:

- **Interceptação de eventos (*Hooks*):** Extensões do Pi podem inspecionar toda chamada de ferramenta *antes* de ela rodar — bloqueando leituras/escritas em arquivos `.env`, impedindo comandos destrutivos (`rm -rf`, `git push --force`) ou exigindo confirmação explícita.
- **Human-in-the-Loop:** Para ações irreversíveis ou mudanças arquiteturais, o harness pausa e pede aprovação humana clara.
- **Checkpoints e Rastreabilidade:** Uso contínuo de `git diff`, commits pequenos e persistência de estado (`pi.appendEntry()`) garantem que qualquer passo errado possa ser auditado e revertido em segundos.

---

## 4. Sensores Objetivos: Testes, Tipos e Linters

Um dos maiores erros ao usar IA para código é deixar o agente apenas "olhar" o arquivo e dizer *"pronto, parece certo!"*.

- **Sem sensores:** O agente confia no próprio palpite (*self-evaluation bias*) e entrega bugs sutis.
- **Com sensores:** Testes automatizados rápidos, checagem de tipos (TypeScript, mypy, etc.) e linters dão um retorno objetivo (**Passou / Falhou**).
- **Mensagens de erro como combustível:** Quanto mais claras forem as mensagens de erro do seu projeto no terminal, melhor o agente corrige o próprio caminho sozinho no loop.

---

## 5. O Efeito Escher: O Harness que Se Auto-Estende e Se Auto-Corrige

![[escher-harness-autoextensao.jpg]]

Há uma propriedade única quando juntamos **código aberto** com um **agente que lê, edita e testa código** (`read`, `edit`, `bash`):

- **O harness consegue ler a si mesmo:** Como o código-fonte do Pi e das suas extensões está aberto e acessível no disco, o próprio agente entende como funciona por dentro.
- **Auto-extensão:** Você pode pedir ao Pi para criar uma nova extensão em `.pi/extensions/`, escrever uma nova `SKILL.md` ou ajustar um comando `/` para a necessidade da equipe.
- **Auto-correção:** Quando uma ferramenta ou extensão falha, o agente lê o *stack trace* no terminal, inspeciona o arquivo da extensão, corrige o bug e testa novamente.
- **A tensão entre evolução e produção:** A mesma capacidade de **reescrever a si mesmo** que acelera o desenvolvimento do harness precisa de trava em produção — é exatamente por isso que projetos como o `pi-father-spinner` aplicam **`chmod a-w`** no código do orquestrador em runtime.

> **Próximo passo na apresentação:** No [[05 - Caso Pratico (pi-father-spinner), Modos e Papel do Time|Bloco 5]], veremos todas essas peças (extensões TypeScript, *Skills*, `AGENTS.md`, `Pi Packages` e travas reais no sistema operacional) funcionando juntas em um projeto open source real: o **`pi-father-spinner`**.
