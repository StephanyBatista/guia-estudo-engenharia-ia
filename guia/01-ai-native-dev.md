# Módulo 1 — Desenvolvimento AI-native

> **Contexto do módulo:** O programa original aborda este tema no contexto de times e organizações. Aqui, a perspectiva é individual: como *você*, que já usa Claude Code no dia a dia, pode formalizar e aprofundar sua prática de desenvolvimento com auxílio de IA — transformando um hábito informal em um fluxo deliberado e mensurável.

---

## 🎯 Objetivos de aprendizado

Ao concluir este módulo, você será capaz de:

- Descrever e documentar seu próprio fluxo de desenvolvimento AI-native, desde o início de uma tarefa até a entrega.
- Distinguir com clareza quando aceitar uma saída de IA e quando submetê-la a revisão crítica.
- Aplicar padrões de prompting específicos para tarefas de engenharia de software.
- Medir o impacto da IA na sua produtividade usando dados reais de uma tarefa concreta.
- Identificar os principais riscos do uso cotidiano de assistentes de código e adotar contramedidas práticas.

---

## 🧠 Conceitos-chave

### 1. O que é desenvolvimento AI-native?

Desenvolvimento AI-native não é simplesmente "usar IA para escrever código". É uma mudança de postura: o assistente de IA deixa de ser uma ferramenta ocasional e passa a ser parte estrutural do ciclo de desenvolvimento — presente no planejamento, na implementação, na revisão e na documentação.

Você já está nesse caminho ao usar Claude Code. O que este módulo formaliza é a diferença entre uso reativo (pedir ajuda quando trava) e uso proativo (desenhar a tarefa junto com a IA desde o início).

**Ciclo típico de desenvolvimento AI-native:**

```
Entender o problema
       ↓
Decompor em subtarefas
       ↓
Redigir prompt(s) com contexto adequado
       ↓
Gerar saída com o assistente
       ↓
Revisar e validar a saída
       ↓
Iterar (ajustar prompt ou saída)
       ↓
Integrar e testar
```

Cada etapa tem um papel diferente para a IA. Entender isso é o primeiro passo para usar o assistente de forma deliberada.

---

### 2. Geração vs. revisão: dois modos distintos

Ao trabalhar com Claude Code (ou qualquer assistente de código), você alterna entre dois modos mentais:

| Modo | O que você faz | Quando usar |
|------|---------------|-------------|
| **Geração** | Pede ao assistente que produza código, testes, documentação ou planos | Tarefas bem definidas, boilerplate, tradução de intenção para código |
| **Revisão** | Analisa criticamente o que o assistente produziu | Sempre, mas com profundidade variável |

O erro mais comum de quem está começando é tratar geração e revisão como a mesma coisa. Gerar é fácil. Revisar exige domínio do contexto — e é onde a sua responsabilidade como engenheira não diminui.

Uma analogia útil: você pode contratar um estagiário muito rápido para escrever código. Mas quem faz o code review, entende as implicações de segurança e assina o PR continuando sendo você.

---

### 3. Padrões de prompting para tarefas de engenharia

Prompts vagos produzem saídas genéricas. Para tarefas de engenharia, três padrões se destacam:

#### 3a. Contexto + Restrições + Formato

```
Contexto: "Estou num serviço Python com FastAPI. A função recebe um dict com campos 
opcionais e precisa validar antes de persistir no banco."

Restrição: "Use Pydantic v2. Sem dependências extras. Mantenha compatibilidade com 
Python 3.11."

Formato desejado: "Retorne o modelo Pydantic, os validadores customizados e um 
exemplo de uso no docstring."
```

Quando você usa Claude Code no terminal, já fornece muito contexto implicitamente (arquivos abertos, histórico de conversa). Mas tornar o contexto explícito no prompt melhora consistentemente a qualidade da saída.

#### 3b. Role prompting para revisão crítica

```
"Aja como um engenheiro sênior revisando este código antes de ir para produção. 
Aponte problemas de performance, segurança e manutenibilidade. Seja direto."
```

