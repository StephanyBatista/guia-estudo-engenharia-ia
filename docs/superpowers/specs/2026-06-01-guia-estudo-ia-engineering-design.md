# Design — Guia de Estudo de Engenharia de IA

**Data:** 2026-06-01
**Autor:** Stephany Batista (com Claude Code)
**Status:** Aprovado (design)

## Contexto e motivação

O curso *InfoQ Certified AI Engineering Program* (QCon/InfoQ) é uma formação
paga (USD 1.470) sobre construção de sistemas de IA prontos para produção.
Como não há condições de pagar pelo curso, este projeto recria a **experiência
de aprendizado** a partir do currículo público do curso, adaptada para estudo
individual, autodidata e de baixo/zero custo.

### Perfil da estudante
- Desenvolvedora competente; usuária de Claude Code.
- Novata no domínio de Engenharia de IA (RAG, agentes, infra, evals) → precisa
  de fundamentos antes dos tópicos avançados.

### Objetivos
1. Ganhar competência aplicável à carreira em Engenharia de IA.
2. Replicar a experiência completa do curso: teoria + exercícios + projeto +
   artigo técnico final.

### Restrições e preferências
- **Ritmo:** sem prazo fixo; organizado por módulos/marcos.
- **Stack:** Python + provedor aberto/local (ex: Ollama, modelos abertos) e
  tiers gratuitos, evitando custos de API.
- **Recursos:** prioridade total a recursos gratuitos (docs, papers, vídeos,
  blogs); 1-2 livros pagos apenas quando o valor for muito alto.
- **Idioma do guia:** português.

## Currículo de referência (curso original, 5 semanas)
1. Becoming an AI-Native Engineering Team
2. Designing & Building RAG & Context Pipelines
3. Designing & Building AI Agents
4. AI Platforms & Infrastructure
5. AI Operational Excellence: Evals, Trust & Reliability (+ capstone)

## Arquitetura do guia

Guia **modular**: uma pasta `guia/` com um arquivo por módulo, um índice, um
rastreador de progresso e uma biblioteca de recursos. Cada arquivo é uma
unidade focada e independente, facilitando navegação, expansão e
acompanhamento ao longo de um estudo sem prazo fixo.

```
guia/
├── README.md                  # Índice, visão geral, como usar, stack escolhida
├── PROGRESSO.md               # Rastreador: checkboxes por módulo + datas + notas
├── 00-fundamentos.md          # Módulo 0 — base nova (LLMs, prompting, setup)
├── 01-ai-native-dev.md        # Módulo 1 — adaptado da Semana 1 (versão individual)
├── 02-rag-e-contexto.md       # Módulo 2 — Semana 2
├── 03-agentes.md              # Módulo 3 — Semana 3
├── 04-plataformas-infra.md    # Módulo 4 — Semana 4
├── 05-evals-confiabilidade.md # Módulo 5 — Semana 5
├── 06-projeto-capstone.md     # Módulo 6 — projeto final + artigo técnico
└── recursos.md                # Biblioteca central de links grátis + livros sugeridos
```

### Template padrão de cada módulo
Todos os módulos (00–05) seguem a mesma estrutura, para previsibilidade:

1. **🎯 Objetivos de aprendizado** — o que saber fazer ao final.
2. **🧠 Conceitos-chave** — resumo teórico direto, com os termos do domínio.
3. **📚 Recursos** — links gratuitos curados + livro opcional quando valer.
4. **⌨️ Mão na massa** — 2-4 exercícios em Python + provedor aberto/local.
5. **🏁 Marco (milestone)** — entregável que prova o domínio do módulo.
6. **✅ Autoavaliação** — checklist para validar antes de seguir.

O Módulo 06 (capstone) tem estrutura própria (escopo do projeto + roteiro do
artigo) por ser integrador.

## Conteúdo dos módulos

### Módulo 0 — Fundamentos de LLMs e Engenharia de IA *(novo)*
O que é um LLM, tokens, embeddings, prompting, parâmetros (temperatura etc.),
limitações (alucinação, janela de contexto), e o que diferencia "usar um
chatbot" de "engenharia de IA". Setup do ambiente: Python, Ollama + modelo
aberto local, primeira chamada a um LLM via código.
**Marco:** script Python que conversa com um modelo local, com entendimento de
cada parâmetro.

### Módulo 1 — Desenvolvimento AI-native (adaptado da Semana 1)
Versão individual da "Semana 1". Integrar IA no fluxo de engenharia
(aproveitando a experiência com Claude Code): assistentes de código,
geração/revisão, quando confiar e quando não, produtividade vs. qualidade.
**Marco:** documentar o próprio fluxo AI-native e medir impacto em uma tarefa
real.

### Módulo 2 — RAG e Pipelines de Contexto (Semana 2)
Embeddings, bancos vetoriais (ex: Chroma local), chunking, retrieval, montagem
de contexto, avaliação de qualidade do retrieval — com modelos abertos.
**Marco:** RAG funcional sobre documentos próprios (PDFs, notas).

### Módulo 3 — Agentes de IA (Semana 3)
O que é um agente, loop raciocínio/ação, tool use (function calling), memória,
planejamento, multi-step. Padrões e armadilhas. Construir um agente do zero
antes de usar frameworks.
**Marco:** agente que usa ≥2 ferramentas para resolver tarefa de múltiplos
passos.

### Módulo 4 — Plataformas e Infraestrutura (Semana 4)
Levar à produção: servir modelos, APIs, custos/latência, caching, filas,
observabilidade, deploy. Versão enxuta e local-first, com conceitos de
produção.
**Marco:** empacotar o agente/RAG como serviço (API) com logging e tratamento
de erros.

### Módulo 5 — Evals, Confiança e Confiabilidade (Semana 5)
Avaliar sistemas de IA: datasets de avaliação, métricas, testes automatizados
de qualidade, detecção de regressão, guardrails, confiabilidade.
**Marco:** suíte de evals automatizada para o projeto.

### Módulo 6 — Projeto Capstone + Artigo Técnico
Integra tudo: sistema de IA de ponta a ponta (RAG + agente + infra + evals) e a
redação de um **artigo técnico de 2.000-3.000 palavras** documentando as
decisões — para portfólio/carreira.
**Marco:** projeto rodando + artigo publicado (ex: GitHub/blog).

## Decisões de design
- **Modular vs. arquivo único:** modular, para navegação e progresso de longo
  prazo sem prazo fixo.
- **Módulo 0 adicionado:** o currículo original assume senioridade no domínio;
  como a estudante é novata em IA, fundamentos vêm primeiro.
- **Semana 1 adaptada:** o conteúdo original é focado em time/organização;
  aqui é reformulado para um contexto individual.
- **Scaffold de código não criado agora:** os exercícios são construídos pela
  estudante conforme cada módulo pede (YAGNI).
- **Local-first:** ferramentas gratuitas/locais como padrão para eliminar
  custos.

## Fora de escopo
- Certificação oficial (não é possível sem o curso pago).
- Scaffold/repositório de código pré-montado.
- Conteúdo de mentoria ao vivo / peer learning do curso original.

## Critérios de sucesso
- Os 9 arquivos do guia existem e seguem o template definido.
- Cada módulo (00–05) tem objetivos, conceitos, recursos gratuitos curados,
  exercícios em Python local-first, marco e autoavaliação.
- O Módulo 06 define escopo do projeto integrador e roteiro do artigo técnico.
- `PROGRESSO.md` permite acompanhar o avanço por módulo.
