# PDF Q&A Chatbot

A Streamlit app for asking questions about uploaded PDF files. It extracts PDF
text, retrieves relevant passages, and uses a Groq-hosted language model to
answer questions with conversational context.

## Live Demo

[Open the app on Streamlit](https://rag-chat-history-ctqftvand3focyiwywxrre.streamlit.app/)

## Features

- Upload one or more PDF files.
- Split PDF text into chunks and create embeddings with
  `all-MiniLM-L6-v2`.
- Retrieve relevant content using an in-memory Chroma vector store.
- Ask follow-up questions using chat history.
- Generate concise answers with Groq's `openai/gpt-oss-20b` model.

## Requirements

- Python 3.13 or newer
- [uv](https://docs.astral.sh/uv/)
- A Groq API key
- A Hugging Face token (`HF_TOKEN`) for the embeddings setup

This app uses the shared `requirements.txt` at the repository root.

## Setup on Windows

Run these commands from the repository root. Create the `chatbot` environment
only if it does not already exist:

```powershell
uv venv chatbot --python 3.13
uv pip install --python .\chatbot\Scripts\python.exe -r .\requirements.txt
```

Create a `.env` file in the repository root and add your Hugging Face token:

```dotenv
HF_TOKEN=your_hugging_face_token
```

Do not commit `.env` or share your tokens. The app asks for the Groq API key in
its web interface; it is not read from `.env`.

## Run

From the repository root:

```powershell
.\chatbot\Scripts\python.exe -m streamlit run "app.py"
```

Open the local URL printed by Streamlit, enter your Groq API key, upload PDFs,
and ask a question.

## How it works

The app loads the uploaded PDFs, splits their contents into 5,000-character
chunks with 500-character overlap, and creates embeddings for retrieval. The
retrieved passages and conversation history are sent to the Groq model to
produce an answer. Answers are instructed to be no more than three sentences.

The vector store and chat history are held in memory; the app does not configure
persistent storage. A deprecation warning may appear for the
`langchain-community` chat history import; it does not by itself prevent the
app from starting.