# Módulo 6 — Projeto Capstone: Sistema de IA de Ponta a Ponta

> **Posição no guia:** módulo integrador — conclui o programa após os módulos 1 a 5.
> **Carga estimada:** 3 a 5 semanas de desenvolvimento + escrita.
> **Pré-requisito:** ter concluído (ou estar confortável com) os módulos 2, 3, 4 e 5.

---

## 🎯 Objetivo

Integrar todos os módulos em um sistema de IA de ponta a ponta e documentar as decisões em um artigo técnico (2.000–3.000 palavras), como no curso original.

O capstone não é mais um exercício isolado: é a prova de que você consegue combinar RAG, agentes, infraestrutura de produção e avaliação em um único produto coeso — e que consegue explicar cada decisão por escrito.

---

## Escopo do projeto

O sistema deve combinar as quatro camadas estudadas no programa:

| Camada | Módulo de origem | O que entra no capstone |
|--------|-----------------|-------------------------|
| Recuperação de contexto | Módulo 2 — RAG | Pipeline de ingestão + retrieval vetorial |
| Raciocínio e ação | Módulo 3 — Agentes | Agente com pelo menos duas ferramentas |
| Produto e observabilidade | Módulo 4 — Serviço/API | Endpoint HTTP + logging estruturado |
| Confiança e qualidade | Módulo 5 — Evals | Suíte de avaliação com baseline mensurável |

### Domínios sugeridos

Escolha um domínio onde você já tenha (ou consiga reunir) dados reais — isso torna o projeto mais rico e o artigo mais honesto.

**Opção A — Assistente de documentação técnica própria**
Ingira a documentação de uma ferramenta, biblioteca ou produto que você usa no trabalho. O agente responde perguntas, cita a fonte e sabe quando a informação não está na base.

**Opção B — Agente de pesquisa sobre artigos**
Baixe artigos (PDF, arXiv, papers internos) de um tema de interesse. O agente pesquisa, sintetiza e compara argumentos entre documentos.

**Opção C — Q&A sobre código de repositório**
Use um repositório público (ou privado) como base de conhecimento. O agente explica funções, rastreia dependências e sugere onde um novo recurso se encaixaria.

**Opção D — Domínio livre**
Qualquer domínio onde você tenha dados e uma pergunta real a responder. Documente a escolha com clareza no artigo.

### Restrições de stack

- Linguagem: Python 3.11+
- Modelo: provedor aberto ou local (Ollama, llama.cpp, vLLM, OpenRouter com modelos gratuitos)
- Banco vetorial: qualquer opção gratuita (Chroma, FAISS, Qdrant local, LanceDB)
- Orquestração: LangChain, LlamaIndex, smolagents ou implementação própria — justifique a escolha
- Sem dependência de APIs pagas obrigatórias

---

## Requisitos mínimos

### Do sistema

1. **Ingestão de documentos**
   - Carregamento de pelo menos um formato (PDF, Markdown, texto, código)
   - Chunking com estratégia documentada (tamanho, overlap, separador)
   - Geração e persistência de embeddings

2. **Retrieval**
   - Busca semântica funcionando (top-k configurável)
   - Pelo menos uma técnica além da busca naïve: re-ranking, MMR, busca híbrida, ou filtro por metadados
   - Tratamento explícito do caso "sem resultado relevante"

3. **Agente com ≥ 2 ferramentas**
   - Ferramenta 1: busca na base vetorial (RAG)
   - Ferramenta 2: pelo menos uma ação adicional — busca na web, execução de código, leitura de arquivo, chamada a API externa, calculadora, etc.
   - Loop de raciocínio visível (ReAct ou equivalente)
   - Controle de iterações para evitar loops infinitos

4. **API com logging**
   - Pelo menos um endpoint HTTP funcional (FastAPI, Flask ou equivalente)
   - Logging estruturado em JSON (timestamp, query, ferramenta usada, latência, modelo)
   - Tratamento de erros com resposta HTTP adequada (não apenas 500 genérico)

