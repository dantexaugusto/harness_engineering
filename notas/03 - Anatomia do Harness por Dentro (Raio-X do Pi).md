# 03 — Anatomia do Harness por Dentro (Raio-X do Pi)

Para entender como um agente de código no terminal funciona sem depender de "mágica" ou caixas-pretas, vamos abrir o capô do **Pi** (`pi-mono`) e olhar suas camadas internas.

![[arquitetura-pi-harness.svg]]

---

## 1. `pi-ai` — O Suporte Universal do Motor (Anti-Lock-in)

Em vez de escrever código acoplado a um único fornecedor, o Pi isola a comunicação com LLMs em um pacote dedicado (`pi-ai`):

- **Normalização por protocolo:** Em vez de manter dezenas de SDKs pesados, o `pi-ai` unifica a conversa em torno dos principais protocolos padrão de mercado (como OpenAI Completions/Responses, Anthropic Messages e Google Generative AI).
- **Nuvem ou Local sem mudar o fluxo:** Funciona com APIs comerciais, roteadores (como OpenRouter) e **modelos locais / *open-weight*** rodando via `Ollama` ou `llama.cpp`.
- **Troca em tempo real (`/model`):** Na metáfora do carro, você troca o motor no meio da viagem — usa um modelo leve e barato para explorar arquivos e alterna para um modelo mais forte para uma refatoração difícil.

---

## 2. `pi-agent-core` — O Loop Agêntico e a Gestão de Contexto

O coração do harness não é a IA — é um **loop de controle determinístico** que orquestra o modelo:

1. **Montagem do prompt:** Junta o *system prompt* enxuto, as regras do projeto e o pedido do usuário.
2. **Chamada ao modelo:** O LLM não "executa" nada sozinho; ele apenas **responde pedindo uma ferramenta** (ou entregando a resposta final).
3. **Execução no ambiente real:** O harness executa a ferramenta solicitada no seu diretório/terminal e captura a saída real (`stdout`, `stderr`, conteúdo de arquivo).
4. **Devolução e nova iteração:** A saída volta para o modelo, que lê o resultado (ex.: um teste que falhou) e decide o próximo passo até resolver o problema.

### Dois detalhes críticos de engenharia aqui:
- **System Prompt < 1.000 tokens:** Enquanto harnesses pesados consomem milhares de tokens só explicando regras internas e dezenas de ferramentas, o Pi mantém o prompt base curtíssimo, deixando a janela de contexto livre para o que importa: **seu código**.
- **Compactação automática de contexto (*Compaction*):** Quando uma sessão fica longa e se aproxima do limite de tokens, o harness resume o histórico antigo de forma controlada para o agente não "esquecer" o objetivo nem degradar a qualidade.

---

## 3. O Núcleo Mínimo: As 4 Ferramentas Fundamentais

Todo o trabalho de código no Pi nasce de apenas **quatro ferramentas nativas**:

| Ferramenta | O que faz | Por que importa na prática |
| :--- | :--- | :--- |
| **`read`** | Lê arquivos do projeto | Permite inspecionar código, logs e configurações sob demanda, sem carregar o repositório inteiro na memória. |
| **`write`** | Cria novos arquivos | Útil para gerar módulos, testes ou documentação do zero. |
| **`edit`** | Faz edições pontuais/cirúrgicas | Evita reescrever um arquivo de 500 linhas inteiro para mudar 3 linhas — economiza tokens e reduz erros. |
| **`bash`** | Executa comandos no terminal | É o "superpoder" Unix: com `bash`, o agente já usa `git`, `rg`/`grep`, testes (`pytest`, `npm test`), linters e builds sem precisar de uma ferramenta separada para cada comando. |

> **Lição de minimalismo:** Quanto menos ferramentas redundantes o modelo precisa escolher, menor o desperdício de tokens e menor a chance de confusão. Tudo o que já existe bem no terminal entra via `bash`.

---

## 4. O que Fica Propositalmente Fora do Núcleo?

No Pi padrão, não vêm embutidos na força:
- Pop-ups rígidos de permissão;
- Subagentes pré-fabricados;
- Modos engessados de planejamento;
- Integrações pesadas (como MCP ou busca web nativa).

Não porque essas coisas não sejam úteis — **mas porque pertencem à camada de extensibilidade** (nosso próximo bloco), onde cada time acopla apenas o que o terreno exige.

---

## 5. Ecossistema Aberto Irmão: LangChain, LangGraph e `deepagents`

![[langgraph-deepagents-vs-pi.svg]]

O Pi não está sozinho nessa filosofia. No ecossistema open source (licença MIT, disponível em **Python** e **JavaScript/TypeScript**), a família **LangChain / LangGraph / `deepagents`** segue o mesmo princípio de independência de fornecedor e controle programático sobre o harness — dividida pelo próprio time em **três camadas**:

1. **LangChain (*Framework*):** abstrações fundamentais, integração com múltiplos modelos, blocos padrão de loop e *middleware*.
2. **LangGraph (*Runtime*):** motor de execução de baixo nível baseado em grafos com estado (*stateful*), *checkpointing* durável, *streaming* e pausas nativas de *human-in-the-loop* para inspecionar ou aprovar chamadas de ferramenta.
3. **`deepagents` (*Harness Oficial*):** o harness oficial "baterias inclusas" do time da LangChain construído sobre o LangGraph (com direito ao companheiro de terminal **`dcode`** / Deep Agents Code). Já traz de fábrica:
   - **Planejamento estruturado (`write_todos`):** força o agente a quebrar tarefas longas em checklist antes de sair editando.
   - **Filesystem virtual / plugável:** descarrega saídas grandes e arquivos fora da memória curta do prompt.
   - **Delegação para subagentes:** cria subagentes com janela de contexto limpa e retorna apenas a síntese.
   - **Compressão de contexto, memória persistente (`/memories/`) e *Skills*.**

| Abordagem Open Source | Ponto Forte | Quando Brilha |
| :--- | :--- | :--- |
| **Pi (`pi-mono`)** | **Minimalismo radical:** 4 ferramentas + *system prompt* `< 1.000 tokens`. | Aprender os fundamentos (**raio-x**), manter o controle enxuto e acoplar extensões TS sob medida. |
| **LangGraph + `deepagents`** | **Runtime em grafos + baterias inclusas:** *checkpointing* durável, `write_todos`, filesystem virtual e Python/TS. | Projetos que querem um runtime de grafos persistentes ou um harness completo já estruturado para tarefas longas. |

> [!tip] Dica de Palco — Lembretes do Bloco 3
> - **Por que abrir o capô do Pi?** Destaque o contraste com ferramentas "caixa-preta": no Pi você enxerga **4 ferramentas** (`read`, `write`, `edit`, `bash`) e um *system prompt* **menor que 1.000 tokens**.
> - **Conexão anti-lock-in:** Aponte para o `pi-ai` como o "suporte universal de motor" — o mesmo harness roda na nuvem ou em modelos locais (`Ollama`, `llama.cpp`) trocando com `/model`.
> - **Amplie a visão open source:** Cite **LangChain / LangGraph** (runtime com estado e *checkpointing*) e o **`deepagents`** (harness oficial deles, com `write_todos`, filesystem virtual e CLI `dcode`) para mostrar que o movimento aberto de *Harness Engineering* tem opções maduras tanto pelo caminho minimalista (Pi) quanto pelo caminho de runtime em grafos (LangGraph).
