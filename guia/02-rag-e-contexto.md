# Módulo 02 — RAG e Pipelines de Contexto

## 🎯 Objetivos de aprendizado

Ao final deste módulo você será capaz de:

- Construir um pipeline RAG (Retrieval-Augmented Generation) completo do zero, desde a ingestão de documentos até a geração da resposta final.
- Entender cada etapa do pipeline e as decisões de design que afetam a qualidade do sistema.
- Avaliar a qualidade do retrieval usando métricas de precisão, recall e relevância.
- Comparar estratégias de chunking e justificar a escolha mais adequada para cada caso de uso.

---

## 🧠 Conceitos-chave

### Por que RAG?

Modelos de linguagem têm dois limites fundamentais: a **janela de contexto** (quantidade de texto que processam de uma vez) e o **conhecimento estático** (treinados até uma data de corte, sem acesso a dados novos). RAG resolve ambos os problemas: em vez de tentar colocar toda a base de conhecimento no prompt, você recupera dinamicamente apenas os trechos mais relevantes para cada pergunta e os injeta no contexto do modelo. O resultado é um sistema que pode responder com base em documentos que o modelo nunca viu durante o treinamento, mantendo respostas fundamentadas e verificáveis — ao contrário das alucinações que surgem quando o modelo "inventa" fatos.

A sigla vem do paper de Lewis et al. (2020): o modelo recupera (R) documentos de um índice externo, aumenta (A) o prompt com esse contexto, e então gera (G) a resposta. Essa separação entre memória externa e capacidade de raciocínio é o que torna RAG tão útil em produção: você atualiza a base de conhecimento sem retreinar o modelo.

### Embeddings e busca por similaridade

Um **embedding** é uma representação numérica de texto na forma de um vetor de alta dimensão (tipicamente 384 a 1536 dimensões). Modelos de embedding são treinados para que textos semanticamente similares fiquem próximos no espaço vetorial — ou seja, "cachorro" e "cão" ficam mais próximos entre si do que "cachorro" e "avião". A similaridade entre dois vetores é medida pelo **cosseno do ângulo** entre eles (cosine similarity): quanto mais próximo de 1, mais similar o significado.

Na prática, você converte tanto os documentos da sua base quanto a pergunta do usuário em embeddings usando o mesmo modelo, e então busca os documentos cujos vetores são mais próximos ao vetor da pergunta. Essa busca é chamada de **busca semântica** — ela encontra documentos relevantes mesmo que não usem exatamente as mesmas palavras da pergunta (ao contrário da busca por palavras-chave). Modelos populares para embeddings em português incluem `paraphrase-multilingual-mpnet-base-v2` e `sentence-transformers/LaBSE`.

### Bancos vetoriais

Um **banco vetorial** (vector store) é um banco de dados especializado em armazenar e buscar embeddings de forma eficiente. Ele mantém os vetores indexados de modo que a busca pelos k vizinhos mais próximos (kNN) seja rápida mesmo com milhões de documentos. O **Chroma** é uma opção open-source que roda localmente sem infraestrutura externa — ideal para desenvolvimento e projetos pessoais. Outros bancos vetoriais comuns incluem FAISS (biblioteca), Weaviate, Qdrant e Pinecone (cloud).

Além dos vetores, o banco armazena **metadados** associados a cada trecho (nome do arquivo, número da página, data de criação etc.), que permitem filtrar resultados e rastrear a origem das respostas. Essa rastreabilidade é fundamental para aplicações que precisam citar fontes.

### Estratégias de chunking

**Chunking** é o processo de dividir documentos grandes em trechos menores antes de criar os embeddings. É uma das decisões mais importantes em RAG porque afeta diretamente a qualidade do retrieval.

