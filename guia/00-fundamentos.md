# Módulo 00 — Fundamentos de LLMs

> **Pré-requisitos:** Python 3.10+, familiaridade com linha de comando, nenhum conhecimento prévio de IA necessário.

---

## 🎯 Objetivos de aprendizado

Ao terminar este módulo você vai conseguir:

- Explicar o que é um LLM (Large Language Model) e por que ele funciona do jeito que funciona.
- Descrever os blocos fundamentais: tokens, embeddings, atenção e parâmetros de geração.
- Fazer a primeira chamada a um modelo local via código Python.
- Interpretar e ajustar os parâmetros `temperature`, `top_p` e `max_tokens` com intenção.

---

## 🧠 Conceitos-chave

### O que é um LLM, afinal?

Um Large Language Model é uma rede neural treinada para prever o próximo token dado um contexto. Simples assim — e surpreendentemente poderoso. Durante o pré-treinamento, o modelo vê enormes volumes de texto e aprende padrões estatísticos da linguagem: gramática, fatos, raciocínio, estilos. Depois, ajustes finos (fine-tuning) e feedback humano (RLHF) moldam o comportamento do modelo para que ele siga instruções e seja menos propenso a respostas prejudiciais. O resultado é o que chamamos de modelo de chat — como o Llama, Mistral, Phi ou Gemma que você vai usar no Ollama.

### Tokens e tokenização

O modelo não processa letras nem palavras inteiras: ele processa **tokens**, pedaços de texto produzidos por um algoritmo como BPE (Byte-Pair Encoding). Em português, "computador" pode ser um único token; "tokenização" pode virar dois ou três. Textos em inglês costumam ser tokenizados com mais eficiência do que em português ou línguas com caracteres especiais. Isso tem impacto prático: o custo de uma chamada à API é medido em tokens, e o limite de contexto também. Uma regra de bolso: 1 token ≈ 0,75 palavras em inglês; em português, espere ~0,6 palavras por token.

### Embeddings

Antes de processar texto, o modelo converte cada token num vetor de números — o **embedding**. Tokens semanticamente parecidos ficam próximos nesse espaço vetorial. É por isso que o modelo "sabe" que "carro" e "automóvel" têm significados parecidos sem nunca ter aprendido uma regra explícita. Embeddings também são usados fora do modelo de geração: você pode extraí-los para busca semântica, clustering e retrieval (temas de módulos futuros).

### Prompting: system, user e few-shot

A interface principal com um LLM é o **prompt**. No formato de chat, ele tem pelo menos duas partes:

- **System message**: instrução de comportamento geral ("você é um assistente especializado em Python que responde em pt-BR").
- **User message**: a mensagem da usuária na conversa.

**Few-shot prompting** é a técnica de incluir exemplos no prompt para guiar o formato ou estilo da resposta. Em vez de explicar o que quer, você mostra:

```
User: Classifique o sentimento: "O produto chegou quebrado."
Assistant: Negativo

User: Classifique o sentimento: "Adorei a entrega rápida!"
Assistant: Positivo

User: Classifique o sentimento: "O manual poderia ser mais claro."
Assistant:
```

O modelo vai inferir o padrão e completar com "Neutro" ou "Negativo". Few-shot é especialmente útil para tarefas de classificação, extração e formatação estruturada.

### Parâmetros de geração: temperature, top_p, max_tokens

Esses parâmetros controlam **como** o modelo escolhe o próximo token a cada passo:

- **`temperature`**: controla a "criatividade". Valores baixos (0–0.3) tornam a saída mais determinística e focada — bom para código, extração de dados. Valores altos (0.8–1.5) aumentam a diversidade e surpresa — bom para brainstorming e ficção. Com `temperature=0` o modelo sempre escolhe o token mais provável.
- **`top_p`** (nucleus sampling): em vez de considerar todos os tokens possíveis, o modelo considera apenas o conjunto mínimo cuja probabilidade acumulada atinge `top_p`. `top_p=0.9` significa "considere apenas os tokens que juntos somam 90% da probabilidade". Costuma ser usado junto com temperature — escolha um ou outro para ajustar, raramente os dois ao mesmo tempo.
- **`max_tokens`**: número máximo de tokens que o modelo vai gerar na resposta. Não confunda com o tamanho do prompt — este parâmetro controla apenas a saída. Se a resposta for cortada, aumente esse valor.

