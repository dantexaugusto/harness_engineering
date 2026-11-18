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
