# ai-research-assistant
# 🔬 AI Research Assistant

> Um agente de IA multi-ferramenta para exploração científica — análise profunda de papers locais, varredura de novos estudos e clarificação de conceitos técnicos.

---

## 🗺️ Arquitetura do Sistema

```
┌────────────────────────────────────────────────────────────────┐
│                                                                │
│   ╭──────────╮        ╭───────────────────╮                    │
│   │          │        │                   │                    │
│   │  Query   │───────▶│   ReAct Agent     │                    │
│   │  usuário │        │   (Groq LLM)      │                    │
│   │          │        │                   │                    │
│   ╰──────────╯        ╰─────────┬─────────╯                    │
│                                 │                              │
│              ┌──────────────────┼──────────────────┐           │
│              │                  │                  │           │
│              ▼                  ▼                  ▼           │
│   ╭──────────────────╮ ╭────────────────╮ ╭──────────────────╮ │
│   │  📄 RAG Local    │ │ 🔍 arXiv Search│ │  📖 Definition   │ │
│   │                  │ │                │ │                  │ │
│   │  Responde sobre  │ │  Busca papers  │ │  Define termos   │ │
│   │  seus PDFs       │ │  públicos      │ │  técnicos        │ │
│   │                  │ │                │ │                  │ │
│   │  VectorStore +   │ │  arXiv API     │ │  Wikipedia API   │ │
│   │  Ollama Embed    │ │  (sem chave)   │ │  (sem chave)     │ │
│   ╰──────────────────╯ ╰────────────────╯ ╰──────────────────╯ │
│              │                                                 │
│              ▼                                                 │
│   ╭──────────────────╮                                         │
│   │  📁 /data        │                                         │
│   │  paper1.pdf      │                                         │
│   │  paper2.pdf      │                                         │
│   ╰──────────────────╯                                         │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

> **Modelo hub-and-spoke:** O agente é o orquestrador central. Ele raciocina qual ferramenta usar (ou nenhuma) e sintetiza a resposta final.

---

## ✨ Funcionalidades

| Capacidade | Ferramenta | Descrição |
|---|---|---|
| **Análise profunda** | `local_rag_tool` | Responde perguntas sobre PDFs privados via RAG |
| **Varredura de horizonte** | `arxiv_search_tool` | Encontra papers recentes no arXiv.org |
| **Clarificação de conceitos** | `definition_tool` | Define termos técnicos via Wikipedia |

---

## 📁 Estrutura do Projeto

```
/ai-research-assistant
├── /data
│   ├── paper1.pdf
│   └── paper2.pdf
├── /tools
│   ├── __init__.py
│   ├── rag_tool.py
│   └── api_tools.py
├── main.py
├── .env
└── requirements.txt
```

---

## ⚙️ Setup

### 1. Pré-requisitos

- Python 3.9+
- [Ollama](https://ollama.ai) rodando localmente com modelo de embedding (ex: `nomic-embed-text`)
- Conta Groq com API Key (gratuita)

### 2. Instalar dependências

```bash
pip install -r requirements.txt
```

`requirements.txt`:
```
llama-index
llama-index-llms-groq
llama-index-embeddings-ollama
python-dotenv
arxiv
wikipedia
```

### 3. Configurar variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto:

```env
GROQ_API_KEY="sua-chave-groq-aqui"
```

### 4. Adicionar seus papers

Coloque seus arquivos PDF na pasta `/data`.

### 5. Rodar o agente

```bash
python main.py
```

---

## 🛠️ Ferramentas em Detalhe

### 📄 Local RAG Tool
- **Arquivo:** `tools/rag_tool.py`
- Indexa PDFs locais com `VectorStoreIndex` do LlamaIndex
- Usa `OllamaEmbedding` para embeddings locais (sem custo, sem dados na nuvem)
- Responde perguntas contextuais sobre o conteúdo dos papers

### 🔍 arXiv Search Tool
- **Arquivo:** `tools/api_tools.py`
- Usa a biblioteca `arxiv` (sem necessidade de API key)
- Busca por palavras-chave ou nome de autores
- Retorna títulos, resumos e links dos papers mais relevantes

### 📖 Definition Tool
- **Arquivo:** `tools/api_tools.py`
- Usa a biblioteca `wikipedia`
- Fornece definições concisas de termos técnicos científicos

---

## 🧠 System Prompt do Agente

O comportamento do agente é guiado pelo seguinte prompt:

```
Você é um assistente de pesquisa especializado em IA. Seu objetivo é ajudar 
usuários respondendo suas perguntas com precisão e metodologia.

Você tem três ferramentas à sua disposição:
- local_rag_tool: Use para responder sobre os papers fornecidos pelo usuário.
- arxiv_search_tool: Use para encontrar novos papers no banco público do arXiv.
- definition_tool: Use para definir termos técnicos.

Processo de decisão:
1. Analise a query do usuário para entender a intenção.
2. Examine cada ferramenta e determine qual é a mais relevante.
3. Se nenhuma ferramenta for adequada, responda com seu conhecimento geral.

Pense passo a passo e explique seu raciocínio ao escolher cada ferramenta.
```

---

## 🗺️ Roadmap

- [x] Estrutura do projeto e dependências
- [x] Local RAG Tool
- [x] arXiv Search Tool
- [x] Definition Tool
- [x] ReAct Agent com Groq
- [ ] Interface web (Streamlit/Gradio)
- [ ] Suporte a múltiplas coleções de papers
- [ ] Memória de conversação persistente

---

## 📄 Licença

MIT License — sinta-se livre para usar, modificar e distribuir.

---

*Desenvolvido como parte do curso **Agentic RAG** — PalancaCode* 🚀