Os dois parâmetros principais são **tamanho do chunk** (em caracteres ou tokens) e **overlap** (sobreposição entre chunks consecutivos). Um chunk pequeno (200–500 tokens) captura ideias específicas com mais precisão, mas pode perder contexto ao ser recuperado isoladamente. Um chunk grande (800–1500 tokens) preserva mais contexto, mas o embedding pode diluir o sinal semântico ao misturar muitos assuntos. O overlap (tipicamente 10–20% do tamanho do chunk) evita que informações importantes sejam cortadas exatamente na fronteira entre dois chunks.

Estratégias avançadas incluem chunking por estrutura do documento (parágrafos, seções, títulos), chunking semântico (dividir onde o assunto muda, detectado por modelos) e **parent-child chunking** (indexar chunks pequenos para retrieval preciso, mas injetar o chunk pai maior no prompt para preservar contexto).

### Pipeline: ingestão → indexação → retrieval → geração

O pipeline RAG tem quatro etapas bem definidas:

1. **Ingestão**: carregamento e pré-processamento dos documentos brutos (PDFs, TXTs, HTMLs). Inclui extração de texto, limpeza e divisão em chunks.
2. **Indexação**: geração dos embeddings de cada chunk e armazenamento no banco vetorial com os metadados correspondentes. Esta etapa é executada uma vez (ou quando a base de conhecimento muda).
3. **Retrieval**: dado um query do usuário, gera o embedding do query e busca os top-k chunks mais similares no banco vetorial.
4. **Geração**: monta um prompt que inclui o contexto recuperado e o query original, e envia ao modelo de linguagem para gerar a resposta final.

As etapas 1 e 2 formam o **pipeline offline** (indexação). As etapas 3 e 4 formam o **pipeline online** (inferência em tempo real).

### Montagem de contexto e prompt

A qualidade da resposta depende muito de como o contexto recuperado é formatado no prompt. Um prompt RAG típico tem três partes: instrução de sistema (define o comportamento do modelo), contexto (os trechos recuperados, geralmente delimitados por marcadores claros) e a pergunta do usuário. É importante instruir o modelo explicitamente a basear a resposta apenas no contexto fornecido e a dizer "não sei" quando a informação não estiver disponível — isso reduz alucinações.

A **ordem** dos chunks no contexto também importa: estudos mostram que modelos tendem a usar mais as informações no início e no fim do contexto (o chamado "lost in the middle" problem). Reordenar os chunks do menos relevante para o mais relevante (colocando o mais relevante por último, logo antes da pergunta) pode melhorar os resultados.

### Avaliação de retrieval

Avaliar RAG exige medir dois componentes separados: a qualidade do retrieval e a qualidade da geração. Para o retrieval, as métricas principais são:

- **Precisão@k**: dos k chunks recuperados, qual fração é realmente relevante para a pergunta?
- **Recall@k**: dos chunks relevantes que existem na base, qual fração foi recuperada entre os top-k?
- **MRR (Mean Reciprocal Rank)**: considera a posição do primeiro chunk relevante nos resultados.

Para a geração, métricas como **faithfulness** (a resposta é suportada pelo contexto?) e **answer relevance** (a resposta responde à pergunta?) são usadas em frameworks como RAGAS. No início, a avaliação manual — ler os chunks recuperados e julgar se são relevantes — é a abordagem mais confiável.

---

## 📚 Recursos

Os recursos externos listados abaixo estão catalogados com links em [`recursos.md`](./recursos.md). Consulte esse arquivo para os links atualizados.

| Recurso | Tipo | Por que vale |
|---|---|---|
| Lewis et al. (2020) — "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" | Paper original | Fundação teórica do RAG; leitura do abstract e introdução é suficiente para começar |
| Pinecone Learn — "What is a Vector Database?" e "What is RAG?" | Artigos | Explicações visuais excelentes sobre embeddings e pipeline RAG |
| Documentação oficial do Chroma | Docs | Referência completa para uso local, coleções, metadados e filtros |
| Documentação do Sentence-Transformers | Docs | Referência para escolha de modelos, uso da API e fine-tuning |
| DeepLearning.AI — "LangChain: Chat with Your Data" | Curso gratuito | Implementação prática de RAG com LangChain; os conceitos se transferem diretamente para uso sem framework |
| DeepLearning.AI — "Advanced RAG" | Curso gratuito | Técnicas avançadas: reranking, HyDE, parent-child chunking, avaliação com RAGAS |

