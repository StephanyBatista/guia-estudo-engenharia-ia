# Módulo 3 — Agentes de IA

---

## 🎯 Objetivos de aprendizado

Ao concluir este módulo você será capaz de:

- Explicar o que diferencia um **agente de IA** de uma chamada simples a um LLM
- Implementar um loop de agente mínimo do zero, sem frameworks
- Criar ferramentas personalizadas e conectá-las ao modelo via *function calling*
- Reconhecer quando usar (e quando **não** usar) a abordagem de agentes
- Identificar os padrões e armadilhas descritos pela Anthropic em "Building Effective Agents"

---

## 🧠 Conceitos-chave

### Agente vs. chamada simples a LLM

Uma **chamada simples a um LLM** é estática: você envia um prompt, recebe uma resposta e o processo termina. Um **agente** é dinâmico: o LLM recebe um objetivo, decide *que ações tomar*, executa essas ações, observa os resultados e repete o ciclo até alcançar a meta — ou até perceber que não consegue. A diferença não está no modelo em si, mas na arquitetura ao redor: o agente tem um loop de controle, ferramentas disponíveis e, idealmente, alguma forma de memória. Isso o torna muito mais poderoso para tarefas abertas, mas também muito mais difícil de depurar e de fazer funcionar de forma confiável.

### Loop raciocínio → ação → observação (ReAct)

O padrão **ReAct** (*Reasoning + Acting*), descrito no paper de Yao et al. (2022), é o coração da maioria dos agentes modernos. O LLM alterna entre dois tipos de token: *Thought* (raciocínio interno, em linguagem natural) e *Action* (chamada a uma ferramenta). Após cada ação, o resultado é inserido de volta no contexto como *Observation*, e o modelo raciocina novamente. Esse ciclo continua até o modelo emitir uma resposta final. O benefício é que o raciocínio explícito melhora a qualidade das decisões; a desvantagem é que cada volta do loop consome tokens e latência.

### Tool use / function calling

*Function calling* é o mecanismo que permite ao LLM sinalizar, dentro da resposta, que quer invocar uma ferramenta — e com quais argumentos. O modelo **não executa** a ferramenta diretamente; ele apenas emite um objeto estruturado (JSON) descrevendo a chamada. Quem executa é o seu código. O resultado é então adicionado ao histórico de mensagens e o modelo recebe o contexto atualizado. Modelos como `qwen2.5`, `mistral` e `llama3.1` disponíveis via Ollama suportam function calling nativo; outros podem ser instruídos via prompt a produzir JSON com formato fixo.

### Memória (curto e longo prazo)

**Memória de curto prazo** é simplesmente a janela de contexto: todas as mensagens trocadas na sessão atual ficam disponíveis ao modelo enquanto cabem na janela. **Memória de longo prazo** exige mecanismos externos: um banco de vetores (como no Módulo 2) para recuperar informações relevantes de sessões anteriores, um banco chave-valor para armazenar fatos do usuário, ou até um arquivo de texto que o agente lê e escreve. A escolha depende do caso de uso — muitos agentes reais precisam apenas de memória de curto prazo bem gerenciada, sem qualquer persistência.

### Planejamento e tarefas multi-step

Para tarefas complexas, um agente precisa **decompor o objetivo em subtarefas** e executá-las em sequência (ou em paralelo, em arquiteturas multi-agente). Essa decomposição pode acontecer de duas formas: *implicitamente*, rodada a rodada, onde o modelo decide a próxima ação com base na observação anterior; ou *explicitamente*, quando o modelo gera um plano completo antes de começar a executar. Planos explícitos são mais previsíveis, mas frágeis se o ambiente mudar durante a execução. A Anthropic recomenda preferir arquiteturas simples e incrementar complexidade apenas quando necessário.

### Quando NÃO usar agentes

Agentes introduzem complexidade, latência e custo de debug que muitas vezes não se justificam. **Não use agentes quando:** a tarefa tem um fluxo fixo e bem definido (uma cadeia de prompts simples resolve melhor); quando a confiabilidade é crítica e erros do agente têm consequências sérias; quando o escopo é pequeno o suficiente para caber em um único prompt bem elaborado; ou quando o ambiente não fornece ferramentas reais para o agente interagir. O artigo "Building Effective Agents" da Anthropic é enfático: comece com a solução mais simples possível e adicione agência apenas onde ela agrega valor mensurável.

### Padrões da Anthropic ("Building Effective Agents")

