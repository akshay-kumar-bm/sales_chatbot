# Sales Chatbot

A configurable AI sales assistant: a business fills in a form describing its product, FAQs, objection handling, sales script, tone, language and call-to-action links, and a second Streamlit app turns that into a chat agent that sells on its behalf.

## What it does
Two Streamlit apps share a JSON file (`sales_chatbot_data.json`):
1. **Input form** (`app_frent_end.py`) - collects product name/description, pricing, FAQs, sales guidelines, tone (Formal/Casual/Friendly), language, and purchase/demo/appointment links, and saves them to the JSON file.
2. **Chatbot** (`app_chatbot.py`) - loads the JSON, builds a system prompt (product info, tone, language, objections, FAQs, sales script, links) and chats with the customer using a Groq-hosted Llama model through LangChain with conversation-buffer memory.
`app.py` appears to be an earlier combined version of both; `demo.ipynb` is the notebook prototype.

## Tech stack
Python, Streamlit, LangChain (`LLMChain`, `ConversationBufferMemory`), `langchain-groq` (model `llama-3.2-11b-vision-preview`).

## Project structure
```
app_frent_end.py        # business input form -> sales_chatbot_data.json
app_chatbot.py          # customer-facing chat UI + LLM chain
app.py                  # combined earlier version
demo.ipynb              # prototype notebook
sales_chatbot_data.json # sample data (EcoClean cleaner example)
requirements.txt        # streamlit, langchain, langchain-groq
```

## Setup and run
```bash
pip install -r requirements.txt
streamlit run app_frent_end.py   # fill in and save business data
streamlit run app_chatbot.py     # start the chatbot
```
Note: the code currently passes the Groq API key as a literal in the source. Before running, change it to read from an environment variable (for example `GROQ_API_KEY`) and use your own key.

## Limitations / future work
- API key is hardcoded (see above); should move to env/`st.secrets`.
- Uses the deprecated `LLMChain`/`ConversationBufferMemory` APIs and a preview model name.
- Objections field is not collected by the form; no persistence of conversations, auth, or tests.