### Janela de contexto

Todo LLM tem um limite de quantos tokens ele consegue "ver" de uma vez — a **janela de contexto** (context window). Ela inclui o system prompt, o histórico de conversa e a resposta sendo gerada. Modelos modernos têm janelas de 8k a 128k tokens. Na prática: se você exceder o limite, o modelo vai truncar o início da conversa e perder informações. Planejar o uso da janela de contexto é uma das habilidades centrais da engenharia de IA — você vai ver isso com muito mais detalhe nos módulos de RAG e agentes.

### Alucinação e limitações

LLMs **alucinam**: produzem texto fluente, convincente e factualmente errado. Isso acontece porque o modelo aprende padrões de texto, não "verdades". Ele não tem acesso à internet (a menos que ferramentas externas sejam conectadas), não sabe o que aconteceu depois do seu corte de treinamento, e pode inventar citações, nomes e datas com total confiança. Como engenheira de IA, seu trabalho é projetar sistemas que minimizem alucinações — por exemplo, fornecendo o contexto relevante no prompt (RAG) ou pedindo que o modelo cite fontes e admita incerteza.

### "Usar um chatbot" vs. "engenharia de IA"

Usar o ChatGPT ou o Claude.ai é conveniente, mas é uma caixa preta. **Engenharia de IA** significa controlar o modelo via API ou biblioteca: escolher o modelo certo, formatar prompts programaticamente, inspecionar parâmetros, tratar erros, encadear chamadas, conectar ferramentas externas e medir qualidade. É a diferença entre usar um carro e saber trocar o óleo — para construir aplicações confiáveis, você precisa entender o motor.

---

## 📚 Recursos

Os recursos abaixo cobrem os fundamentos deste módulo. A lista completa de materiais do guia (com links, idioma e estimativa de tempo) está em [`recursos.md`](./recursos.md).

| Recurso | Formato | Idioma |
|---|---|---|
| **Andrej Karpathy — "Intro to Large Language Models"** (YouTube, 1h) | Vídeo | Inglês |
| **3Blue1Brown — série "Neural Networks"** (capítulos sobre transformers) | Vídeo | Inglês (legendas PT) |
| **"The Illustrated Transformer"** — Jay Alammar (jalammar.github.io) | Artigo visual | Inglês |
| **Hugging Face LLM Course** (huggingface.co/learn/llm-course) | Curso interativo | Inglês/Português |
| **Documentação do Ollama** (ollama.com/docs) | Docs | Inglês |

**Sugestão de ordem:** assista ao Karpathy primeiro para ter a visão geral; depois leia "The Illustrated Transformer" para entender a arquitetura visualmente; use o HuggingFace Course para aprofundar tokenização e embeddings.

---

## ⌨️ Mão na massa

### 1. Instalar o Ollama e baixar um modelo