A Anthropic descreve um conjunto de padrões arquiteturais para agentes confiáveis. Os mais importantes são: **agente único com ferramentas** (o padrão mais simples e recomendado como ponto de partida); **pipeline de prompts em cadeia** (cada saída alimenta o próximo prompt, sem loop); **roteador** (o LLM decide qual caminho de execução seguir); e **orquestrador + subagentes** (um LLM central delega subtarefas a agentes especializados). Para cada padrão, há *guardrails* recomendados: verificações humanas em pontos críticos, limites no número de iterações do loop, e preferência por ações reversíveis antes de irreversíveis.

---

## 📚 Recursos

Todos os links abaixo estão com URLs completos em [`recursos.md`](./recursos.md), na seção **Agentes de IA**.

| Recurso | Por que ler |
|---|---|
| **Anthropic — "Building Effective Agents"** | Ponto de partida obrigatório: padrões concretos escritos por quem constrói Claude |
| **Yao et al. (2022) — Paper ReAct** | Artigo curto que formaliza o loop raciocínio-ação-observação |
| **Hugging Face — AI Agents Course + smolagents** | Curso gratuito e atualizado com exemplos práticos de tool use |
| **DeepLearning.AI — "Functions, Tools and Agents"** | Curso curto (~2h) com foco em function calling e agentes com LangChain |

---

## ⌨️ Mão na massa

### Visão geral dos exercícios

1. Implementar um **loop de agente mínimo do zero** (sem framework): o LLM decide qual ferramenta chamar, o Python executa, o resultado volta ao contexto.
2. Criar **duas ferramentas**: uma calculadora e uma busca em arquivo de texto (reutilizando a ideia de RAG do Módulo 2 de forma simplificada).
3. Dar ao agente uma **tarefa multi-step** que exija usar as duas ferramentas em sequência.
4. Adicionar **memória simples** (histórico acumulado) e observar o comportamento.

### Pré-requisitos

```bash
# Certifique-se que o Ollama está rodando e que você tem um modelo com function calling
ollama pull qwen2.5:7b   # bom suporte a tool use, roda em ~8 GB de RAM
# Alternativa menor (~4 GB):
# ollama pull qwen2.5:3b

pip install ollama
```

### Exercício 1 e 2 — Loop mínimo + duas ferramentas

O código abaixo implementa um agente completo sem nenhum framework. Leia com atenção: cada parte do loop está comentada.

