# Guia de Estudo de Engenharia de IA — Plano de Implementação

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar um guia de estudo modular em português que recria a experiência do *InfoQ Certified AI Engineering Program* para estudo individual, autodidata e de baixo/zero custo.

**Architecture:** Uma pasta `guia/` com 9 arquivos Markdown: um índice (`README.md`), um rastreador de progresso (`PROGRESSO.md`), uma biblioteca de recursos (`recursos.md`) e 7 arquivos de módulo (00–06). Os módulos 00–05 seguem um template fixo de 6 seções; o módulo 06 (capstone) tem estrutura própria.

**Tech Stack:** Markdown. O conteúdo dos exercícios assume Python + provedor aberto/local (Ollama, modelos abertos, Chroma, FastAPI), priorizando ferramentas gratuitas.

**Nota sobre git:** o diretório ainda não é um repositório git. A Task 1 faz `git init`. Se a estudante preferir não versionar, pular os passos de commit.

**Nota sobre TDD:** este é um projeto de documentação, não de código. Não há testes automatizados; a "verificação" de cada task é conferir que o arquivo existe e contém todas as seções exigidas pelo template.

---

### Task 1: Setup — repositório e estrutura de pastas

**Files:**
- Create: `guia/` (diretório)

- [ ] **Step 1: Inicializar git (opcional)**

```bash
cd /Users/stephany.batista/Documents/source/guia-course-ia-agent
git init
printf "%s\n" "__pycache__/" "*.pyc" ".venv/" ".env" > .gitignore
```

- [ ] **Step 2: Criar a pasta do guia**

```bash
mkdir -p guia
```

- [ ] **Step 3: Verificar**

Run: `ls -d guia && cat .gitignore`
Expected: a pasta `guia` existe e o `.gitignore` lista os padrões acima.

- [ ] **Step 4: Commit**

```bash
git add .gitignore
git commit -m "chore: inicializa repositório e estrutura do guia"
```

---

### Task 2: README.md — índice e guia de uso

**Files:**
- Create: `guia/README.md`

- [ ] **Step 1: Escrever o README**

Conteúdo obrigatório:
- Título e 1 parágrafo explicando o propósito (recriar o currículo do InfoQ Certified AI Engineering Program para estudo autodidata e gratuito).
- **Como usar:** estudo por módulos, sem prazo fixo; concluir o marco de cada módulo antes de seguir; registrar avanço em `PROGRESSO.md`.
- **Stack escolhida:** Python + provedor aberto/local (Ollama + modelo aberto), Chroma para vetores, FastAPI para serviço; tudo gratuito.
- **Tabela de módulos** com link para cada arquivo:

| # | Módulo | Arquivo |
|---|--------|---------|
| 0 | Fundamentos de LLMs e Engenharia de IA | [00-fundamentos.md](00-fundamentos.md) |
| 1 | Desenvolvimento AI-native | [01-ai-native-dev.md](01-ai-native-dev.md) |
| 2 | RAG e Pipelines de Contexto | [02-rag-e-contexto.md](02-rag-e-contexto.md) |
| 3 | Agentes de IA | [03-agentes.md](03-agentes.md) |
| 4 | Plataformas e Infraestrutura | [04-plataformas-infra.md](04-plataformas-infra.md) |
| 5 | Evals, Confiança e Confiabilidade | [05-evals-confiabilidade.md](05-evals-confiabilidade.md) |
| 6 | Projeto Capstone + Artigo Técnico | [06-projeto-capstone.md](06-projeto-capstone.md) |

- **Template de módulo:** explicar as 6 seções (Objetivos, Conceitos-chave, Recursos, Mão na massa, Marco, Autoavaliação).
- Link para `recursos.md` e `PROGRESSO.md`.

- [ ] **Step 2: Verificar**

Run: `grep -c "00-fundamentos.md\|06-projeto-capstone.md" guia/README.md`
Expected: ≥ 2 (a tabela referencia os arquivos de módulo).

- [ ] **Step 3: Commit**

```bash
git add guia/README.md
git commit -m "docs: adiciona README índice do guia"
```

---

### Task 3: PROGRESSO.md — rastreador de progresso

**Files:**
- Create: `guia/PROGRESSO.md`

- [ ] **Step 1: Escrever o rastreador**

Conteúdo: uma seção por módulo (00–06) com checkboxes para os exercícios e o marco, mais campos de data de início/conclusão e um espaço de notas. Formato exato por módulo:

```markdown
## Módulo 0 — Fundamentos
- Início: ____  | Conclusão: ____
- [ ] Conceitos lidos
- [ ] Exercícios feitos
- [ ] 🏁 Marco concluído
- Notas:
```