---

## ⌨️ Mão na massa

### Pré-requisitos

```bash
pip install sentence-transformers chromadb ollama PyMuPDF
```

Certifique-se de que o Ollama está rodando localmente com um modelo instalado:

```bash
ollama pull llama3.2
ollama serve  # em outro terminal, se não estiver rodando
```

---

### Exercício 1 — Indexar documentos com Sentence-Transformers + Chroma

Crie um arquivo `indexar.py` com o código abaixo. Adapte o caminho para apontar para seus próprios PDFs ou arquivos de texto.

```python
import os
import fitz  # PyMuPDF
import chromadb
from sentence_transformers import SentenceTransformer

# ── Configurações ──────────────────────────────────────────────────────────────
DOCS_DIR = "./docs"          # pasta com seus PDFs ou .txt
CHROMA_DIR = "./chroma_db"   # onde o banco vetorial será salvo
COLLECTION_NAME = "minha_base"
MODELO_EMBEDDING = "paraphrase-multilingual-mpnet-base-v2"  # suporta português

# Estratégia de chunking — ajuste aqui para o Exercício 4
CHUNK_SIZE = 500       # caracteres
CHUNK_OVERLAP = 100    # caracteres de sobreposição


# ── Funções auxiliares ─────────────────────────────────────────────────────────

def extrair_texto_pdf(caminho: str) -> str:
    """Extrai texto de um PDF usando PyMuPDF."""
    doc = fitz.open(caminho)
    texto = ""
    for pagina in doc:
        texto += pagina.get_text()
    return texto


def extrair_texto_txt(caminho: str) -> str:
    """Lê um arquivo de texto simples."""
    with open(caminho, "r", encoding="utf-8") as f:
        return f.read()


def carregar_documentos(pasta: str) -> list[dict]:
    """Carrega todos os PDFs e TXTs de uma pasta."""
    documentos = []
    for nome_arquivo in os.listdir(pasta):
        caminho = os.path.join(pasta, nome_arquivo)
        if nome_arquivo.endswith(".pdf"):
            texto = extrair_texto_pdf(caminho)
        elif nome_arquivo.endswith(".txt"):
            texto = extrair_texto_txt(caminho)
        else:
            continue  # ignora outros formatos
        documentos.append({"nome": nome_arquivo, "texto": texto})
        print(f"  Carregado: {nome_arquivo} ({len(texto)} caracteres)")
    return documentos


def dividir_em_chunks(texto: str, tamanho: int, overlap: int) -> list[str]:
    """
    Divide o texto em chunks com sobreposição.
    Estratégia simples baseada em caracteres.
    """
    chunks = []
    inicio = 0
    while inicio < len(texto):
        fim = inicio + tamanho
        chunk = texto[inicio:fim]
        if chunk.strip():  # ignora chunks vazios
            chunks.append(chunk)
        inicio += tamanho - overlap  # avança menos que o tamanho para criar overlap
    return chunks


# ── Pipeline de indexação ──────────────────────────────────────────────────────

def indexar(docs_dir: str, chroma_dir: str, collection_name: str):
    print("Carregando modelo de embedding...")
    modelo = SentenceTransformer(MODELO_EMBEDDING)

    print("Conectando ao Chroma...")
    cliente = chromadb.PersistentClient(path=chroma_dir)

    # Apaga a coleção se já existir (útil para re-indexar)
    try:
        cliente.delete_collection(collection_name)
        print(f"Coleção '{collection_name}' anterior removida.")
    except Exception:
        pass

    colecao = cliente.create_collection(
        name=collection_name,
        metadata={"hnsw:space": "cosine"}  # usa similaridade de cosseno
    )

    print(f"\nCarregando documentos de '{docs_dir}'...")
    documentos = carregar_documentos(docs_dir)

    if not documentos:
        print("Nenhum documento encontrado. Crie a pasta 'docs/' e adicione PDFs ou TXTs.")
        return

    todos_chunks = []
    todos_metadados = []
    todos_ids = []

    id_counter = 0
    for doc in documentos:
        chunks = dividir_em_chunks(doc["texto"], CHUNK_SIZE, CHUNK_OVERLAP)
        print(f"  {doc['nome']}: {len(chunks)} chunks gerados")
        for i, chunk in enumerate(chunks):
            todos_chunks.append(chunk)
            todos_metadados.append({
                "fonte": doc["nome"],
                "chunk_index": i,
                "chunk_size": CHUNK_SIZE,
                "overlap": CHUNK_OVERLAP,
            })
            todos_ids.append(f"doc_{id_counter}")
            id_counter += 1

    print(f"\nGerando embeddings para {len(todos_chunks)} chunks...")
    # encode() retorna um numpy array; Chroma aceita listas
    embeddings = modelo.encode(todos_chunks, show_progress_bar=True).tolist()

    print("Salvando no Chroma...")
    # Chroma aceita lotes; inserimos tudo de uma vez
    colecao.add(
        ids=todos_ids,
        documents=todos_chunks,
        embeddings=embeddings,
        metadatas=todos_metadados,
    )

    print(f"\nIndexação concluída! {colecao.count()} chunks armazenados em '{chroma_dir}'.")


if __name__ == "__main__":
    os.makedirs(DOCS_DIR, exist_ok=True)
    os.makedirs(CHROMA_DIR, exist_ok=True)
    indexar(DOCS_DIR, CHROMA_DIR, COLLECTION_NAME)
```