5. **Suíte de evals com baseline**
   - Conjunto de teste com ≥ 20 exemplos rotulados (query + resposta esperada ou critério de qualidade)
   - Pelo menos duas métricas: uma automática (ROUGE, BERTScore, match exato, LLM-as-judge) e uma baseada em comportamento (ferramenta correta escolhida, citação presente, recusa adequada)
   - Baseline documentado: qual seria a performance de uma abordagem simples (busca por palavras-chave, resposta direta sem RAG)?
   - Script `run_evals.py` reproduzível

### De documentação mínima no repositório

```
projeto-capstone/
├── README.md           # visão geral + como rodar
├── ingest.py           # pipeline de ingestão
├── agent.py            # definição do agente e ferramentas
├── api.py              # servidor HTTP
├── evals/
│   ├── dataset.json    # exemplos de teste
│   └── run_evals.py    # script de avaliação
├── requirements.txt
└── artigo.md           # ou link para post publicado
```

---

## Roteiro do artigo técnico

O artigo deve ter entre 2.000 e 3.000 palavras e seguir as seções abaixo. A contagem alvo por seção é uma orientação — o que importa é que cada seção cumpra seu propósito.

### 1. Problema e contexto (~300 palavras)

**O que escrever:**
- Qual dor ou necessidade motivou o projeto?
- Quem seria o usuário (mesmo que seja você mesma)?
- Por que uma abordagem baseada em RAG + agente é adequada para esse problema?
- Que alternativas mais simples você considerou e por que descartou?

**Armadilha a evitar:** não comece com "a inteligência artificial está revolucionando...". Comece com o problema concreto.

**Exemplo de abertura forte:**
> "Nossa equipe gasta cerca de duas horas por semana respondendo perguntas sobre a API interna que já estão documentadas. Tentamos um bot de busca por palavras-chave, mas a taxa de resposta irrelevante era alta. Este projeto testa se um agente com RAG resolve o problema com qualidade suficiente para ser usado em produção."

---

### 2. Arquitetura e decisões de design (~700 palavras)

**O que escrever:**
- Diagrama ou descrição em prosa do fluxo completo (query → agente → ferramentas → resposta)
- Decisões de chunking: tamanho escolhido, overlap, separador — e por quê
- Modelo de embedding: qual, por quê (velocidade, qualidade, custo, privacidade)
- Modelo de geração: qual, por quê
- Estratégia de retrieval: top-k, threshold de similaridade, re-ranking?
- Design do agente: ReAct? Planner? Por quê essa arquitetura?
- Decisões de infraestrutura: FastAPI vs Flask, banco vetorial escolhido, persistência

**Estrutura sugerida para cada decisão:**
```
Decisão: [o que foi decidido]
Alternativas consideradas: [o que mais você avaliou]
Critério de escolha: [o que pesou mais — velocidade, qualidade, simplicidade, privacidade]
Trade-off aceito: [o que você abre mão com essa escolha]
```

**Por que essa seção é a mais importante do artigo:**
Qualquer desenvolvedor consegue juntar código que funciona. O que diferencia um engenheiro é a capacidade de justificar por que as peças foram montadas dessa forma — e não de outra.

---

### 3. Implementação: RAG, agente, infraestrutura (~800 palavras)

**O que escrever:**
- Detalhes do pipeline de ingestão: como os documentos foram preparados, que problemas apareceram (encoding, PDFs malformados, documentos muito longos)
- Como o retrieval funciona na prática: trecho de código relevante comentado, exemplo de query e resultado
- Definição das ferramentas do agente: assinatura, comportamento, como o modelo aprende a usá-las (via docstring? system prompt? few-shot?)
- Um exemplo de trace completo: query do usuário → pensamento do agente → ferramenta chamada → observação → resposta final
- O que foi mais difícil de implementar e como você resolveu

**Dica para o trace de exemplo:**
Escolha uma query que mostre o agente usando pelo menos duas ferramentas em sequência. Isso demonstra o raciocínio encadeado melhor do que um exemplo simples.

**Dica para o trecho de código:**
Não copie blocos longos. Escolha as 10–15 linhas que capturam a decisão mais importante e comente o porquê de cada linha não óbvia.

---

### 4. Avaliação: como medi qualidade e confiabilidade (~600 palavras)

