📘 Project Overview

This project builds an AI-powered Fact Checking System that verifies user claims using:

* Government-sourced factual data (PIB RSS feeds)
* Sentence-transformer embeddings for similarity search
* FAISS vector database for retrieval
* spaCy for claim extraction
* Groq LLaMA-3.1/3.3 70B for final fact verification
* Gradio UI for interactive usage

The system collects factual statements from authentic government RSS sources, embeds them, retrieves the most relevant evidence when a user submits a claim, and finally uses an LLM to determine whether the claim is True, False, or Unverifiable.

📑 Table of Contents

1. Project Overview
2. Features
3. Technology Stack
4. Why These Technologies
5. System Architecture
6. Environment Setup
7. API Keys & Authentication
8. Hugging Face Token
9. Groq API Key
10. Installation Steps
11. Running the Full Pipeline
12. Using the Gradio App
13. Troubleshooting
14. Future Improvements


🚀 Features

* Fetches real-time factual data from PIB (Press Information Bureau) RSS feeds
* Cleans and compiles data into a structured CSV database
* Uses all-MiniLM-L6-v2 to generate dense vector embeddings of facts
* Fast vector similarity search using FAISS
* Automatic claim extraction via spaCy’s NLP pipeline
* Uses Groq's LLaMA-3.1/3.3 70B to evaluate claim truthfulness
* Provides structured JSON outputs with:
    1. verdict
    2. confidence
    3. reasoning
    4. evidence used
* Clean and interactive Gradio UI for public use
* Fully modular for future dataset additions and model upgrades

🧠 Technology Stack
Core Components
Component	                    Technology Used	Purpose
Claim extraction	            spaCy (en_core_web_md)	Extract sentences, entities, and keywords
Embeddings	                    sentence-transformers / MiniLM	Create dense vector representations
Vector store	                FAISS	Fast similarity search
Verification LLM	            Groq (LLaMA 3.1 / 3.3 70B)	Final verdict generation
UI	                            Gradio	Interactive front-end
Data source	                    PIB RSS Feeds	Verified government releases

💡 Why These Technologies
🔹 SentenceTransformers (MiniLM-L6-v2)
* Lightweight, fast, 384-dimensional embeddings
* High accuracy for semantic similarity tasks
* Works well with large datasets

🔹 FAISS

* Industry-standard vector store used by Meta, NVIDIA, OpenAI
* Blazing fast ANN (approximate nearest neighbor) search
* Runs fully in-memory without external services

🔹 spaCy

* Efficient NLP pipeline for sentence segmentation, POS tagging, and entity extraction
* Medium model (en_core_web_md) gives a good balance of accuracy & speed

🔹 Groq LLaMA Models

* Groq LPU hardware gives insanely fast inference (10–500 tokens/ms)
* Free API tier available
* More deterministic than OpenAI for structured JSON outputs

🔹 Gradio

* Easiest way to build production-ready ML apps
* Supports sliders, textboxes, examples, themes

🏗 System Architecture
     ┌──────────────────────────────┐
     │ 1. Fetch PIB RSS Feeds       │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 2. Parse & Clean Feed Data   │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 3. Store in CSV Database     │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 4. Chunk Text & Create       │
     │    Embeddings (MiniLM)       │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 5. Build FAISS Index         │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 6. User Inputs Claim         │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 7. Extract Claim (spaCy)     │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 8. Retrieve Top-k Evidence   │
     │    (FAISS)                   │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 9. Verify with Groq LLM      │
     └───────────────┬──────────────┘
                     ▼
     ┌──────────────────────────────┐
     │ 10. Display Verdict (UI)     │
     └──────────────────────────────┘

🛠 Environment Setup
Recommended environments:

* Google Colab (preferred — GPU not required)
Local system with:
    * Python 3.9 – 3.11
    * 8 GB RAM minimum

🔐 API Keys & Authentication

1️⃣ Generate Hugging Face Token
This token allows you to download embedding models.
Steps:

1. Visit: https://huggingface.co/settings/tokens
2. Click New Token
3. Set permissions → Read
4. Copy the token
5. Add to environment:
6. export HF_TOKEN="your_hf_token_here"

2️⃣ Generate Groq API Key
Required for LLaMA-3 inference.
Steps:

1. Go to https://console.groq.com/keys
2. Click Create API Key
3. Copy the key
4. Add to environment:
5. export GROQ_API_KEY="your_groq_api_key_here"

📦 Installation Steps

Inside your notebook, run:

!pip install -q sentence-transformers faiss-cpu spacy gradio pandas groq huggingface_hub feedparser beautifulsoup4 requests lxml
!python -m spacy download en_core_web_md

▶️ Running the Full Pipeline

Once dependencies are installed:

Run Cell 1–7 to fetch and build the factual dataset.

Run Cell 8 to embed and index facts.

Run Cell 9–12 for claim extraction, retrieval, and LLM verification.

Run Cell 13 to test the fact checker.

Run Cell 14 to launch the Gradio UI.

🌐 Launching the App

After running Cell 14:
A Gradio link appears
Open it in a browser
Enter any claim

The system will:
Extract claim
Retrieve top evidence
Evaluate via LLaMA on Groq
Provide verdict + reasoning

Example output:

✅ VERDICT: True
Confidence: 91%
Reasoning: The retrieved evidence confirms...
1. Evidence text...
2. Evidence text...

❗ Troubleshooting
Issue	                        Solution
FAISS index shape mismatch	    Ensure embeddings are all float32
Groq API Error	                Check your API key + billing limits
Cannot download HF model	    Ensure HF token has read permission
RSS feeds return 403	        Add custom user-agent (already included)
Very few facts extracted	    PIB sometimes rate-limits — wait 10 minutes

🚀 Future Improvements

Add multilingual fact checking
Integrate NewsAPI, LokSabha Debates, RTI databases
Add web search retrieval (Bing / Tavily)
Reinforcement Learning for Chain-of-Thought verification
Weighted evidence scoring instead of simple L2 distance
