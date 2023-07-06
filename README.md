# chat-bot-template-py

[![](https://dcbadge.vercel.app/api/server/nhvCbCtKV?style=flat)](https://discord.gg/nhvCbCtKV)

# Introduction

Welcome to Embedchain Chat Template tutorial. This repository includes the starter code to quickly get a bot running.

In this tutorial, we will create a Naval Ravikant Bot. This bot will have following context from the following sources.

- [Naval Ravikant Joe Rogan Podcast](https://www.youtube.com/watch?v=3qHkcs3kG44)
- [The Almanack of Naval Ravikant](https://navalmanack.s3.amazonaws.com/Eric-Jorgenson_The-Almanack-of-Naval-Ravikant_Final.pdf)
- [Free Markets Provide the Best Feedback from Naval's blog](https://nav.al/feedback)
- [More Compute Power Doesn’t Produce AGI from Naval's blog](https://nav.al/agi)
- Question / Answer Pair:
  - Q: Who is Naval Ravikant?
  - A: Naval Ravikant is an Indian-American entrepreneur and investor.

# Getting Started

## Installation

- First make sure that you have the following installed.

* Python 3 and virtualenv

- Make sure that you have the package cloned locally, using the following commands

```bash
git clone https://github.com/embedchain/chat-bot-template-py.git
cd chat-bot-template-py
```

- Create and activate your virtual environment as follows

```bash
# For Linux Users
virtualenv -p $(which python3) pyenv
source pyenv/bin/activate

# For Windows users
virtualenv pyenv
.\pyenv\Scripts\activate
```

- Now install the required packages using

```bash
pip install -r requirements.txt
```

- We use OpenAI's embedding model to create embeddings for chunks and ChatGPT API as LLM to get answer given the relevant docs. Make sure that you have an OpenAI account and an API key. If you have don't have an API key, you can create one by visiting [this link](https://platform.openai.com/account/api-keys).

- Rename the `sample.env` to `.env` and set your environment variables.

```bash
OPENAI_API_KEY=""
```

## Usage

- Activate your virtual environment

```bash
# For Linux Users
source pyenv/bin/activate

# For Windows Users
.\pyenv\Scripts\activate
```

- Run the development server, using

```bash
python main.py
```

- Open [http://localhost:8000](http://localhost:8000) with your browser to see the result.

- By default we have setup a `Naval Ravikant Chat Bot` app.

- Wait for the data to load completely and then ask any query using the chat box and then click on Submit.

- Your results will be displayed as chats in the chat window

- To customize and create your own bot app, go to `main.py` and enter your own data sources in the load_app() function in the following manner

```python
# Embed Online Resources
chat_bot_app.add("youtube_video", "https://www.youtube.com/watch?v=3qHkcs3kG44")
chat_bot_app.add("pdf_file", "https://navalmanack.s3.amazonaws.com/Eric-Jorgenson_The-Almanack-of-Naval-Ravikant_Final.pdf")
chat_bot_app.add("web_page", "https://nav.al/feedback")
chat_bot_app.add("web_page", "https://nav.al/agi")

# Embed Local Resources
chat_bot_app.add_local("qna_pair", ("Who is Naval Ravikant?", "Naval Ravikant is an Indian-American entrepreneur and investor."))
```

- To change your bot name, change the global variable in the `main.py` file as follows

```python
bot_name="Naval Ravikant"
```

- Now reload or run your app again to see the changes.

## Format supported

We support the following formats:

### Youtube Video

To add any youtube video to your app, use the data_type (first argument to `.add`) as `youtube_video`. Eg:

```python
app.add('youtube_video', 'a_valid_youtube_url_here')
```

### PDF File

To add any pdf file, use the data_type as `pdf_file`. Eg:

```python
app.add('pdf_file', 'a_valid_url_where_pdf_file_can_be_accessed')
```

Note that we do not support password protected pdfs.

### Web Page

To add any web page, use the data_type as `web_page`. Eg:

```python
app.add('web_page', 'a_valid_web_page_url')
```

### Doc File

To add any doc/docx file, use the data_type as `doc_file`. Eg:

```python
app.add('doc_file', 'a_local_doc_file_path')
```

### Text

To supply your own text, use the data_type as `text` and enter a string. The text is not processed, this can be very versatile. Eg:

```python
app.add_local('text', 'Seek wealth, not money or status. Wealth is having assets that earn while you sleep. Money is how we transfer time and wealth. Status is your place in the social hierarchy.')
```

Note: This is not used in the examples because in most cases you will supply a whole paragraph or file, which did not fit.

### QnA Pair

To supply your own QnA pair, use the data_type as `qna_pair` and enter a tuple. Eg:

```python
app.add_local('qna_pair', ("Question", "Answer"))
```

# Tech Stack

embedchain is built on the following stack:

- [Langchain](https://github.com/hwchase17/langchain) as an LLM framework to load, chunk and index data
- [OpenAI's Ada embedding model](https://platform.openai.com/docs/guides/embeddings) to create embeddings
- [OpenAI's ChatGPT API](https://platform.openai.com/docs/guides/gpt/chat-completions-api) as LLM to get answers given the context
- [Chroma](https://github.com/chroma-core/chroma) as the vector database to store embeddings
