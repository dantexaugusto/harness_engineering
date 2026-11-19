# 01 — Do Chat ao Terminal, os Pioneiros e o Anti-Lock-in

## 0. Antes de Tudo: De Onde Vem o Termo "Harness"?

Para quem fala português do Brasil, a palavra *"harness"* não é intuitiva à primeira vista. Em inglês, **harness** significa literalmente **arreio** (usado em cavalos para puxar carroças/arados e responder às rédeas) ou **peitoral** (usado em cães de guia ou tração).

![[origem-termo-harness.jpg]]

A ideia por trás da palavra é a mesma nos três casos:
- **Sem o arreio/peitoral:** há muita força e energia, mas sem direção, sem acoplamento ao trabalho e sem controle seguro.
- **Com o *harness*:** você **canaliza força bruta em trabalho útil, guiado e seguro**.

É daí que vem **Harness Engineering**: construir toda a estrutura ao redor de um modelo de IA para transformar capacidade bruta de linguagem em engenharia de código confiável.

---

![[evolucao-chat-ao-terminal.jpg]]

## 1. A Mudança de Paradigma: Sair da Janela de Chat
Durante muito tempo, usar IA para código significava copiar trechos de uma janela de chat no navegador e colar no editor. O problema é que, fora do ambiente real, o modelo está **cego**: ele não vê a estrutura do projeto, não lê logs e não sabe se o código realmente compila ou passa nos testes.

| Geração                   | Onde a IA vive         | Como funciona                                                                        | Limite principal                                            |
| :------------------------ | :--------------------- | :----------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| **1. Chat na Web**        | Navegador              | Você descreve o problema, copia a resposta e testa manualmente.                      | Sem acesso ao projeto; feedback lento e manual.             |
| **2. Assistente na IDE**  | Editor de código       | Autocomplete e edições em arquivos abertos.                                          | Ainda centrado no editor; alcance limitado sobre o sistema. |
| **3. Agente no Terminal** | CLI (Linha de comando) | Lê arquivos, edita, roda `git`, executa testes, lê erros e corrige em ciclo fechado. | Exige controle, verificação e limites claros (*Harness*).   |

**Por que o terminal?**
Terminais operam em **texto puro** (`stdin` / `stdout`), que é a língua nativa dos LLMs, e já reúnem as ferramentas reais de verificação da engenharia (`git`, compiladores, test runners, linters, containers).

---

## 2. Crédito aos Pioneiros: Anthropic e Claude Code
É justo reconhecer que a **Anthropic** e o time responsável pelo **Claude Code** praticamente inauguraram e popularizaram o campo moderno de **Harness Engineering** para código.

Principais contribuições que moldaram a área:
1. **Filosofia simples e componível:** No guia *Building Effective Agents* (dezembro de 2024), a Anthropic mostrou que loops simples e ferramentas diretas no terminal superam frameworks rígidos e supercomplexos.
2. **O agente nativo de terminal:** O **Claude Code** provou na prática que um agente com acesso ao shell, arquivos e testes resolve tarefas reais de ponta a ponta sem depender de uma interface gráfica pesada.
3. **A disciplina do Harness:** As pesquisas da Anthropic consolidaram a visão de que $\text{Agente} = \text{Modelo} + \text{Harness}$ — quando um agente falha em tarefas longas, o gargalo muitas vezes está na infraestrutura de contexto, memória entre sessões e verificação, não apenas na "inteligência" do modelo.
4. **Convenções na raiz do projeto:** Padrões populares como arquivos de instrução no repositório (`CLAUDE.md`) ajudaram a inspirar convenções abertas hoje usadas por múltiplos agentes (como `AGENTS.md`).

---

## 3. O Próximo Passo: Open Source e Independência de Fornecedor (Anti-Lock-in)
Reconhecer os pioneiros não significa ficar preso a um único ecossistema. Produtos comerciais naturalmente otimizam para seus próprios modelos, SDKs e nuvens.

Para times de tecnologia, adotar uma abordagem **open source e agnóstica a modelo** traz vantagens estratégicas:
- **Troca livre de modelos:** Modelos evoluem rápido. Separar **inteligência (LLM)** de **infraestrutura (harness)** permite usar o melhor modelo do dia — ou alternar entre APIs na nuvem e **modelos locais / open-weight** (ex.: Ollama, `llama.cpp`) por custo ou privacidade.
- **Transparência ("sem caixa-preta"):** Em um harness aberto, você sabe exatamente qual prompt é enviado, quais ferramentas existem e como suas regras e arquivos são manipulados.
- **Domínio do fluxo de engenharia:** O conhecimento do time (`AGENTS.md`, skills, validações, automações) vira ativo do repositório, não dependência de um fornecedor.

> **Por isso nosso estudo usa o Pi (`pi.dev`):** um harness open source minimalista e extensível que serve como **raio-x** para entender, peça por peça, como qualquer agente de terminal funciona por dentro.

> [!tip] Dica de Palco — Lembretes do Bloco 1
> - **Quebra-gelo de abertura:** Comece traduzindo *"harness"* para o público brasileiro (*arreio* de cavalo / *peitoral* de cão) — quem não conhece o termo entende na hora que é "o que canaliza e guia força bruta".
> - **Justiça histórica:** Cite explicitamente a **Anthropic** e o time do **Claude Code** antes de entrar no viés open source — mostra maturidade técnica e contextualiza de onde veio a disciplina.
> - **Virada anti-lock-in:** Reforce que o foco aqui são **fundamentos**; modelos mudam rápido, mas o domínio sobre o *harness* fica com o time.
