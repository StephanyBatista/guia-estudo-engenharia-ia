# Módulo 4 — Plataformas e Infraestrutura

> **Posição no currículo:** módulo 4 de 6 — você já construiu RAG e agentes; agora vai empacotá-los como um serviço real.

---

## 🎯 Objetivos de aprendizado

Ao concluir este módulo você será capaz de:

- Empacotar um sistema de IA (agente ou RAG) como um serviço HTTP utilizando FastAPI.
- Expor um endpoint de inferência com contrato claro (entrada/saída tipadas).
- Instrumentar o serviço com logging estruturado, medição de latência e contagem de tokens.
- Implementar cache simples para reduzir chamadas redundantes ao modelo.
- Aplicar tratamento de erros e timeouts para lidar com falhas do modelo sem derrubar o serviço.
- Entender as preocupações centrais de produção: custo, latência e observabilidade.

---

## 🧠 Conceitos-chave

### Servir modelos: local vs. API

Servir um modelo significa disponibilizá-lo para receber requisições e retornar respostas. Localmente, você usa o Ollama, que expõe o modelo como uma API HTTP rodando na sua máquina sem custo de token. Em produção, a alternativa são provedores de API como OpenAI ou Anthropic, que cobram por token e introduzem latência de rede. A vantagem do local-first é o ciclo de desenvolvimento sem custo e sem riscos de privacidade; a desvantagem é que modelos menores (7B–13B) têm qualidade inferior a modelos grandes de API. O LiteLLM e o vLLM são ferramentas que facilitam trocar de provedor sem reescrever código, o que é a prática recomendada em sistemas reais: desenvolva localmente, troque o provedor sem alterar a lógica de negócio.

### Expor via API REST com FastAPI

FastAPI é o padrão de mercado para expor modelos de ML como serviços HTTP em Python: tipagem automática via Pydantic, geração de documentação OpenAPI, suporte nativo a async/await e alto desempenho. O padrão é criar um endpoint `POST /chat` ou `POST /query` que recebe uma `Request` tipada e devolve uma `Response` tipada. Isso garante contrato explícito entre quem chama e quem serve, facilita testes e permite validação de entrada sem código manual. Async é especialmente importante aqui porque chamadas ao modelo são operações de I/O e, com async, o servidor pode atender outras requisições enquanto espera a resposta do LLM.

### Custo e latência: caching, batching e filas

Custo de LLM vem de tokens processados; latência vem principalmente do tempo de geração (time-to-first-token + tempo por token). As estratégias principais para atacar ambos são: (1) **cache de respostas** — para perguntas idênticas ou muito similares, retornar a resposta já calculada sem chamar o modelo de novo; (2) **cache de embeddings** — embeddings são determinísticos, então calcular o mesmo texto duas vezes é desperdício puro; (3) **batching** — agrupar múltiplas requisições e processar juntas aumenta o throughput do modelo; (4) **filas** — em volume alto, uma fila (ex: Redis + worker) desacopla a recepção da requisição do processamento, evitando timeouts por sobrecarga. Localmente, um cache em memória (dicionário Python ou `functools.lru_cache`) já resolve a maioria dos casos de desenvolvimento.

### Observabilidade: logging estruturado, tracing e métricas

Observabilidade é a capacidade de entender o que está acontecendo dentro do sistema a partir de suas saídas externas. Os três pilares são: **logs** — registros de eventos (entrada, saída, latência, tokens usados, erros); **traces** — visibilidade do caminho completo de uma requisição por múltiplos componentes (ex: API → retrieval → LLM → resposta); **métricas** — agregações numéricas ao longo do tempo (p50/p95/p99 de latência, taxa de erro, tokens/segundo). Para desenvolvimento local, logging estruturado em JSON já é suficiente — você consegue filtrar e analisar com `grep` ou qualquer ferramenta de log. Em produção, ferramentas como LangSmith, Langfuse ou Weights & Biases Weave adicionam tracing distribuído. A regra prática: **logue sempre entrada, latência e tokens**; o resto você adiciona quando precisar depurar um problema real.

### Tratamento de erros e timeouts