Repetir o bloco para os módulos 1 a 6, ajustando o nome. No topo, uma barra de visão geral:

```markdown
# Progresso

- [ ] Módulo 0 — Fundamentos
- [ ] Módulo 1 — AI-native dev
- [ ] Módulo 2 — RAG
- [ ] Módulo 3 — Agentes
- [ ] Módulo 4 — Infra
- [ ] Módulo 5 — Evals
- [ ] Módulo 6 — Capstone
```

- [ ] **Step 2: Verificar**

Run: `grep -c "🏁 Marco concluído" guia/PROGRESSO.md`
Expected: 7 (um por módulo).

- [ ] **Step 3: Commit**

```bash
git add guia/PROGRESSO.md
git commit -m "docs: adiciona rastreador de progresso"
```

---

### Task 4: recursos.md — biblioteca central de recursos

**Files:**
- Create: `guia/recursos.md`

- [ ] **Step 1: Escrever a biblioteca de recursos**

Organizar por tema, marcando claramente o que é **grátis** e o que é **livro pago (opcional)**. Conteúdo mínimo:

**Fundamentos (grátis):**
- 3Blue1Brown — série "Neural networks / Transformers" (YouTube)
- Andrej Karpathy — "Intro to Large Language Models" (YouTube, ~1h)
- Jay Alammar — "The Illustrated Transformer" (blog)
- Hugging Face — "LLM Course" (huggingface.co/learn)
- Ollama — documentação oficial (ollama.com)

**RAG (grátis):**
- Lewis et al. (2020) — paper original "Retrieval-Augmented Generation"
- Pinecone Learn — guias de embeddings e RAG
- Chroma — docs oficiais
- Sentence-Transformers — docs
- DeepLearning.AI — short courses gratuitos "LangChain: Chat with Your Data" e "Building and Evaluating Advanced RAG"

**Agentes (grátis):**
- Anthropic — "Building Effective Agents" (blog)
- Yao et al. — paper "ReAct"
- Hugging Face — "AI Agents Course" e biblioteca `smolagents`
- DeepLearning.AI — "Functions, Tools and Agents with LangChain"

**Infraestrutura (grátis):**
- FastAPI — docs oficiais
- Made With ML (Goku Mohandas) — madewithml.com
- Chip Huyen — blog (huyenchip.com)
- LiteLLM e vLLM — docs

**Evals (grátis):**
- Hamel Husain — "Your AI Product Needs Evals" (blog)
- promptfoo e RAGAS — docs open-source
- DeepLearning.AI — "Evaluating and Debugging Generative AI"

