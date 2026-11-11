# HW5 — Build a university IT helpdesk with LangGraph

**CSS-4007 · Artificial Intelligence**  
**Due:** 8 November 2026, 23:59 (UTC+5)

## Project Description

This project implements a university IT helpdesk using LangGraph.

The application supports:

- Wi‑Fi support requests
- Password recovery requests
- Service status checks
- Helpdesk ticket draft creation
- Conversation memory
- User preference memory

## Python Version
Python 3.14.7

## Installation

Create a virtual environment:

python -m venv .venv

Activate the environment.

Windows:

.venv\Scripts\activate

macOS/Linux:

source .venv/bin/activate

Install dependencies:

pip install -r requirements.txt

## API Key Setup

Copy `.env.example` to `.env`.

Add your Gemini API key to the file.

Example:

GEMINI_API_KEY=your_api_key

## Running the Project

Open a terminal in the project directory.

Start Jupyter Notebook:

python -m notebook

Open and run the notebooks for Part 1, Part 2, and Part 3.

## Memory Storage

Conversation memory is stored in:

- memory.db (SQLite database)

User preferences are stored in:

- user_memory.json

## Resetting Memory

To start with a clean state, delete:

- memory.db
- user_memory.json

This will remove all saved conversations and user preferences.

























## References

- [LangGraph Graph API](https://docs.langchain.com/oss/python/langgraph/graph-api)
- [LangGraph persistence](https://docs.langchain.com/oss/python/langgraph/persistence)
- [LangGraph memory](https://docs.langchain.com/oss/python/langgraph/add-memory)