Esse padrão é especialmente útil para pedir ao Claude Code que revise o próprio código que ele acabou de gerar — forçando uma segunda perspectiva.

#### 3c. Decomposição progressiva

Em vez de pedir tudo de uma vez para tarefas complexas:

```
Etapa 1: "Quais são as principais abordagens para resolver X? Liste prós e contras."
Etapa 2: "Vamos com a abordagem B. Esboce a estrutura de dados."
Etapa 3: "Implemente a função principal com base nesse esboço."
Etapa 4: "Escreva os testes para os casos de borda que identificamos."
```

Você provavelmente já faz isso intuitivamente com Claude Code. O objetivo aqui é tornar esse processo consciente e replicável.

---

### 4. Riscos do desenvolvimento AI-native

Usar IA no desenvolvimento cria riscos específicos que não existiam antes. Conhecê-los é o primeiro passo para mitigá-los.

#### 4a. Confiança cega

O modelo de linguagem gera texto que parece correto. Ele pode gerar código sintaticamente válido, com boa formatação e comentários claros — que simplesmente não faz o que você precisa, ou tem um bug sutil.

**Sinal de alerta:** você aceita uma resposta porque ela "parece certa" sem executar ou raciocinar sobre ela.

#### 4b. Dívida técnica acelerada

A IA pode gerar código funcional que é difícil de manter: nomes genéricos, ausência de tratamento de erros, abstrações que não se encaixam na arquitetura do projeto. Se você aceitar esse código sem adaptar, a dívida técnica se acumula mais rápido do que sem IA.

**Contramedida:** revise sempre a aderência ao estilo e às convenções do projeto antes de integrar.

#### 4c. Vazamento de contexto

Claude Code tem acesso ao contexto da sua sessão. Se você incluir credenciais, tokens ou dados sensíveis no contexto (seja em arquivos, mensagens ou comandos), esses dados circulam pelo modelo. Isso não é exclusivo do Claude Code — vale para qualquer assistente de código com acesso ao filesystem.

**Contramedida:** nunca cole secrets em mensagens para o assistente. Use variáveis de ambiente. Revise o que está no contexto da sessão.

#### 4d. Erosão do entendimento

Se você nunca precisa pensar no problema porque o assistente resolve, você deixa de desenvolver a intuição que seria adquirida resolvendo. Isso é invisível no curto prazo e cumulativo no longo prazo.

**Contramedida:** periodicamente, resolva tarefas sem auxílio de IA. Este módulo pede exatamente isso em um dos exercícios.

---

### 5. Produtividade vs. qualidade: a tensão real

A percepção imediata de usar IA é de ganho de velocidade. E é real — você gera código mais rápido, encontra documentação mais rápido, escreve boilerplate mais rápido.

Mas produtividade não é só velocidade de escrita. Um entregável que precisa de três rodadas de fix por bugs que uma revisão cuidadosa teria evitado pode ser menos produtivo do que uma implementação mais lenta e deliberada.

A medição de impacto que este módulo propõe não é "quanto código gerei" mas sim: **quanto tempo levei do entendimento do problema até um código integrado e testado?** E em qual percentual desse tempo a IA contribuiu positivamente?

---

### 6. Verificação de saídas: um framework mínimo

Para cada saída do assistente, faça ao menos as perguntas abaixo antes de integrar:

| Nível | Pergunta | Quando aprofundar |
|-------|----------|-------------------|
| **Sintático** | O código compila/executa sem erros? | Sempre — execute localmente |
| **Semântico** | O código faz o que você pediu? | Sempre — trace o fluxo mentalmente ou com testes |
| **Contextual** | O código se encaixa na arquitetura existente? | Para qualquer código que vai para um PR |
| **Segurança** | Há inputs não validados, secrets hardcoded, SQL injection, etc.? | Para código que lida com dados externos |
| **Manutenibilidade** | Um colega (ou você daqui a 6 meses) vai entender isso? | Para qualquer código que fica no repositório |

Você não precisa fazer todos os níveis para todo snippet. O exercício 3 deste módulo pede que você defina seus próprios critérios de quando ir a cada nível.

