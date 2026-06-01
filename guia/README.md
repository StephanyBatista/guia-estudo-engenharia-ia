# Guia de Estudo: AI Engineering Program

Este guia recria o currículo do **InfoQ Certified AI Engineering Program** para estudo individual, autodidata e gratuito. O objetivo é percorrer os mesmos tópicos do programa oficial — fundamentos de LLMs, RAG, agentes, infraestrutura e avaliação — usando exclusivamente ferramentas abertas e sem custo. Cada módulo combina leitura de conceitos, exercícios práticos e um marco concreto que deve ser concluído antes de avançar para o próximo.

---

## Como usar

1. **Estude por módulos, no seu próprio ritmo.** Não há prazo fixo; o objetivo é a compreensão, não a velocidade.
2. **Conclua o marco de cada módulo antes de avançar.** O marco é o critério mínimo de conclusão descrito ao final de cada arquivo. Só passe para o próximo módulo depois de cumpri-lo.
3. **Registre seu avanço em [PROGRESSO.md](PROGRESSO.md).** A cada módulo concluído, marque a data e anote observações sobre o que aprendeu ou o que ficou em aberto.

---

## Stack escolhida

Tudo gratuito, tudo local ou open source:

| Componente | Ferramenta |
|---|---|
| Linguagem | Python 3.11+ |
| Modelo de linguagem | [Ollama](https://ollama.com/) com modelo aberto (ex: `llama3`, `mistral`, `gemma`) |
| Banco de vetores | [Chroma](https://www.trychroma.com/) |
| Serviço/API | [FastAPI](https://fastapi.tiangolo.com/) |
| Orquestração de agentes | LangChain ou código puro (sem dependência paga) |

> Nenhuma chave de API paga é necessária. Todos os exercícios foram desenhados para rodar localmente com Ollama.

---

## Módulos

| # | Módulo | Arquivo |
|---|--------|---------|
| 0 | Fundamentos de LLMs e Engenharia de IA | [00-fundamentos.md](00-fundamentos.md) |
| 1 | Desenvolvimento AI-native | [01-ai-native-dev.md](01-ai-native-dev.md) |
| 2 | RAG e Pipelines de Contexto | [02-rag-e-contexto.md](02-rag-e-contexto.md) |
| 3 | Agentes de IA | [03-agentes.md](03-agentes.md) |
| 4 | Plataformas e Infraestrutura | [04-plataformas-infra.md](04-plataformas-infra.md) |
| 5 | Evals, Confiança e Confiabilidade | [05-evals-confiabilidade.md](05-evals-confiabilidade.md) |
| 6 | Projeto Capstone + Artigo Técnico | [06-projeto-capstone.md](06-projeto-capstone.md) |

---

## Template de módulo

Cada arquivo de módulo segue a mesma estrutura de seis seções:

| Seção | O que contém |
|---|---|
| **Objetivos** | O que você será capaz de fazer ao concluir o módulo. |
| **Conceitos-chave** | Definições e explicações dos termos e ideias centrais do módulo. |
| **Recursos** | Links para artigos, vídeos, papers e documentações gratuitas para aprofundamento. |
| **Mao na massa** | Exercícios práticos com código Python para executar localmente. |
| **Marco** | Entregável ou demonstração mínima que comprova a conclusão do módulo. |
| **Autoavaliacao** | Perguntas para checar o próprio entendimento antes de avançar. |

---

## Arquivos de apoio

- [recursos.md](recursos.md) — lista consolidada de referências e leituras recomendadas para todo o guia.
- [PROGRESSO.md](PROGRESSO.md) — seu diário de progresso; atualize a cada módulo concluído.