Instale o Ollama a partir de [ollama.com](https://ollama.com). Após a instalação, baixe um modelo compacto e capaz:

```bash
# Baixa o Llama 3.2 de 3B parâmetros (~2 GB)
ollama pull llama3.2

# Confirma que o modelo está disponível
ollama list
```

Após o download, teste pelo terminal:

```bash
ollama run llama3.2 "Olá! Qual é a capital do Brasil?"
```

### 2. Primeira chamada via Python

O Ollama expõe uma API HTTP local na porta 11434. Você pode usar a biblioteca `ollama` (wrapper oficial) ou fazer chamadas diretas com `requests`. Abaixo, os dois jeitos:

**Opção A — usando a biblioteca `ollama`:**

```bash
pip install ollama
```

```python
import ollama

# Chamada simples ao modelo local
resposta = ollama.chat(
    model="llama3.2",
    messages=[
        {
            "role": "system",
            "content": "Você é um assistente técnico que responde em português."
        },
        {
            "role": "user",
            "content": "Explique o conceito de token em LLMs em até 3 frases."
        }
    ]
)

print(resposta["message"]["content"])
```

**Opção B — usando `requests` diretamente (sem instalar nada além do Ollama):**

```python
import requests
import json

url = "http://localhost:11434/api/chat"

payload = {
    "model": "llama3.2",
    "messages": [
        {
            "role": "system",
            "content": "Você é um assistente técnico que responde em português."
        },
        {
            "role": "user",
            "content": "Explique o conceito de token em LLMs em até 3 frases."
        }
    ],
    "stream": False  # False = recebe a resposta completa de uma vez
}

response = requests.post(url, json=payload)
data = response.json()

print(data["message"]["content"])
```

> **Dica:** se o Ollama não estiver rodando, inicie com `ollama serve` em outro terminal.

### 3. Variar a `temperature` e comparar saídas

Este exercício torna concreto o efeito da temperatura. Use o mesmo prompt e compare as respostas:

```python
import ollama

prompt_usuario = "Escreva uma frase criativa sobre programação."

for temperatura in [0.0, 0.7, 1.5]:
    print(f"\n{'='*50}")
    print(f"Temperature: {temperatura}")
    print('='*50)

    resposta = ollama.chat(
        model="llama3.2",
        messages=[
            {"role": "user", "content": prompt_usuario}
        ],
        options={
            "temperature": temperatura
        }
    )

    print(resposta["message"]["content"])
```

Execute algumas vezes com cada valor e observe:
- `temperature=0.0`: respostas quase idênticas entre execuções, mais "seguras".
- `temperature=0.7`: equilíbrio entre coerência e variedade (padrão da maioria dos modelos).
- `temperature=1.5`: respostas mais criativas — e às vezes mais erráticas ou incoerentes.

### 4. Contar tokens e observar `max_tokens`

Para contar tokens localmente sem chamar uma API paga, use a biblioteca `tiktoken` (tokenizador da OpenAI, compatível com muitos modelos) ou `transformers`:

```bash
pip install tiktoken
```

```python
import tiktoken

# cl100k_base é o encoding do GPT-4 — use como referência para estimar
enc = tiktoken.get_encoding("cl100k_base")

textos = [
    "Olá, mundo!",
    "Tokenização é o processo de dividir texto em unidades menores chamadas tokens.",
    "A engenharia de inteligência artificial envolve projetar sistemas que utilizam modelos de linguagem para resolver problemas práticos de forma confiável e escalável.",
]

for texto in textos:
    tokens = enc.encode(texto)
    print(f"Texto: {texto!r}")
    print(f"  → {len(tokens)} tokens")
    print(f"  → Tokens: {tokens[:10]}{'...' if len(tokens) > 10 else ''}\n")
```

Agora teste o efeito de `max_tokens` numa chamada real:

```python
import ollama

pergunta = "Explique como funciona um transformer passo a passo, com detalhes técnicos."

for limite in [20, 100, 500]:
    print(f"\n--- max_tokens={limite} ---")
    resposta = ollama.chat(
        model="llama3.2",
        messages=[{"role": "user", "content": pergunta}],
        options={"num_predict": limite}  # no Ollama, max_tokens chama-se num_predict
    )
    print(resposta["message"]["content"])
```

> **Nota:** no Ollama, o parâmetro equivalente a `max_tokens` chama-se `num_predict`. Na API HTTP direta, ele vai dentro de `"options": {"num_predict": 100}`.

---

## 🏁 Marco

Crie o arquivo `marco_00.py` com o script abaixo. Ele deve rodar sem erros e demonstrar que você domina os conceitos deste módulo:

```python
"""
Marco do Módulo 00 — Fundamentos de LLMs
=========================================
Script de conversa com modelo local via Ollama.
Cada parâmetro está comentado explicando seu papel.
"""

import ollama

def conversar_com_modelo(
    pergunta: str,
    system_prompt: str = "Você é um assistente técnico que responde em português de forma clara e objetiva.",
    model: str = "llama3.2",
    temperature: float = 0.7,
    max_tokens: int = 300,
    top_p: float = 0.9,
) -> str:
    """
    Envia uma pergunta ao modelo local e retorna a resposta.

    Parâmetros:
    -----------
    pergunta      : a mensagem do usuário para o modelo.
    system_prompt : instrução de comportamento geral do assistente.
                    Define personalidade, idioma e restrições.
    model         : nome do modelo instalado no Ollama.
    temperature   : controla a aleatoriedade da resposta.
                    0 = determinístico; 1.5 = muito criativo.
    max_tokens    : limite de tokens na resposta gerada.
                    No Ollama esse parâmetro chama-se num_predict.
    top_p         : nucleus sampling — considera só os tokens
                    cuja probabilidade acumulada soma top_p.
                    0.9 = 90% dos tokens mais prováveis.
    """
    resposta = ollama.chat(
        model=model,
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user",   "content": pergunta},
        ],
        options={
            "temperature": temperature,
            "num_predict": max_tokens,  # equivalente a max_tokens
            "top_p":       top_p,
        },
    )
    return resposta["message"]["content"]


def main():
    print("=== Marco 00: Conversa com Modelo Local ===\n")

    # Pergunta 1: conceito técnico
    pergunta1 = "O que é tokenização e por que ela importa para LLMs?"
    print(f"Pergunta: {pergunta1}")
    print(f"Resposta:\n{conversar_com_modelo(pergunta1)}\n")

    # Pergunta 2: com temperature baixa (mais focado)
    pergunta2 = "Liste 3 limitações conhecidas de LLMs."
    print(f"Pergunta (temperature=0.1): {pergunta2}")
    print(f"Resposta:\n{conversar_com_modelo(pergunta2, temperature=0.1)}\n")

    # Pergunta 3: com temperature alta (mais criativo)
    pergunta3 = "Crie uma metáfora para explicar embeddings a alguém leigo."
    print(f"Pergunta (temperature=1.2): {pergunta3}")
    print(f"Resposta:\n{conversar_com_modelo(pergunta3, temperature=1.2)}\n")

    print("=== Marco concluído com sucesso! ===")


if __name__ == "__main__":
    main()
```

**Como executar:**

```bash
# Certifique-se que o Ollama está rodando
ollama serve &

# Execute o marco
python marco_00.py
```

A saída esperada é três respostas coerentes, demonstrando que você controla o modelo programaticamente com diferentes configurações de parâmetros.

---

## ✅ Autoavaliação

Antes de seguir para o próximo módulo, confirme que você consegue responder "sim" a cada item:

- [ ] Consigo explicar o que é um **token** e por que o modelo não processa palavras inteiras.
- [ ] Consigo explicar o que é um **embedding** e qual a relação com similaridade semântica.
- [ ] Entendo o que **temperature** faz e sei quando usar valores altos vs. baixos.
- [ ] Sei o que é **janela de contexto** e quais problemas acontecem quando ela é ultrapassada.
- [ ] Consigo explicar pelo menos duas razões pelas quais **LLMs alucinam**.
- [ ] Entendo a diferença entre **usar um chatbot** e **chamar um modelo via API**.
- [ ] Rodei o `marco_00.py` com sucesso e vi as três respostas impressas no terminal.
- [ ] Modifiquei a `temperature` e observei diferença real nas respostas.

Se ficou algum "não" na lista, volte à seção de conceitos ou aos recursos indicados antes de avançar. O próximo módulo assume esses fundamentos como ponto de partida.

---

> **Próximo módulo:** `01-prompt-engineering.md` — técnicas avançadas de prompting, chain-of-thought, structured output e avaliação de prompts.