---

## 📚 Recursos

Os recursos abaixo aprofundam os conceitos deste módulo. A lista completa e atualizada está em [recursos.md](recursos.md).

**Leitura prioritária para este módulo:**

- **Anthropic — "Building Effective Agents"** (introdução e seção de padrões)
  Mesmo sendo focado em agentes autônomos, a introdução discute o espectro de assistência da IA — de augmentation até agentes autônomos — e é a âncora conceitual deste módulo.
  URL: https://www.anthropic.com/research/building-effective-agents

- **Documentação do Claude Code** (Anthropic)
  Referência oficial para as capacidades, limitações e boas práticas de uso do Claude Code. Especialmente as seções sobre contexto de sessão e permissões de arquivos.
  URL: https://docs.anthropic.com/en/docs/claude-code

- **Simon Willison — "AI-assisted development" (blog)**
  Simon Willison documenta extensivamente sua prática de uso de LLMs para desenvolvimento. Os posts sobre "how I use LLMs" são referência para desenvolvedores que querem um fluxo deliberado.
  URL: https://simonwillison.net (buscar "LLM" e "AI-assisted")

- **Addy Osmani — "The 70% problem: Hard truths about AI-assisted coding" (blog)**
  Artigo direto sobre o risco de usar IA para as partes fáceis enquanto as partes difíceis continuam difíceis — e como isso pode criar uma falsa sensação de produtividade.
  URL: https://addyo.substack.com/p/the-70-problem-hard-truths-about

**Para contexto adicional:**
Veja as seções "Agentes de IA" e "Avaliação de LLMs" em [recursos.md](recursos.md) para leituras que antecipam os módulos seguintes.

---

## ⌨️ Mão na massa

### Exercício 1 — Tarefa real com registro de prompts

**Objetivo:** tornar visível o que você já faz intuitivamente.

1. Escolha uma tarefa de código real do seu trabalho ou de um projeto pessoal. Deve ser algo que você faria de qualquer forma — não um exercício artificial.

2. Resolva a tarefa usando Claude Code normalmente, mas agora **registre em um arquivo de texto**:
   - Cada prompt que você enviou (cole o texto exato)
   - O que na resposta você aceitou sem modificar
   - O que você modificou e por quê
   - O que você descartou e por quê

3. Ao final, responda:
   - Quantas iterações foram necessárias?
   - Em qual momento você sentiu mais incerteza sobre a qualidade da saída?
   - Houve algum momento em que a IA gerou algo que parecia certo mas não era?

**Formato de registro sugerido:**

```markdown
## Tarefa: [descrição]
**Data:** [data]
**Tempo total:** [estimativa]

### Prompt 1
[texto do prompt]

**Saída:** [aceitei / modifiquei / descartei]
**Motivo:** [por quê]

### Prompt 2
...

### Reflexão final
- Iterações: X
- Momento de maior incerteza: [descrição]
- Surpresas (positivas ou negativas): [descrição]
```

---

### Exercício 2 — Estimativa comparativa

**Objetivo:** calibrar a sua percepção de ganho de produtividade com dados reais.

Após concluir o exercício 1:

1. Releia o registro que você fez. Entenda o que foi feito.

2. Faça uma estimativa honesta: **quanto tempo você teria levado para resolver a mesma tarefa sem nenhum auxílio de IA?** Considere:
   - Tempo de pesquisa (documentação, Stack Overflow, etc.)
   - Tempo de escrita do código
   - Tempo de depuração
   - Tempo de escrever testes

3. Calcule a diferença e o percentual de redução estimado.

4. Reflita: o ganho foi uniforme em todas as etapas? Em quais etapas a IA ajudou mais? Em quais ajudou menos (ou atrapalhou)?

**Nota:** não existe resposta certa aqui. O objetivo é ter um dado concreto, por mais impreciso que seja, para contrastar com a percepção subjetiva de "a IA é sempre mais rápida".

---

### Exercício 3 — Checklist pessoal de revisão

