# Biblioteca de Recursos

Recursos organizados por tema para acompanhar o guia de estudo. Prioridade total para materiais gratuitos; os livros pagos aparecem ao final como opcionais para quem quiser se aprofundar.

---

## Fundamentos de LLMs

### Grátis

- **3Blue1Brown — "Neural Networks" (série YouTube)**
  Série visual e matemática que explica redes neurais do zero, incluindo um episódio dedicado a Transformers. Ideal para fixar intuição geométrica antes de partir para código.
  URL: https://www.youtube.com/playlist?list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi

- **3Blue1Brown — "But what is a GPT?" e "Attention in transformers" (YouTube)**
  Dois vídeos standalone que mergulham diretamente na arquitetura Transformer de forma visual e acessível.
  URL: https://www.youtube.com/@3blue1brown (buscar "GPT" e "Attention" no canal)

- **Andrej Karpathy — "Intro to Large Language Models" (YouTube, ~1h)**
  Palestra densa e prática de um dos maiores especialistas da área: cobre pré-treino, fine-tuning, RLHF e uso seguro de LLMs em produção.
  URL: https://www.youtube.com/watch?v=zjkBMFhNj_g

- **Andrej Karpathy — "Let's build GPT from scratch" (YouTube)**
  Implementa um GPT mínimo linha a linha em Python/PyTorch. Imprescindível para entender o que está por baixo das APIs.
  URL: https://www.youtube.com/watch?v=kCc8FmEb1nY

- **Jay Alammar — "The Illustrated Transformer" (blog)**
  Artigo com diagramas animados que ficou referência mundial para explicar o mecanismo de atenção. Leitura obrigatória antes de qualquer código com Transformers.
  URL: https://jalammar.github.io/illustrated-transformer/

- **Hugging Face — "LLM Course" (huggingface.co/learn)**
  Curso oficial gratuito da Hugging Face: abrange a biblioteca `transformers`, fine-tuning, embeddings e deploy. Tem notebooks prontos para rodar no Google Colab.
  URL: https://huggingface.co/learn/llm-course

- **Ollama — Documentação oficial (ollama.com)**
  Guia completo para rodar LLMs localmente (Llama, Mistral, Phi etc.) sem custo de API. Essencial para a stack local descrita neste guia.
  URL: https://ollama.com/docs

---

## RAG (Retrieval-Augmented Generation)

### Grátis

- **Lewis et al. (2020) — "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (paper)**
  O artigo original da Meta que introduziu o conceito de RAG. Vale a leitura da introdução e seção de experimentos mesmo para iniciantes.
  URL: https://arxiv.org/abs/2005.11401

- **Pinecone Learn — Guias de Embeddings e RAG**
  Série de tutoriais práticos que cobre desde o conceito de embedding até pipelines RAG completos com exemplos de código. Não exige conta paga para ler.
  URL: https://www.pinecone.io/learn/

- **Chroma — Documentação oficial**
  Banco de vetores open-source, fácil de rodar localmente. A documentação inclui quickstarts e exemplos de integração com LangChain e LlamaIndex.
  URL: https://docs.trychroma.com/

- **Sentence-Transformers — Documentação oficial**
  Biblioteca Python para gerar embeddings de sentenças com modelos open-source. Referência para escolher e usar modelos de embedding sem custo de API.
  URL: https://www.sbert.net/

- **DeepLearning.AI — "LangChain: Chat with Your Data" (short course, grátis)**
  Curso curto (~2h) que ensina a montar um chatbot RAG com LangChain, incluindo chunking, embeddings e retrieval. Acesso gratuito mediante cadastro.
  URL: https://learn.deeplearning.ai/courses/langchain-chat-with-your-data

- **DeepLearning.AI — "Building and Evaluating Advanced RAG" (short course, grátis)**
  Continuação do anterior: cobre técnicas avançadas como HyDE, reranking e métricas de avaliação de RAG com RAGAS. Gratuito com cadastro.
  URL: https://learn.deeplearning.ai/courses/building-evaluating-advanced-rag

---

## Agentes de IA

### Grátis

- **Anthropic — "Building Effective Agents" (blog)**
  Artigo da própria Anthropic com padrões concretos para construir agentes confiáveis: loops de agente, uso de ferramentas, orquestração e como evitar armadilhas comuns.
  URL: https://www.anthropic.com/research/building-effective-agents

- **Yao et al. (2022) — "ReAct: Synergizing Reasoning and Acting in Language Models" (paper)**
  Paper que introduziu o padrão ReAct (Reason + Act), base de quase todos os frameworks de agentes modernos. Leitura curta e muito esclarecedora.
  URL: https://arxiv.org/abs/2210.03629