Modelos falham: o servidor Ollama pode estar indisponível, a resposta pode demorar mais que o aceitável, ou o modelo pode retornar conteúdo malformado. Sem tratamento explícito, esses problemas viram exceções não tratadas que derrubam o serviço ou travam o worker indefinidamente. As práticas essenciais são: (1) **timeout** explícito em toda chamada ao modelo (ex: 30s) — nunca deixe uma chamada sem prazo; (2) **retry com backoff exponencial** para erros transitórios (ex: modelo temporariamente indisponível); (3) **fallback** — se o modelo local falhar, devolver uma mensagem de erro clara em vez de silêncio ou exceção não tratada; (4) **validação de saída** — checar se a resposta tem o formato esperado antes de repassar ao cliente. Com FastAPI, HTTPException é o mecanismo padrão para retornar erros HTTP com mensagem legível.

### Deploy básico: container

Containerizar o serviço com Docker é o passo mínimo para garantir reprodutibilidade: qualquer máquina com Docker consegue rodar exatamente o mesmo ambiente. O `Dockerfile` de uma API FastAPI é simples — imagem Python, instala dependências, copia código, roda `uvicorn`. Localmente, `docker compose` permite subir a API e o Ollama juntos com um único comando. Em produção, o container é a unidade de deploy padrão em Kubernetes, ECS, Cloud Run etc. O conceito importante aqui não é dominar Kubernetes agora, mas entender que empacotar em container é o que separa "funciona na minha máquina" de "funciona em qualquer lugar".

---

## 📚 Recursos

Todos os recursos abaixo estão listados com URLs completas em [`recursos.md`](./recursos.md), na seção **Infraestrutura e MLOps**.

| Recurso | Por quê ler |
|---|---|
| **FastAPI — Documentação oficial** | Referência completa para criar APIs Python; o tutorial "First Steps" cobre 80% do que este módulo usa |
| **Made With ML — Goku Mohandas** | Guia aberto de MLOps do design ao deploy; excelente para desenvolvedoras que querem entender o ciclo completo |
| **Chip Huyen — Blog (huyenchip.com)** | Artigos diretos sobre serving, latência, monitoramento e design de pipelines em produção |
| **LiteLLM — Documentação oficial** | Como unificar chamadas a Ollama, OpenAI e outros sob a mesma interface |
| **vLLM — Documentação oficial** | Engine de serving open-source para quem quiser hospedar modelos com throughput de produção |

---

## ⌨️ Mão na massa

Os quatro exercícios abaixo constroem sobre o agente ou pipeline RAG que você implementou nos módulos anteriores. Se ainda não tem um, use uma chamada simples ao Ollama como ponto de partida.

### Exercício 1 — Expor o agente como API FastAPI

Instale as dependências:

```bash
pip install fastapi uvicorn httpx
```

Crie `api.py`:

```python
import time
import logging
import json
import hashlib
from contextlib import asynccontextmanager
from typing import Optional

import httpx
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

# ---------------------------------------------------------------------------
# Logging estruturado em JSON
# ---------------------------------------------------------------------------

class JsonFormatter(logging.Formatter):
    def format(self, record: logging.LogRecord) -> str:
        payload = {
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "message": record.getMessage(),
        }
        if hasattr(record, "extra"):
            payload.update(record.extra)
        return json.dumps(payload, ensure_ascii=False)

handler = logging.StreamHandler()
handler.setFormatter(JsonFormatter())
logger = logging.getLogger("ai-service")
logger.addHandler(handler)
logger.setLevel(logging.INFO)

# ---------------------------------------------------------------------------
# Cache em memória simples
# ---------------------------------------------------------------------------

_cache: dict[str, str] = {}

def _cache_key(text: str) -> str:
    """Gera chave de cache determinística a partir do texto."""
    return hashlib.sha256(text.encode()).hexdigest()

def cache_get(text: str) -> Optional[str]:
    return _cache.get(_cache_key(text))

def cache_set(text: str, response: str) -> None:
    _cache[_cache_key(text)] = response

# ---------------------------------------------------------------------------
# Schemas de entrada e saída
# ---------------------------------------------------------------------------

class QueryRequest(BaseModel):
    question: str
    context: Optional[str] = None  # contexto RAG opcional

class QueryResponse(BaseModel):
    answer: str
    latency_ms: float
    cached: bool
    model: str

# ---------------------------------------------------------------------------
# Chamada ao Ollama com timeout e retry simples
# ---------------------------------------------------------------------------

OLLAMA_URL = "http://localhost:11434/api/generate"
MODEL_NAME = "llama3.2"       # ajuste para o modelo que você tem localmente
TIMEOUT_SECONDS = 30.0
MAX_RETRIES = 2

async def call_model(prompt: str) -> str:
    """Chama o Ollama com timeout e retry em erros transitórios."""
    payload = {
        "model": MODEL_NAME,
        "prompt": prompt,
        "stream": False,
    }
    last_error: Exception = RuntimeError("Nenhuma tentativa realizada")

    async with httpx.AsyncClient(timeout=TIMEOUT_SECONDS) as client:
        for attempt in range(1, MAX_RETRIES + 1):
            try:
                response = await client.post(OLLAMA_URL, json=payload)
                response.raise_for_status()
                data = response.json()
                return data["response"]
            except httpx.TimeoutException as exc:
                last_error = exc
                logger.warning(
                    "Timeout na chamada ao modelo",
                    extra={"attempt": attempt, "max_retries": MAX_RETRIES},
                )
            except httpx.HTTPStatusError as exc:
                last_error = exc
                logger.error(
                    "Erro HTTP do modelo",
                    extra={"status_code": exc.response.status_code, "attempt": attempt},
                )
                break  # erros 4xx não adianta tentar de novo
            except Exception as exc:
                last_error = exc
                logger.error(
                    "Erro inesperado na chamada ao modelo",
                    extra={"error": str(exc), "attempt": attempt},
                )

    raise last_error

# ---------------------------------------------------------------------------
# Monta o prompt com contexto RAG opcional
# ---------------------------------------------------------------------------

def build_prompt(question: str, context: Optional[str]) -> str:
    if context:
        return (
            f"Use o contexto abaixo para responder à pergunta.\n\n"
            f"Contexto:\n{context}\n\n"
            f"Pergunta: {question}\n"
            f"Resposta:"
        )
    return f"Pergunta: {question}\nResposta:"

# ---------------------------------------------------------------------------
# Aplicação FastAPI
# ---------------------------------------------------------------------------

@asynccontextmanager
async def lifespan(app: FastAPI):
    logger.info("Serviço iniciado", extra={"model": MODEL_NAME})
    yield
    logger.info("Serviço encerrado")

app = FastAPI(
    title="AI Agent Service",
    description="Agente/RAG exposto como API REST",
    version="0.1.0",
    lifespan=lifespan,
)

@app.get("/health")
async def health():
    """Endpoint de saúde — usado por load balancers e orquestradores."""
    return {"status": "ok", "model": MODEL_NAME}

@app.post("/query", response_model=QueryResponse)
async def query(request: QueryRequest):
    """
    Recebe uma pergunta (e contexto RAG opcional) e retorna a resposta do modelo.
    Loga entrada, latência e status de cache a cada requisição.
    """
    prompt = build_prompt(request.question, request.context)
    start = time.perf_counter()
    cached = False

    # Tenta servir do cache antes de chamar o modelo
    answer = cache_get(prompt)
    if answer is not None:
        cached = True
        logger.info("Cache hit", extra={"question_preview": request.question[:80]})
    else:
        try:
            answer = await call_model(prompt)
            cache_set(prompt, answer)
        except httpx.TimeoutException:
            raise HTTPException(
                status_code=504,
                detail="O modelo não respondeu dentro do prazo. Tente novamente.",
            )
        except Exception as exc:
            raise HTTPException(
                status_code=503,
                detail=f"Serviço do modelo indisponível: {exc}",
            )

    latency_ms = (time.perf_counter() - start) * 1000

    # Log estruturado de cada requisição
    logger.info(
        "Requisição processada",
        extra={
            "question_preview": request.question[:80],
            "latency_ms": round(latency_ms, 2),
            "cached": cached,
            "answer_length": len(answer),
        },
    )

    return QueryResponse(
        answer=answer,
        latency_ms=round(latency_ms, 2),
        cached=cached,
        model=MODEL_NAME,
    )
```

Rode o servidor:

```bash
uvicorn api:app --reload --port 8000
```

Teste com `curl`:

```bash
curl -s -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question": "O que é RAG?"}' | python3 -m json.tool
```

Acesse a documentação interativa em `http://localhost:8000/docs`.

---

### Exercício 2 — Verificar o logging estruturado

O código já inclui logging estruturado. Faça duas requisições idênticas e observe a saída no terminal:

- Na primeira, você verá `"cached": false` e a latência real de chamada ao modelo.
- Na segunda, verá `"cached": true` e latência de ~0 ms.

Exemplo de saída esperada no terminal (uma linha por requisição):