**O que observar**: após rodar, explore o diretório `chroma_db/` — você verá os arquivos SQLite e binários que o Chroma usa. Tente adicionar um documento novo e re-indexar para ver o processo completo.

---

### Exercício 2 — Implementar a função de retrieval e inspecionar os chunks

```python
import chromadb
from sentence_transformers import SentenceTransformer

CHROMA_DIR = "./chroma_db"
COLLECTION_NAME = "minha_base"
MODELO_EMBEDDING = "paraphrase-multilingual-mpnet-base-v2"
TOP_K = 3  # quantos chunks recuperar


def conectar_colecao(chroma_dir: str, collection_name: str):
    cliente = chromadb.PersistentClient(path=chroma_dir)
    return cliente.get_collection(collection_name)


def recuperar_chunks(
    query: str,
    colecao,
    modelo: SentenceTransformer,
    top_k: int = TOP_K,
) -> list[dict]:
    """
    Gera o embedding da query e busca os top_k chunks mais similares.
    Retorna lista de dicts com texto, metadados e score de distância.
    """
    embedding_query = modelo.encode(query).tolist()

    resultados = colecao.query(
        query_embeddings=[embedding_query],
        n_results=top_k,
        include=["documents", "metadatas", "distances"],
    )

    chunks_recuperados = []
    for i in range(len(resultados["ids"][0])):
        chunks_recuperados.append({
            "id": resultados["ids"][0][i],
            "texto": resultados["documents"][0][i],
            "metadados": resultados["metadatas"][0][i],
            # Chroma com cosseno retorna distância (0=idêntico, 2=oposto)
            # Convertemos para similaridade: 1 - distância/2
            "similaridade": round(1 - resultados["distances"][0][i] / 2, 4),
        })

    return chunks_recuperados


def inspecionar_retrieval(query: str):
    print(f"\nQuery: '{query}'")
    print("=" * 60)

    modelo = SentenceTransformer(MODELO_EMBEDDING)
    colecao = conectar_colecao(CHROMA_DIR, COLLECTION_NAME)

    chunks = recuperar_chunks(query, colecao, modelo, top_k=TOP_K)

    for i, chunk in enumerate(chunks, 1):
        print(f"\n--- Chunk {i} (similaridade: {chunk['similaridade']}) ---")
        print(f"Fonte: {chunk['metadados']['fonte']} | chunk #{chunk['metadados']['chunk_index']}")
        print(f"Texto:\n{chunk['texto'][:300]}...")  # mostra os primeiros 300 chars
    
    return chunks


if __name__ == "__main__":
    # Experimente diferentes queries para entender o que o retrieval captura
    perguntas_teste = [
        "Qual é o prazo de entrega?",
        "Como funciona o processo de reembolso?",
        "Quais são os requisitos técnicos?",
    ]
    for pergunta in perguntas_teste:
        inspecionar_retrieval(pergunta)
```