**Livros pagos (opcionais, só se valer muito):**
- *AI Engineering* — Chip Huyen (O'Reilly, 2025) — o mais alinhado ao curso
- *Hands-On Large Language Models* — Jay Alammar & Maarten Grootendorst
- *Designing Machine Learning Systems* — Chip Huyen

- [ ] **Step 2: Verificar**

Run: `grep -c "grátis\|Grátis\|GRÁTIS\|pago" guia/recursos.md`
Expected: ≥ 6 (seções marcadas por custo).

- [ ] **Step 3: Commit**

```bash
git add guia/recursos.md
git commit -m "docs: adiciona biblioteca central de recursos"
```

---

### Task 5: 00-fundamentos.md — Módulo 0

**Files:**
- Create: `guia/00-fundamentos.md`

- [ ] **Step 1: Escrever o módulo seguindo o template de 6 seções**

- **🎯 Objetivos:** explicar o que é um LLM e seus componentes; fazer a primeira chamada a um modelo via código; entender os parâmetros de geração.
- **🧠 Conceitos-chave:** tokens e tokenização; embeddings; o que é prompting (system/user, few-shot); parâmetros (temperature, top_p, max_tokens); janela de contexto; alucinação e limitações; diferença entre "usar um chatbot" e "engenharia de IA". Texto direto, 1-2 parágrafos por conceito.
- **📚 Recursos:** Karpathy "Intro to LLMs", 3Blue1Brown, Illustrated Transformer, HF LLM Course, docs do Ollama (ver `recursos.md`).
- **⌨️ Mão na massa (exercícios concretos):**
  1. Instalar Ollama e baixar um modelo aberto (`ollama pull llama3.2`).
  2. Script Python que envia um prompt ao modelo local e imprime a resposta (via API HTTP do Ollama ou biblioteca `ollama`).
  3. Variar `temperature` (0, 0.7, 1.5) no mesmo prompt e comparar as saídas.
  4. Contar tokens de um texto e observar o efeito no `max_tokens`.
- **🏁 Marco:** um script Python comentado que conversa com o modelo local, com explicação de cada parâmetro usado.
- **✅ Autoavaliação:** consigo explicar token, embedding, temperatura e janela de contexto? Sei por que um LLM alucina? Rodei o script com sucesso?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/00-fundamentos.md`
Expected: 6 (todas as seções do template presentes).

- [ ] **Step 3: Commit**

```bash
git add guia/00-fundamentos.md
git commit -m "docs: adiciona Módulo 0 — fundamentos"
```

---

### Task 6: 01-ai-native-dev.md — Módulo 1

**Files:**
- Create: `guia/01-ai-native-dev.md`

- [ ] **Step 1: Escrever o módulo (template de 6 seções)**

- **🎯 Objetivos:** formalizar um fluxo de desenvolvimento AI-native; saber quando confiar e quando revisar saídas de IA; medir impacto na produtividade.
- **🧠 Conceitos-chave:** assistentes de código (ex: Claude Code) no ciclo de dev; geração vs. revisão; padrões de prompting para tarefas de engenharia; riscos (confiança cega, dívida técnica, vazamento de contexto); produtividade vs. qualidade; verificação de saídas. Conectar à experiência prévia da estudante com Claude Code.
- **📚 Recursos:** Anthropic "Building Effective Agents" (parte introdutória), docs de assistentes de código, artigos sobre AI-assisted development (ver `recursos.md`).
- **⌨️ Mão na massa:**
  1. Escolher uma tarefa real de código e resolvê-la com auxílio de IA, registrando os prompts usados.
  2. Refazer mentalmente a mesma tarefa sem IA e estimar/comparar o tempo.
  3. Criar uma checklist pessoal de "quando revisar a fundo a saída da IA".
- **🏁 Marco:** documento (1 página) descrevendo seu fluxo AI-native + medição de impacto em uma tarefa real.
- **✅ Autoavaliação:** tenho um fluxo documentado? Sei identificar quando a saída de IA precisa de revisão crítica? Medi algum impacto concreto?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/01-ai-native-dev.md`
Expected: 6

- [ ] **Step 3: Commit**

```bash
git add guia/01-ai-native-dev.md
git commit -m "docs: adiciona Módulo 1 — desenvolvimento AI-native"
```

---

### Task 7: 02-rag-e-contexto.md — Módulo 2

**Files:**
- Create: `guia/02-rag-e-contexto.md`

- [ ] **Step 1: Escrever o módulo (template de 6 seções)**

- **🎯 Objetivos:** construir um pipeline RAG funcional do zero; entender e avaliar a qualidade do retrieval.
- **🧠 Conceitos-chave:** por que RAG (limite de contexto, conhecimento atualizado); embeddings e busca por similaridade; bancos vetoriais; estratégias de chunking (tamanho, overlap); pipeline ingestão → indexação → retrieval → geração; montagem de contexto/prompt; avaliação de retrieval (precisão, recall, relevância).
- **📚 Recursos:** paper RAG (Lewis 2020), Pinecone Learn, docs Chroma e Sentence-Transformers, DeepLearning.AI "Chat with Your Data" e "Advanced RAG" (ver `recursos.md`).
- **⌨️ Mão na massa:**
  1. Indexar um conjunto de documentos próprios (PDFs/notas) com embeddings de um modelo aberto (Sentence-Transformers) em um Chroma local.
  2. Implementar a função de retrieval (top-k) e inspecionar os trechos recuperados.
  3. Montar o prompt com contexto recuperado e gerar a resposta via modelo local.
  4. Experimentar 2 estratégias de chunking e comparar a qualidade das respostas.
- **🏁 Marco:** um RAG funcional sobre documentos próprios, respondendo perguntas com base neles.
- **✅ Autoavaliação:** sei explicar cada etapa do pipeline? Consigo justificar minha escolha de chunking? Sei avaliar se o retrieval trouxe contexto relevante?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/02-rag-e-contexto.md`
Expected: 6

- [ ] **Step 3: Commit**

```bash
git add guia/02-rag-e-contexto.md
git commit -m "docs: adiciona Módulo 2 — RAG e contexto"
```

---

### Task 8: 03-agentes.md — Módulo 3

**Files:**
- Create: `guia/03-agentes.md`

- [ ] **Step 1: Escrever o módulo (template de 6 seções)**

- **🎯 Objetivos:** entender o que é um agente; construir um agente do zero com tool use; reconhecer padrões e armadilhas.
- **🧠 Conceitos-chave:** definição de agente vs. chamada simples a LLM; loop raciocínio → ação → observação (ReAct); tool use / function calling; memória (curto/longo prazo); planejamento e tarefas multi-step; quando NÃO usar agentes (complexidade desnecessária); padrões da Anthropic ("Building Effective Agents").
- **📚 Recursos:** Anthropic "Building Effective Agents", paper ReAct, HF Agents Course + `smolagents`, DeepLearning.AI "Functions, Tools and Agents" (ver `recursos.md`).
- **⌨️ Mão na massa:**
  1. Implementar um loop de agente mínimo do zero (sem framework): LLM decide qual ferramenta chamar, executa, observa, repete.
  2. Criar 2 ferramentas (ex: calculadora e busca em arquivo/RAG do Módulo 2).
  3. Dar ao agente uma tarefa multi-step que exija usar as duas ferramentas em sequência.
  4. Adicionar memória simples (histórico) e observar a diferença.
- **🏁 Marco:** um agente que usa ≥2 ferramentas para resolver uma tarefa de múltiplos passos.
- **✅ Autoavaliação:** sei explicar o loop raciocínio-ação? Sei quando um agente é (e não é) a escolha certa? Meu agente encadeia ferramentas corretamente?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/03-agentes.md`
Expected: 6

- [ ] **Step 3: Commit**

```bash
git add guia/03-agentes.md
git commit -m "docs: adiciona Módulo 3 — agentes"
```

---

### Task 9: 04-plataformas-infra.md — Módulo 4

**Files:**
- Create: `guia/04-plataformas-infra.md`

- [ ] **Step 1: Escrever o módulo (template de 6 seções)**

- **🎯 Objetivos:** empacotar um sistema de IA como serviço; entender preocupações de produção (custo, latência, observabilidade).
- **🧠 Conceitos-chave:** servir modelos (local vs. API); expor via API (REST/FastAPI); custo e latência (caching de respostas/embeddings, batching, filas); observabilidade (logging estruturado, tracing, métricas); tratamento de erros e timeouts; deploy básico (container). Versão local-first, mas com os conceitos de produção nomeados.
- **📚 Recursos:** docs FastAPI, Made With ML, blog do Chip Huyen, docs LiteLLM/vLLM (ver `recursos.md`).
- **⌨️ Mão na massa:**
  1. Expor o agente/RAG dos módulos anteriores como uma API FastAPI com um endpoint.
  2. Adicionar logging estruturado de cada requisição (entrada, latência, tokens).
  3. Adicionar cache simples para respostas/embeddings repetidos.
  4. Adicionar tratamento de erro e timeout para falhas do modelo.
- **🏁 Marco:** o agente/RAG empacotado como serviço (API) com logging e tratamento de erros.
- **✅ Autoavaliação:** minha API responde e loga corretamente? Sei onde o custo/latência aparece? Sei o que mudaria para deploy real?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/04-plataformas-infra.md`
Expected: 6

- [ ] **Step 3: Commit**

```bash
git add guia/04-plataformas-infra.md
git commit -m "docs: adiciona Módulo 4 — plataformas e infraestrutura"
```

---

### Task 10: 05-evals-confiabilidade.md — Módulo 5

**Files:**
- Create: `guia/05-evals-confiabilidade.md`

- [ ] **Step 1: Escrever o módulo (template de 6 seções)**

- **🎯 Objetivos:** criar uma suíte de avaliação para um sistema de IA; detectar regressões; aumentar confiabilidade.
- **🧠 Conceitos-chave:** por que evals são o que separa amador de profissional; datasets de avaliação (exemplos + saídas esperadas); tipos de eval (assertions/regras, LLM-as-judge, métricas de RAG como faithfulness e relevância); detecção de regressão; guardrails (validação de entrada/saída); confiabilidade (retries, fallbacks).
- **📚 Recursos:** Hamel Husain "Your AI Product Needs Evals", docs promptfoo e RAGAS, DeepLearning.AI "Evaluating and Debugging Generative AI" (ver `recursos.md`).
- **⌨️ Mão na massa:**
  1. Montar um dataset de avaliação (10-20 perguntas + respostas/critérios esperados) para o RAG/agente.
  2. Escrever evals baseadas em regras (ex: a resposta contém X, não contém Y).
  3. Implementar um eval "LLM-as-judge" para qualidade da resposta.
  4. Rodar a suíte, registrar a baseline e simular uma regressão para confirmar que a suíte a detecta.
- **🏁 Marco:** uma suíte de evals automatizada para o seu projeto, com baseline registrada.
- **✅ Autoavaliação:** tenho um dataset de avaliação? Minha suíte detecta regressões? Sei explicar prós/contras de LLM-as-judge?

- [ ] **Step 2: Verificar**

Run: `grep -c "🎯\|🧠\|📚\|⌨️\|🏁\|✅" guia/05-evals-confiabilidade.md`
Expected: 6

- [ ] **Step 3: Commit**

```bash
git add guia/05-evals-confiabilidade.md
git commit -m "docs: adiciona Módulo 5 — evals e confiabilidade"
```

---

### Task 11: 06-projeto-capstone.md — Módulo 6

**Files:**
- Create: `guia/06-projeto-capstone.md`

- [ ] **Step 1: Escrever o módulo capstone (estrutura própria)**

- **🎯 Objetivo:** integrar todos os módulos em um sistema de IA de ponta a ponta e documentar as decisões em um artigo técnico (2.000-3.000 palavras), como no curso original.
- **Escopo do projeto:** combinar RAG (Mód. 2) + agente (Mód. 3) + serviço/API (Mód. 4) + suíte de evals (Mód. 5) sobre um domínio à escolha da estudante. Sugestões de domínio: assistente sobre documentação técnica própria, agente de pesquisa sobre uma base de artigos, Q&A sobre código de um repositório.
- **Requisitos mínimos do sistema:** ingestão de documentos; retrieval; pelo menos um agente com ≥2 ferramentas; API com logging; suíte de evals com baseline.
- **Roteiro do artigo técnico** (com contagem alvo de palavras por seção):
  1. Problema e contexto (~300)
  2. Arquitetura e decisões de design (~700)
  3. Implementação: RAG, agente, infra (~800)
  4. Avaliação: como medi qualidade e confiabilidade (~600)
  5. Aprendizados, limitações e próximos passos (~400)
- **Entrega:** projeto rodando + artigo publicado (GitHub README/blog).
- **✅ Autoavaliação:** o sistema integra os 4 pilares? O artigo explica as decisões com clareza? Consigo defender cada escolha técnica?

- [ ] **Step 2: Verificar**

Run: `grep -c "Roteiro do artigo\|Escopo do projeto\|Requisitos mínimos" guia/06-projeto-capstone.md`
Expected: 3

- [ ] **Step 3: Commit**

```bash
git add guia/06-projeto-capstone.md
git commit -m "docs: adiciona Módulo 6 — capstone e artigo técnico"
```

---

### Task 12: Revisão final e atualização do índice

**Files:**
- Modify: `guia/README.md`

- [ ] **Step 1: Conferir consistência**

Run: `ls guia/`
Expected: README.md, PROGRESSO.md, recursos.md, 00-fundamentos.md, 01-ai-native-dev.md, 02-rag-e-contexto.md, 03-agentes.md, 04-plataformas-infra.md, 05-evals-confiabilidade.md, 06-projeto-capstone.md (10 arquivos).

- [ ] **Step 2: Verificar que cada módulo 00–05 tem as 6 seções**

Run: `for f in guia/0[0-5]*.md; do echo "$f: $(grep -c '🎯\|🧠\|📚\|⌨️\|🏁\|✅' "$f")"; done`
Expected: cada arquivo reporta 6.

- [ ] **Step 3: Corrigir links quebrados no README, se houver**

Conferir que todos os links da tabela de módulos do README apontam para arquivos existentes.

- [ ] **Step 4: Commit**

```bash
git add guia/
git commit -m "docs: revisão final do guia de estudo"
```

---

## Self-Review

**Spec coverage:**
- Estrutura de 9 arquivos do guia → Tasks 2-11 (README, PROGRESSO, recursos, 7 módulos). ✅
- Template de 6 seções nos módulos 00–05 → Tasks 5-10, verificado na Task 12. ✅
- Módulo 0 de fundamentos → Task 5. ✅
- Semana 1 adaptada (versão individual) → Task 6. ✅
- Módulos 2-5 (RAG, agentes, infra, evals) → Tasks 7-10. ✅
- Capstone + artigo técnico 2.000-3.000 palavras → Task 11. ✅
- Recursos gratuitos + livros opcionais → Task 4. ✅
- Rastreador de progresso → Task 3. ✅
- Local-first / Python + provedor aberto → exercícios nas Tasks 5-11. ✅
- Fora de escopo (certificação, scaffold pré-montado, mentoria ao vivo) → respeitado; nenhuma task os cria. ✅

**Placeholder scan:** cada task de módulo lista conceitos, recursos concretos e exercícios numerados específicos — sem "TBD"/"etc. genérico". ✅

**Type/nome consistency:** nomes de arquivos idênticos entre spec, README (Task 2), comandos de verificação e tasks. As 6 seções usam os mesmos emojis em todas as tasks e nas verificações. ✅