- **Hugging Face — "AI Agents Course" (huggingface.co/learn)**
  Curso gratuito e atualizado da Hugging Face que cobre agentes com a biblioteca `smolagents`, padrões de tool-use e agentes multimodais.
  URL: https://huggingface.co/learn/agents-course

- **Hugging Face — smolagents (biblioteca)**
  Biblioteca minimalista para construir agentes em Python com qualquer LLM. A documentação tem exemplos claros e é complementar ao curso acima.
  URL: https://huggingface.co/docs/smolagents

- **DeepLearning.AI — "Functions, Tools and Agents with LangChain" (short course, grátis)**
  Curso curto que ensina a criar agentes com function-calling, ferramentas customizadas e o executor de agentes do LangChain. Gratuito com cadastro.
  URL: https://learn.deeplearning.ai/courses/functions-tools-agents-langchain

---

## Infraestrutura e MLOps

### Grátis

- **FastAPI — Documentação oficial**
  Referência completa para criar APIs Python de alto desempenho — o padrão de mercado para expor modelos de ML. O tutorial "First Steps" já resolve 80% dos casos de uso.
  URL: https://fastapi.tiangolo.com/

- **Made With ML — Goku Mohandas (madewithml.com)**
  Guia aberto de MLOps que cobre desde design de projetos até deploy e monitoramento, com foco em boas práticas de engenharia. Excelente para quem vem do desenvolvimento e quer entender o ciclo completo.
  URL: https://madewithml.com/

- **Chip Huyen — Blog (huyenchip.com)**
  Blog técnico de referência sobre sistemas de ML em produção: artigos sobre serving, latência, monitoramento e design de pipelines. Conteúdo direto e sem paywalls.
  URL: https://huyenchip.com/blog/

- **LiteLLM — Documentação oficial**
  Biblioteca que unifica chamadas para +100 LLMs (OpenAI, Anthropic, modelos locais via Ollama etc.) sob uma API única. Facilita trocar de provedor sem reescrever código.
  URL: https://docs.litellm.ai/

- **vLLM — Documentação oficial**
  Engine de serving open-source para LLMs com alta taxa de throughput. Útil quando se quer hospedar modelos locais com desempenho próximo ao de produção.
  URL: https://docs.vllm.ai/

---

## Avaliação de LLMs (Evals)

### Grátis

- **Hamel Husain — "Your AI Product Needs Evals" (blog)**
  Artigo prático e direto que argumenta por que avaliação não é opcional e mostra como estruturar evals desde o início do projeto, mesmo sem dataset grande.
  URL: https://hamel.dev/blog/posts/evals/

- **promptfoo — Documentação oficial**
  Ferramenta open-source para testar e comparar prompts e modelos sistematicamente. Suporta avaliação automatizada, regressão e comparação A/B de respostas.
  URL: https://www.promptfoo.dev/docs/

- **RAGAS — Documentação oficial**
  Framework open-source especificamente para avaliar pipelines RAG, com métricas como faithfulness, answer relevancy e context recall. Integra com LangChain e LlamaIndex.
  URL: https://docs.ragas.io/

- **DeepLearning.AI — "Evaluating and Debugging Generative AI" (short course, grátis)**
  Curso curto com foco em como monitorar e depurar aplicações de IA generativa usando Weights & Biases. Cobre logging, tracing e avaliação de qualidade. Gratuito com cadastro.
  URL: https://learn.deeplearning.ai/courses/evaluating-debugging-generative-ai

---

## Livros Pagos (Opcionais)

Recomendados apenas se você quiser ir além do que os recursos gratuitos cobrem. Os três abaixo são os mais alinhados com o conteúdo deste guia.

- **"AI Engineering" — Chip Huyen (O'Reilly, 2025)** — livro pago — opcional
  O livro mais alinhado ao currículo deste guia: cobre toda a stack de engenharia de IA — RAG, agentes, evals, serving e segurança — com foco em sistemas reais em produção. Lançado em 2025, reflete o estado atual da área.

- **"Hands-On Large Language Models" — Jay Alammar & Maarten Grootendorst (O'Reilly, 2024)** — livro pago — opcional
  Abordagem visual e prática para LLMs: embeddings, fine-tuning, RAG e prompting com exemplos de código claros. Ótimo complemento para quem já conhece os artigos do blog do Alammar.

- **"Designing Machine Learning Systems" — Chip Huyen (O'Reilly, 2022)** — livro pago — opcional
  Fundamentos sólidos de design de sistemas de ML em produção: feature engineering, serving, monitoramento e pipelines de dados. Base importante mesmo para quem trabalha principalmente com LLMs.

---

> **Dica de uso:** comece pelos recursos marcados como grátis na ordem em que aparecem no [roteiro de estudos](./roteiro.md). Os livros pagos são acréscimos, não pré-requisitos.