**O que observar**: analise os scores de similaridade. Um score acima de 0.7 geralmente indica boa relevância. Observe se os chunks recuperados realmente respondem à pergunta ou se contêm apenas palavras similares sem contexto útil (falso positivo semântico).

---

### Exercício 3 — Montar o prompt com contexto e gerar resposta via Ollama

```python
import ollama
import chromadb
from sentence_transformers import SentenceTransformer

CHROMA_DIR = "./chroma_db"
COLLECTION_NAME = "minha_base"
MODELO_EMBEDDING = "paraphrase-multilingual-mpnet-base-v2"
MODELO_LLM = "llama3.2"  # modelo instalado no Ollama
TOP_K = 3


def montar_prompt(query: str, chunks: list[dict]) -> str:
    """
    Monta o prompt RAG com o contexto recuperado.
    A instrução explícita de 'baseie-se apenas no contexto' reduz alucinações.
    """
    contexto = "\n\n---\n\n".join(
        f"[Fonte: {c['metadados']['fonte']}]\n{c['texto']}"
        for c in chunks
    )

    prompt = f"""Você é um assistente preciso e útil. Responda à pergunta do usuário
baseando-se EXCLUSIVAMENTE no contexto fornecido abaixo.
Se a informação necessária não estiver no contexto, diga claramente:
"Não encontrei essa informação nos documentos disponíveis."
Não invente informações.

=== CONTEXTO ===
{contexto}
=== FIM DO CONTEXTO ===

Pergunta: {query}

Resposta:"""
    return prompt


def rag_pipeline(query: str) -> str:
    """Pipeline RAG completo: recupera contexto e gera resposta."""
    # 1. Carregar modelo e banco
    modelo_emb = SentenceTransformer(MODELO_EMBEDDING)
    cliente = chromadb.PersistentClient(path=CHROMA_DIR)
    colecao = cliente.get_collection(COLLECTION_NAME)

    # 2. Retrieval: buscar chunks relevantes
    embedding_query = modelo_emb.encode(query).tolist()
    resultados = colecao.query(
        query_embeddings=[embedding_query],
        n_results=TOP_K,
        include=["documents", "metadatas", "distances"],
    )

    chunks = [
        {
            "texto": resultados["documents"][0][i],
            "metadados": resultados["metadatas"][0][i],
            "similaridade": round(1 - resultados["distances"][0][i] / 2, 4),
        }
        for i in range(len(resultados["ids"][0]))
    ]

    # 3. Montar prompt com contexto
    prompt = montar_prompt(query, chunks)

    # 4. Gerar resposta via Ollama
    print(f"\nQuery: {query}")
    print(f"Chunks recuperados: {len(chunks)} (similaridades: {[c['similaridade'] for c in chunks]})")
    print("\nGerando resposta...\n")

    resposta = ollama.chat(
        model=MODELO_LLM,
        messages=[{"role": "user", "content": prompt}],
    )

    texto_resposta = resposta["message"]["content"]
    print(f"Resposta:\n{texto_resposta}")

    # 5. Exibir fontes usadas
    fontes = list({c["metadados"]["fonte"] for c in chunks})
    print(f"\nFontes consultadas: {', '.join(fontes)}")

    return texto_resposta


if __name__ == "__main__":
    query = input("Digite sua pergunta: ")
    rag_pipeline(query)
```