```json
{"timestamp": "2026-06-01 10:00:01,123", "level": "INFO", "message": "Requisição processada", "question_preview": "O que é RAG?", "latency_ms": 1842.5, "cached": false, "answer_length": 312}
{"timestamp": "2026-06-01 10:00:03,456", "level": "INFO", "message": "Cache hit", "question_preview": "O que é RAG?"}
{"timestamp": "2026-06-01 10:00:03,460", "level": "INFO", "message": "Requisição processada", "question_preview": "O que é RAG?", "latency_ms": 0.31, "cached": true, "answer_length": 312}
```

Para salvar os logs em arquivo e analisá-los depois, redirecione a saída:

```bash
uvicorn api:app --port 8000 2>&1 | tee requests.log
```

---

### Exercício 3 — Cache de embeddings (bônus)

Se o seu pipeline usa embeddings (módulo RAG), adicione cache para não recalcular o mesmo texto:

```python
from sentence_transformers import SentenceTransformer
import numpy as np

_embed_model = SentenceTransformer("all-MiniLM-L6-v2")
_embed_cache: dict[str, list[float]] = {}

def get_embedding(text: str) -> list[float]:
    """Retorna embedding do cache ou calcula e armazena."""
    key = _cache_key(text)
    if key not in _embed_cache:
        embedding = _embed_model.encode(text, convert_to_numpy=True)
        _embed_cache[key] = embedding.tolist()
    return _embed_cache[key]
```

Chame `get_embedding(text)` em vez de chamar o modelo diretamente. Textos idênticos são calculados uma única vez por sessão.

---

### Exercício 4 — Simular falha e verificar tratamento de erro

Para testar o tratamento de timeout, pare o Ollama enquanto o servidor está rodando:

```bash
# Terminal 1: servidor rodando
uvicorn api:app --port 8000

# Terminal 2: pare o Ollama
ollama stop   # ou pkill ollama

# Terminal 3: faça uma requisição
curl -s -X POST http://localhost:8000/query \
  -H "Content-Type: application/json" \
  -d '{"question": "Teste de falha"}'
```

Você deve receber HTTP 503 com mensagem legível, não um erro 500 genérico nem travamento:

```json
{"detail": "Serviço do modelo indisponível: ..."}
```

Reinicie o Ollama e confirme que o serviço se recupera automaticamente na próxima requisição.

---

### Estrutura do projeto ao final

```
projeto/
├── api.py              # serviço FastAPI (este módulo)
├── rag.py              # pipeline RAG (módulo 2)
├── agent.py            # agente (módulo 3)
├── requirements.txt
└── Dockerfile          # opcional, para containerização
```

**`Dockerfile` mínimo** (opcional, mas recomendado):

```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

CMD ["uvicorn", "api:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
# Build e run local
docker build -t ai-service .
docker run -p 8000:8000 --network host ai-service
```

O `--network host` permite que o container acesse o Ollama rodando na máquina host.

---

## 🏁 Marco

O agente/RAG dos módulos anteriores está empacotado como serviço HTTP: recebe perguntas via `POST /query`, retorna respostas tipadas, loga cada requisição com latência e status de cache, trata timeouts e indisponibilidade do modelo com erros HTTP claros (503/504). O serviço é reiniciável sem perda de estado e pode ser containerizado com um `Dockerfile` simples.

**Entregável concreto:** rodar `uvicorn api:app --port 8000`, fazer três requisições (incluindo uma repetida), e confirmar que os logs mostram cache hit na segunda chamada idêntica e que a parada do Ollama retorna 503 em vez de travar.

---

## ✅ Autoavaliação

Responda honestamente antes de avançar para o próximo módulo:

- **Minha API responde e loga corretamente?**
  Consegui ver no terminal os logs estruturados com latência e status de cache a cada requisição?

- **Sei onde o custo e a latência aparecem?**
  Consigo apontar no código onde o tempo é gasto (chamada ao modelo) e onde o cache elimina essa chamada? Entendo que em produção "custo" seria tokens cobrados no lugar de tempo de CPU?

- **Sei o que mudaria para deploy real?**
  Tenho clareza de que o cache em memória seria substituído por Redis, os logs iriam para um agregador (Datadog, Loki etc.), e o Ollama seria trocado por um provedor de API ou por vLLM — sem alterar a lógica do endpoint?

- **Tratamento de erros está funcionando?**
  Testei a falha do modelo e recebi HTTP 503 com mensagem legível, não uma exceção Python não tratada?

Se todas as respostas forem sim: você está pronto para o módulo de avaliação.