```python
# agent.py
import json
import math
import ollama

# ──────────────────────────────────────────────
# 1. DEFINIÇÃO DAS FERRAMENTAS
# ──────────────────────────────────────────────

# Ferramenta 1: calculadora simples
def calcular(expressao: str) -> str:
    """Avalia uma expressão matemática Python e retorna o resultado."""
    try:
        # Restringe o namespace para evitar código arbitrário
        resultado = eval(expressao, {"__builtins__": {}}, {"math": math})
        return str(resultado)
    except Exception as e:
        return f"Erro ao calcular '{expressao}': {e}"


# Ferramenta 2: busca em arquivo de texto (simulando RAG simples)
CONHECIMENTO = """
salário_dev_junior=4500
salário_dev_pleno=8000
salário_dev_senior=14000
bônus_anual=10%
horas_por_semana=40
férias_dias=30
"""

def buscar_dado(chave: str) -> str:
    """Busca o valor de uma chave no arquivo de conhecimento interno."""
    chave = chave.strip().lower().replace(" ", "_")
    for linha in CONHECIMENTO.strip().splitlines():
        if "=" in linha:
            k, v = linha.split("=", 1)
            if k.strip().lower() == chave:
                return f"{chave} = {v.strip()}"
    return f"Chave '{chave}' não encontrada no arquivo de conhecimento."


# Mapa nome → função (usado pelo dispatcher)
FERRAMENTAS_DISPONIVEIS = {
    "calcular": calcular,
    "buscar_dado": buscar_dado,
}

# ──────────────────────────────────────────────
# 2. SCHEMA DAS FERRAMENTAS (formato Ollama/OpenAI)
# ──────────────────────────────────────────────

TOOLS_SCHEMA = [
    {
        "type": "function",
        "function": {
            "name": "calcular",
            "description": (
                "Avalia uma expressão matemática Python pura e retorna o resultado numérico. "
                "Use apenas operadores matemáticos, números e funções do módulo math. "
                "Exemplos válidos: '2 + 2', '4500 * 1.10', 'math.sqrt(144)'."
            ),
            "parameters": {
                "type": "object",
                "properties": {
                    "expressao": {
                        "type": "string",
                        "description": "Expressão matemática a ser avaliada",
                    }
                },
                "required": ["expressao"],
            },
        },
    },
    {
        "type": "function",
        "function": {
            "name": "buscar_dado",
            "description": (
                "Busca um dado específico no arquivo de conhecimento interno da empresa. "
                "Use para obter salários, bônus, horas de trabalho e outros dados cadastrais. "
                "Exemplos de chaves válidas: 'salário_dev_pleno', 'bônus_anual'."
            ),
            "parameters": {
                "type": "object",
                "properties": {
                    "chave": {
                        "type": "string",
                        "description": "Nome do dado a buscar (ex: 'salário_dev_senior')",
                    }
                },
                "required": ["chave"],
            },
        },
    },
]

# ──────────────────────────────────────────────
# 3. DISPATCHER — executa a ferramenta escolhida pelo modelo
# ──────────────────────────────────────────────

def executar_ferramenta(nome: str, argumentos: dict) -> str:
    """Despacha a chamada de ferramenta e retorna o resultado como string."""
    if nome not in FERRAMENTAS_DISPONIVEIS:
        return f"Ferramenta '{nome}' não encontrada."
    func = FERRAMENTAS_DISPONIVEIS[nome]
    return func(**argumentos)


# ──────────────────────────────────────────────
# 4. LOOP DO AGENTE
# ──────────────────────────────────────────────

def rodar_agente(tarefa: str, modelo: str = "qwen2.5:7b", max_iteracoes: int = 10) -> str:
    """
    Loop principal do agente ReAct:
      1. Envia mensagens ao LLM com as ferramentas disponíveis
      2. Se o modelo pedir uma ferramenta, executa e adiciona o resultado ao histórico
      3. Repete até o modelo responder sem pedir ferramentas (resposta final)
    """
    print(f"\n{'='*60}")
    print(f"TAREFA: {tarefa}")
    print(f"{'='*60}\n")

    # Histórico de mensagens — é a "memória de curto prazo" do agente
    historico = [
        {
            "role": "system",
            "content": (
                "Você é um assistente analítico. Para responder perguntas que exigem dados "
                "ou cálculos, use as ferramentas disponíveis. Raciocine passo a passo. "
                "Só responda sem usar ferramentas quando já tiver todos os dados necessários."
            ),
        },
        {"role": "user", "content": tarefa},
    ]

    for iteracao in range(1, max_iteracoes + 1):
        print(f"--- Iteração {iteracao} ---")

        # Chamada ao LLM
        resposta = ollama.chat(
            model=modelo,
            messages=historico,
            tools=TOOLS_SCHEMA,
        )

        mensagem = resposta["message"]

        # O modelo quer chamar alguma ferramenta?
        if mensagem.get("tool_calls"):
            # Adiciona a mensagem do assistente (com as tool_calls) ao histórico
            historico.append(mensagem)

            # Processa cada chamada de ferramenta solicitada
            for chamada in mensagem["tool_calls"]:
                nome_ferramenta = chamada["function"]["name"]
                argumentos = chamada["function"]["arguments"]

                # Se os argumentos vierem como string JSON, converte para dict
                if isinstance(argumentos, str):
                    argumentos = json.loads(argumentos)

                print(f"  → Ferramenta: {nome_ferramenta}({argumentos})")
                resultado = executar_ferramenta(nome_ferramenta, argumentos)
                print(f"  ← Resultado:  {resultado}")

                # Adiciona o resultado da ferramenta ao histórico
                historico.append({
                    "role": "tool",
                    "content": resultado,
                })

        else:
            # O modelo respondeu sem pedir ferramenta — essa é a resposta final
            resposta_final = mensagem["content"]
            print(f"\nRESPOSTA FINAL:\n{resposta_final}\n")
            return resposta_final

    return "Limite de iterações atingido sem resposta final."


# ──────────────────────────────────────────────
# 5. EXERCÍCIO 3 — tarefa multi-step
# ──────────────────────────────────────────────

if __name__ == "__main__":
    # Tarefa que exige: buscar dado → calcular → buscar outro dado → calcular novamente
    tarefa_multi_step = (
        "Qual seria o salário anual de um dev pleno considerando o bônus anual da empresa? "
        "Mostre o cálculo passo a passo."
    )
    rodar_agente(tarefa_multi_step)
```

Execute com:

```bash
python agent.py
```

Saída esperada (resumida):

```
============================================================
TAREFA: Qual seria o salário anual de um dev pleno considerando o bônus anual da empresa? ...
============================================================

--- Iteração 1 ---
  → Ferramenta: buscar_dado({'chave': 'salário_dev_pleno'})
  ← Resultado:  salário_dev_pleno = 8000

--- Iteração 2 ---
  → Ferramenta: buscar_dado({'chave': 'bônus_anual'})
  ← Resultado:  bônus_anual = 10%

--- Iteração 3 ---
  → Ferramenta: calcular({'expressao': '8000 * 12 * 1.10'})
  ← Resultado:  105600.0

RESPOSTA FINAL:
O salário anual de um dev pleno com o bônus de 10% é R$ 105.600,00.
Cálculo: R$ 8.000/mês × 12 meses × 1,10 = R$ 105.600,00.
```