**O que observar**: compare a resposta com e sem o contexto (experimente remover o contexto do prompt e ver o que o modelo responde). Observe quando o modelo "inventa" informações versus quando se limita ao contexto fornecido.

---

### Exercício 4 — Comparar duas estratégias de chunking

Este exercício mostra como o tamanho do chunk afeta a qualidade do retrieval. Você vai indexar os mesmos documentos duas vezes com configurações diferentes e comparar os resultados.

```python
import chromadb
import fitz
import os
from sentence_transformers import SentenceTransformer
import ollama

DOCS_DIR = "./docs"
MODELO_EMBEDDING = "paraphrase-multilingual-mpnet-base-v2"
MODELO_LLM = "llama3.2"

# Duas estratégias para comparar
ESTRATEGIAS = {
    "chunks_pequenos": {"tamanho": 200, "overlap": 50},
    "chunks_grandes": {"tamanho": 800, "overlap": 150},
}


def extrair_textos(pasta: str) -> list[dict]:
    documentos = []
    for nome in os.listdir(pasta):
        caminho = os.path.join(pasta, nome)
        if nome.endswith(".pdf"):
            doc = fitz.open(caminho)
            texto = "".join(p.get_text() for p in doc)
        elif nome.endswith(".txt"):
            with open(caminho, encoding="utf-8") as f:
                texto = f.read()
        else:
            continue
        documentos.append({"nome": nome, "texto": texto})
    return documentos


def dividir_em_chunks(texto: str, tamanho: int, overlap: int) -> list[str]:
    chunks, inicio = [], 0
    while inicio < len(texto):
        chunk = texto[inicio : inicio + tamanho]
        if chunk.strip():
            chunks.append(chunk)
        inicio += tamanho - overlap
    return chunks


def criar_colecao(cliente, nome: str, documentos: list[dict], modelo, tamanho: int, overlap: int):
    """Cria (ou recria) uma coleção com a estratégia de chunking especificada."""
    try:
        cliente.delete_collection(nome)
    except Exception:
        pass

    colecao = cliente.create_collection(nome, metadata={"hnsw:space": "cosine"})

    ids, texts, metas = [], [], []
    for doc in documentos:
        for i, chunk in enumerate(dividir_em_chunks(doc["texto"], tamanho, overlap)):
            ids.append(f"{doc['nome']}_{i}")
            texts.append(chunk)
            metas.append({"fonte": doc["nome"], "chunk_index": i, "tamanho": tamanho})

    embeddings = modelo.encode(texts, show_progress_bar=False).tolist()
    colecao.add(ids=ids, documents=texts, embeddings=embeddings, metadatas=metas)
    print(f"  '{nome}': {len(ids)} chunks indexados (tamanho={tamanho}, overlap={overlap})")
    return colecao


def montar_prompt_simples(query: str, chunks_texto: list[str]) -> str:
    contexto = "\n\n---\n\n".join(chunks_texto)
    return f"""Responda à pergunta baseando-se APENAS no contexto abaixo.
Se não encontrar a informação, diga "Não encontrei nos documentos."

CONTEXTO:
{contexto}

Pergunta: {query}
Resposta:"""


def comparar_estrategias(query: str):
    print(f"\n{'='*60}")
    print(f"Query: '{query}'")
    print('='*60)

    modelo = SentenceTransformer(MODELO_EMBEDDING)
    documentos = extrair_textos(DOCS_DIR)

    if not documentos:
        print("Nenhum documento encontrado em ./docs/")
        return

    cliente = chromadb.PersistentClient(path="./chroma_comparacao")

    print("\nIndexando com duas estratégias...")
    colecoes = {}
    for nome_estrategia, params in ESTRATEGIAS.items():
        colecoes[nome_estrategia] = criar_colecao(
            cliente, nome_estrategia, documentos, modelo,
            params["tamanho"], params["overlap"]
        )

    emb_query = modelo.encode(query).tolist()

    for nome_estrategia, colecao in colecoes.items():
        params = ESTRATEGIAS[nome_estrategia]
        print(f"\n{'─'*40}")
        print(f"Estratégia: {nome_estrategia} (tamanho={params['tamanho']}, overlap={params['overlap']})")

        resultado = colecao.query(
            query_embeddings=[emb_query],
            n_results=3,
            include=["documents", "distances"],
        )

        chunks_recuperados = resultado["documents"][0]
        distancias = resultado["distances"][0]
        similaridades = [round(1 - d / 2, 4) for d in distancias]

        print(f"Similaridades dos chunks recuperados: {similaridades}")
        print(f"Tamanho médio dos chunks: {sum(len(c) for c in chunks_recuperados) // len(chunks_recuperados)} chars")

        prompt = montar_prompt_simples(query, chunks_recuperados)
        resposta = ollama.chat(
            model=MODELO_LLM,
            messages=[{"role": "user", "content": prompt}],
        )
        print(f"\nResposta:\n{resposta['message']['content']}")

    print(f"\n{'='*60}")
    print("Compare as respostas acima e reflita:")
    print("- Qual estratégia trouxe chunks mais informativos?")
    print("- A resposta com chunks menores foi mais precisa ou mais fragmentada?")
    print("- A resposta com chunks maiores teve mais contexto ou mais ruído?")


if __name__ == "__main__":
    # Use uma pergunta específica sobre seu conteúdo
    query = input("Digite uma pergunta sobre seus documentos: ")
    comparar_estrategias(query)
```