**Objetivo:** transformar os riscos discutidos na seção de conceitos em critérios acionáveis para o seu contexto específico.

Crie um arquivo `checklist-revisao-ia.md` (no seu repositório pessoal de notas ou onde preferir) com sua checklist de "quando revisar a fundo a saída da IA".

A checklist deve ser personalizada para o seu contexto: sua stack, seu tipo de projeto, seu nível de conforto com diferentes domínios. Abaixo um ponto de partida — adapte livremente:

```markdown
# Checklist — Quando revisar a fundo a saída do assistente

## Reviso sempre (revisão mínima)
- [ ] O código executa sem erro?
- [ ] O comportamento bate com o que pedi?
- [ ] O código está aderente ao estilo do projeto?

## Reviso com atenção (revisão padrão)
- [ ] A tarefa envolve lógica de negócio não-trivial?
- [ ] O código vai para um PR que outros revisarão?
- [ ] Toquei em código que não conheço bem?
- [ ] A resposta veio em menos de 10 segundos para uma tarefa complexa? (suspeito de superficialidade)

## Reviso com profundidade (revisão crítica)
- [ ] O código lida com autenticação, autorização ou dados sensíveis?
- [ ] Há chamadas externas (APIs, banco de dados, filesystem)?
- [ ] O código é chamado em produção por muitos usuários?
- [ ] Não entendo completamente o que foi gerado?

## Não aceito sem entender
- [ ] Qualquer alteração em infraestrutura (CI/CD, Docker, permissões)
- [ ] Qualquer código que usa secrets ou credenciais
- [ ] Qualquer migração de banco de dados
```

---

## 🏁 Marco

**Entregável:** um documento de uma página (pode ser em Markdown, Notion, papel digitalizado — o formato não importa) com duas partes:

**Parte 1 — Seu fluxo AI-native documentado**

Descreva em prosa ou diagrama como você integra o assistente de IA no seu ciclo de desenvolvimento. Deve responder:
- Em quais etapas do desenvolvimento você usa IA? (planejamento, implementação, testes, revisão, documentação?)
- Como você decide o que pedir para a IA vs. o que fazer sozinha?
- Como é seu processo de revisão de saídas?

Este documento não é para mostrar para ninguém. É para você ter clareza sobre o que faz hoje e poder comparar com o que fará depois de estudar os módulos seguintes.

**Parte 2 — Medição de impacto em uma tarefa real**

Com base no exercício 2, documente:
- Descrição breve da tarefa escolhida
- Tempo real com IA: [X minutos]
- Estimativa de tempo sem IA: [Y minutos]
- Percentual estimado de redução: [(Y-X)/Y * 100]%
- Principal aprendizado sobre o seu uso de IA nessa tarefa

**Critério de conclusão:** o documento existe e tem as duas partes preenchidas. Não há resposta certa — o valor está na reflexão, não na métrica.

---

## ✅ Autoavaliação

Antes de marcar este módulo como concluído no [PROGRESSO.md](PROGRESSO.md), responda honestamente:

- **Tenho um fluxo documentado?** Consigo descrever, em palavras, como integro a IA no meu ciclo de desenvolvimento — incluindo quando não uso?

- **Sei identificar quando a saída de IA precisa de revisão crítica?** Tenho critérios pessoais (mesmo que informais) para decidir o nível de atenção que dou a cada saída?

- **Medi algum impacto concreto?** Tenho pelo menos um dado numérico (mesmo impreciso) sobre o impacto da IA na minha produtividade em uma tarefa real?

- **Conheço os riscos principais?** Consigo nomear pelo menos três riscos do desenvolvimento AI-native e explicar como mitigá-los na minha prática atual?

Se a resposta for "não" para qualquer uma dessas perguntas, releia a seção de conceitos correspondente e complete o exercício relacionado antes de avançar.

---

> **Próximo módulo:** [02-rag-e-contexto.md](02-rag-e-contexto.md) — RAG e Pipelines de Contexto. Você vai entender como os sistemas de IA recuperam e usam informação externa, e construir seu primeiro pipeline de busca semântica.
