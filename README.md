# 🤖 Guia de Estudo de Engenharia de IA

Um guia de estudo **autodidata, gratuito e em português** que recria o currículo do
[InfoQ Certified AI Engineering Program](https://certification.qconferences.com/ai-engineering)
para quem quer aprender a construir sistemas de IA prontos para produção — RAG,
agentes, infraestrutura e avaliação (evals) — sem pagar por um curso.

> 💡 Feito para uma desenvolvedora que sabe programar mas é novata no domínio de
> IA. Começa pelos fundamentos e vai até um projeto completo de ponta a ponta.

---

## 🎯 O que você vai construir

Ao longo dos módulos, você constrói incrementalmente um **sistema de IA completo**:
um RAG que responde com base nos seus documentos, transformado em um agente com
ferramentas, empacotado como uma API com observabilidade e validado por uma
suíte de avaliação automatizada — exatamente o que se espera de um engenheiro de
IA em produção. No final, você documenta tudo em um artigo técnico para o seu
portfólio.

**Stack (tudo gratuito e local-first):** Python · [Ollama](https://ollama.com) +
modelos abertos · [Chroma](https://www.trychroma.com) · [FastAPI](https://fastapi.tiangolo.com)

---

## 🧭 Como estudar (passo a passo)

O guia **não tem prazo fixo** — você avança no seu ritmo. A regra é simples:
**conclua o marco (🏁) de um módulo antes de passar para o próximo.** Cada módulo
constrói sobre o anterior.

### 1. Prepare o ambiente (uma vez só)
- Instale o [Python 3.10+](https://www.python.org/downloads/) e o [Ollama](https://ollama.com).
- Baixe um modelo aberto para rodar localmente, por exemplo:
  ```bash
  ollama pull llama3.2
  ```
- Crie um ambiente virtual para os exercícios:
  ```bash
  python -m venv .venv && source .venv/bin/activate
  ```

### 2. Siga os módulos em ordem
Para **cada** módulo, percorra as 6 seções nesta ordem:

| Seção | O que fazer |
|-------|-------------|
| 🎯 **Objetivos** | Leia primeiro para saber aonde quer chegar. |
| 🧠 **Conceitos-chave** | Estude a teoria com calma. Não pule. |
| 📚 **Recursos** | Aprofunde com os links gratuitos (vídeos, docs, papers). |
| ⌨️ **Mão na massa** | Faça os exercícios em Python. Aprender é codar, não só ler. |
| 🏁 **Marco** | Entregue o resultado que prova que você dominou o módulo. |
| ✅ **Autoavaliação** | Responda ao checklist com honestidade antes de seguir. |

### 3. Registre seu progresso
Marque os checkboxes em [`guia/PROGRESSO.md`](guia/PROGRESSO.md) conforme avança.
Anote dúvidas e aprendizados — isso vira matéria-prima para o artigo final.

### 4. Feche com o projeto capstone
No [Módulo 6](guia/06-projeto-capstone.md) você integra tudo em um sistema único
e escreve um artigo técnico de 2.000–3.000 palavras. Publique no seu GitHub/blog:
é o seu portfólio de Engenharia de IA.

---

## 📚 Trilha de módulos

| # | Módulo | O que você aprende | Marco |
|---|--------|--------------------|-------|
| 0 | [Fundamentos](guia/00-fundamentos.md) | LLMs, tokens, embeddings, prompting, setup | Script que conversa com um modelo local |
| 1 | [Desenvolvimento AI-native](guia/01-ai-native-dev.md) | Integrar IA no seu fluxo de dev | Seu fluxo documentado + impacto medido |
| 2 | [RAG e Contexto](guia/02-rag-e-contexto.md) | Embeddings, chunking, retrieval | RAG funcional sobre seus documentos |
| 3 | [Agentes de IA](guia/03-agentes.md) | Tool use, loop ReAct, memória | Agente com ≥2 ferramentas |
| 4 | [Plataformas e Infra](guia/04-plataformas-infra.md) | Servir como API, logging, cache, deploy | Seu sistema empacotado como serviço |
| 5 | [Evals e Confiabilidade](guia/05-evals-confiabilidade.md) | Datasets, LLM-as-judge, regressão | Suíte de evals com baseline |
| 6 | [Projeto Capstone](guia/06-projeto-capstone.md) | Integrar tudo + escrever artigo | Projeto rodando + artigo publicado |

📖 Lista completa de recursos gratuitos: [`guia/recursos.md`](guia/recursos.md)

---

## 🗂️ Estrutura do repositório

```
.
├── README.md          ← você está aqui (como estudar)
├── guia/              ← o conteúdo do guia
│   ├── README.md      ← índice detalhado dos módulos
│   ├── PROGRESSO.md   ← seu rastreador de progresso
│   ├── recursos.md    ← biblioteca de recursos gratuitos
│   └── 00..06*.md     ← os 7 módulos
└── docs/              ← spec e plano de implementação do guia
```

---

## ⭐ Dicas para aproveitar melhor

- **Codifique tudo.** Ler sobre RAG não ensina RAG — construir um, sim.
- **Não pule fundamentos.** O Módulo 0 é a base de todo o resto.
- **Erre de propósito.** Mude parâmetros, quebre o código, veja o que acontece.
- **Documente enquanto aprende.** Suas anotações viram o artigo do capstone.
- **Sem pressa.** Sem prazo fixo: melhor um módulo bem feito do que cinco corridos.

Bons estudos! 🚀