**O que observar e registrar**:

| Critério | Chunks pequenos (200 chars) | Chunks grandes (800 chars) |
|---|---|---|
| Precisão do chunk recuperado | | |
| Contexto preservado | | |
| Qualidade da resposta final | | |
| Casos onde é melhor | Perguntas factuais diretas | Perguntas que precisam de contexto |

Documente suas observações. Não há resposta certa universal — a melhor estratégia depende do tipo de documento e das perguntas típicas do seu caso de uso.

---

## 🏁 Marco

Um pipeline RAG completamente funcional rodando localmente, capaz de:

- Ingerir seus próprios documentos (PDFs e/ou arquivos de texto) e indexá-los com embeddings multilíngues no Chroma.
- Recuperar os chunks mais relevantes para qualquer pergunta feita em português.
- Gerar respostas fundamentadas nos seus documentos via Ollama, sem nenhuma chamada a APIs externas ou custos.
- Comparar o comportamento com diferentes estratégias de chunking e justificar a escolha.

Quando você conseguir fazer uma pergunta sobre o conteúdo dos seus documentos e receber uma resposta correta e rastreável até a fonte, o marco está alcançado.

---

## ✅ Autoavaliação

Responda honestamente antes de seguir para o próximo módulo:

- Sei explicar cada etapa do pipeline (ingestão, indexação, retrieval, geração) sem consultar anotações?
- Consigo justificar minha escolha de chunking para o meu caso de uso específico — tamanho, overlap e por quê?
- Sei avaliar se o retrieval trouxe contexto relevante olhando os scores de similaridade e lendo os chunks recuperados?
- Sei identificar quando uma resposta do modelo veio do contexto versus quando pode ser alucinação?
- Consigo dizer quais perguntas o meu RAG responde bem e quais ele ainda responde mal — e tenho hipóteses sobre por quê?

Se alguma resposta for "não" ou "parcialmente", volte aos exercícios antes de avançar. RAG é a base de grande parte das aplicações de IA em produção — vale a pena solidificar aqui.