**O que escrever:**
- Como o conjunto de teste foi construído: de onde vieram os exemplos, quem rotulou, como você garantiu variedade (perguntas fáceis, difíceis, fora do escopo)
- Quais métricas foram usadas e por quê (não apenas "usei BERTScore" — explique o que ela mede no seu contexto)
- Resultado do baseline vs. sistema completo: números reais, mesmo que modestos
- Pelo menos um exemplo de falha interessante: o que o agente errou e por quê você acha que errou
- Como você usaria os evals para guiar a próxima iteração

**Tabela sugerida de resultados:**

| Métrica | Baseline (busca simples) | Sistema completo | Delta |
|---------|--------------------------|------------------|-------|
| Resposta correta (%) | — | — | — |
| Ferramenta correta (%) | — | — | — |
| Citação presente (%) | — | — | — |
| Latência média (s) | — | — | — |

**Sobre honestidade nos resultados:**
Se o sistema não superou o baseline em alguma métrica, diga isso e explique. Resultados honestos com análise são mais valiosos (e críveis) do que resultados inflados sem justificativa.

---

### 5. Aprendizados, limitações e próximos passos (~400 palavras)

**O que escrever:**
- Os três aprendizados mais importantes do projeto (um técnico, um de produto, um de processo)
- Limitações atuais que você não resolveu: o que o sistema não faz bem? Em quais condições ele falha?
- O que você faria diferente se começasse hoje
- Próximos passos concretos: se você continuasse esse projeto por mais um mês, o que priorizaria?

**Formato útil para limitações:**
```
Limitação: [descrição]
Impacto: [quando isso afeta o usuário]
Caminho para resolver: [o que precisaria mudar]
```

**Por que essa seção importa:**
Ela demonstra maturidade técnica. Qualquer sistema tem limitações; engenheiros que as reconhecem publicamente são mais confiáveis do que os que fingem que tudo funciona perfeitamente.

---

## Entrega

### O que publicar

1. **Repositório no GitHub** com código funcionando, README claro e instruções de execução (`pip install -r requirements.txt && python ingest.py && python api.py`)
2. **Artigo publicado** — escolha um dos formatos:
   - `artigo.md` no próprio repositório (mínimo aceitável)
   - Post no Dev.to, Hashnode ou Medium (alcance maior, comentários)
   - Seção dedicada no README do projeto (integrado, fácil de navegar)

### Critério de "funcionando"

O projeto está pronto quando outra pessoa consegue clonar o repositório, seguir o README sem pedir ajuda, e obter respostas do agente em menos de 10 minutos. Peça para alguém tentar — o tempo que leva é o melhor indicador de qualidade da entrega.

---

## ✅ Autoavaliação

Antes de considerar o capstone concluído, responda honestamente:

**Sobre o sistema:**
- O sistema integra os 4 pilares (RAG, agente, API/serviço, evals)?
- A suíte de evals tem pelo menos 20 exemplos e uma baseline documentada?
- A API tem logging estruturado e tratamento de erros?
- O agente usa pelo menos duas ferramentas de forma encadeada?
- Outra pessoa consegue rodar o projeto seguindo o README?

**Sobre o artigo:**
- O artigo explica as decisões com clareza — não apenas o que foi feito, mas por quê?
- Cada escolha técnica (modelo, banco vetorial, estratégia de chunking, arquitetura do agente) está justificada?
- Os resultados de avaliação são reais e honestos?
- Consigo defender cada escolha técnica se alguém questionar?

**Teste de defesa:**
Leia o artigo como se fosse uma revisora técnica cética. Para cada afirmação, pergunte: "Por que essa escolha e não outra?" Se a resposta não estiver no artigo, adicione-a.

---

## Referências e recursos

- [LangChain — RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/) — pipeline de referência
- [LlamaIndex — Building Agents](https://docs.llamaindex.ai/en/stable/use_cases/agents/) — alternativa de orquestração
- [RAGAS](https://docs.ragas.io/) — framework de evals para RAG
- [smolagents](https://huggingface.co/docs/smolagents) — agentes leves da Hugging Face
- [Ollama](https://ollama.com/) — modelos locais sem configuração complexa
- [FastAPI](https://fastapi.tiangolo.com/) — framework de API recomendado
- [Write Good Decisions Docs](https://adr.github.io/) — como documentar decisões de arquitetura (ADR)

---

*Este módulo conclui o guia. O projeto capstone é a evidência tangível do que você aprendeu — vale o esforço de fazê-lo bem.*