### Exercício 4 — Observando o efeito da memória

O `historico` já é memória de curto prazo dentro de uma sessão. Para observar a diferença, tente duas versões:

**Versão A — sem memória entre perguntas (novo histórico a cada chamada):**

```python
# Chama rodar_agente duas vezes; cada chamada começa do zero
rodar_agente("Qual é o salário do dev senior?")
rodar_agente("E qual seria o salário anual dele com bônus?")  # modelo "esqueceu" o valor
```

**Versão B — com memória acumulada entre perguntas:**

```python
def rodar_agente_com_memoria(tarefas: list[str], modelo: str = "qwen2.5:7b") -> None:
    """Mantém o histórico entre perguntas diferentes na mesma sessão."""
    historico = [
        {
            "role": "system",
            "content": (
                "Você é um assistente analítico com acesso a dados da empresa. "
                "Use as ferramentas disponíveis para responder perguntas que exigem dados ou cálculos."
            ),
        }
    ]

    for tarefa in tarefas:
        print(f"\nUSUÁRIO: {tarefa}")
        historico.append({"role": "user", "content": tarefa})

        while True:
            resposta = ollama.chat(
                model=modelo,
                messages=historico,
                tools=TOOLS_SCHEMA,
            )
            mensagem = resposta["message"]

            if mensagem.get("tool_calls"):
                historico.append(mensagem)
                for chamada in mensagem["tool_calls"]:
                    nome = chamada["function"]["name"]
                    args = chamada["function"]["arguments"]
                    if isinstance(args, str):
                        args = json.loads(args)
                    resultado = executar_ferramenta(nome, args)
                    historico.append({"role": "tool", "content": resultado})
            else:
                historico.append(mensagem)
                print(f"AGENTE: {mensagem['content']}")
                break


# Teste: a segunda pergunta não precisa rebuscar o salário porque ele já está no contexto
rodar_agente_com_memoria([
    "Qual é o salário do dev senior?",
    "E qual seria o salário anual dele com bônus? Use o valor que você já sabe.",
])
```

**O que observar:** na Versão B, o modelo não chama `buscar_dado` na segunda pergunta porque o salário já está no histórico. Na Versão A, ele precisa buscar novamente. Isso ilustra que memória de curto prazo (contexto acumulado) reduz chamadas de ferramentas e melhora a coerência da conversa.

### Dica de depuração

Se o modelo não chamar as ferramentas corretamente, inspecione o histórico:

```python
import pprint
pprint.pprint(historico)
```

Isso revela exatamente o que o modelo "viu" em cada iteração e ajuda a identificar se o schema das ferramentas está claro o suficiente.

---

## 🏁 Marco

Você concluiu este módulo quando tiver **um agente que usa as duas ferramentas para resolver uma tarefa de múltiplos passos** — ou seja, a execução percorre pelo menos duas iterações do loop, cada uma invocando uma ferramenta diferente, antes de produzir a resposta final.

Critérios de verificação:

- [ ] O loop termina sozinho (o modelo emite resposta final sem tool_calls)
- [ ] O histórico mostra alternância entre mensagens `assistant`, `tool` e de volta a `assistant`
- [ ] A resposta final usa os dados das duas ferramentas de forma correta
- [ ] O agente não entra em loop infinito (o limite `max_iteracoes` foi testado)

---

## ✅ Autoavaliação

Responda sem consultar o material:

1. **Sei explicar o loop raciocínio-ação?** Consigo descrever as três fases do ReAct (Thought, Action, Observation) e por que elas se repetem?

2. **Sei quando um agente é (e não é) a escolha certa?** Consigo dar dois exemplos de tarefas que *não* precisam de agente e explicar por quê?

3. **Meu agente encadeia ferramentas corretamente?** O modelo usa o resultado da primeira ferramenta para decidir os argumentos da segunda, ou as chamadas são independentes?

4. **Entendi o papel do histórico de mensagens?** Consigo explicar o que acontece se eu omitir a mensagem `tool` do histórico após executar uma ferramenta?

5. **Consigo identificar os padrões da Anthropic?** Sei nomear pelo menos três padrões arquiteturais descritos em "Building Effective Agents" e quando aplicar cada um?

Se alguma resposta ficou travada, releia a seção de conceitos correspondente antes de avançar para o próximo módulo.
