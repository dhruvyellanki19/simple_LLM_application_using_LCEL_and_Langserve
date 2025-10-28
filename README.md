# Simple Translation API with LCEL & LangServe

A beginner-friendly project that builds a translation API using **LangChain Expression Language (LCEL)** and **LangServe**.

## What This Project Does

This is a **language translation service** that:
- Takes text in any language and translates it to another language
- Uses Groq's LLaMA model for fast, free AI translation
- Provides both a Jupyter notebook for learning and a REST API for production

## Project Files

```
├── LCEL.ipynb          # Interactive tutorial showing LCEL step-by-step
├── app_server.py       # Translation API server  
├── requirements.txt    # All needed Python packages
└── .env               # Your API key (you create this)
```

## Quick Setup

1. **Get API Key**: Sign up at [Groq Console](https://console.groq.com/) (free)

2. **Create `.env` file**:
   ```
   GROQ_API_KEY=your_key_here
   ```

3. **Activate environment & install**:
   ```bash
   source LCEL_venv/bin/activate
   pip install -r requirements.txt
   ```

## How to Run

###  Learning Mode 
```bash
jupyter notebook LCEL.ipynb
```
Step-by-step tutorial showing how LCEL works.

### API Server
```bash
python app_server.py
```
Starts translation API at `http://localhost:8000`

## Understanding LCEL (The Magic)

**LCEL** lets you build AI apps like connecting LEGO blocks using the `|` operator:

```python
# Instead of complex code, LCEL makes it simple:
chain = prompt | model | output_parser

# This means: prompt → AI model → clean output
result = chain.invoke({"language": "Spanish", "text": "Hello"})
```

## What is LangServe?

**LangServe** is a tool that automatically converts your LCEL chain into a web API. Think of it as a magic wrapper:

### Without LangServe (Manual API):
```python
# You'd need to write lots of FastAPI code manually
@app.post("/translate")
def translate(request: TranslateRequest):
    # Handle input validation
    # Call your chain
    # Format response
    # Handle errors
    return response
```

### With LangServe (Automatic):
```python
# Just one line does everything!
add_routes(app, chain, path="/chain")
```

### How LangServe Works in Our Project:

1. **Takes Your Chain**: Uses our `prompt | model | parser` translation chain
2. **Creates Endpoints**: Automatically makes `/chain/invoke`, `/chain/playground/`, etc.
3. **Handles Everything**: Input validation, error handling, response formatting
4. **Adds Playground**: Interactive UI to test your API without code

So instead of writing 50+ lines of API code, LangServe does it all with one function call!

## What Each File Does

### `LCEL.ipynb` - Learning Tutorial
Shows you how to build the translation chain step by step:
1. Connect to Groq AI model
2. Create translation prompts  
3. Parse AI responses
4. Chain everything together

### `app_server.py` - Production API
Uses **LangServe** to turn your LCEL chain into a web API:
- Creates a FastAPI server
- Adds your translation chain as an endpoint with `add_routes()`
- **Input**: `{"language": "French", "text": "Good morning"}`
- **Output**: `"Bonjour"`
- **Auto-generates**: API docs, playground UI, validation

## Test the API

**Using the playground** (easiest):
Go to `http://localhost:8000/chain/playground/`

**Using curl**:
```bash
curl -X POST "http://localhost:8000/chain/invoke" \
  -H "Content-Type: application/json" \
  -d '{"input": {"language": "French", "text": "Hello world"}}'
```

## Key Concepts

- **LCEL Chain**: `prompt | model | parser` - data flows left to right  
- **LangServe**: Automatically converts your chain into REST API with one line of code
- **FastAPI Integration**: LangServe builds on FastAPI for high performance
- **Auto-endpoints**: Creates `/invoke`, `/playground/`, `/docs` endpoints automatically
- **Groq**: Fast, free AI model (LLaMA 3.1)

## Why This Matters

This project teaches you to:
- Build AI apps with minimal code
- Create reusable AI workflows
- Deploy AI models as APIs
- Use free, fast AI models

