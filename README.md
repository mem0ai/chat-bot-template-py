# chat-bot-template-py

[![](https://dcbadge.vercel.app/api/server/nhvCbCtKV?style=flat)](https://discord.gg/nhvCbCtKV)

This is a replit instance of chat-bot-template-py

chat-bot-template-py is a frontend app created in Flask powered by [embedchain](https://github.com/embedchain/embedchain). embedchain is a framework to easily create LLM powered bots over any dataset. If you want a javascript version, check out [embedchain-js](https://github.com/embedchain/embedchainjs)

It abstracts the entire process of loading dataset, chunking it, creating embeddings and then storing in vector database.

You can add a single or multiple dataset using `.add` and `.add_local` function and then use `.query` function to find an answer from the added datasets.

If you want to create a Naval Ravikant bot which has 2 of his blog posts, as well as a question and answer pair you supply, all you need to do is add the links to the blog posts and the QnA pair and embedchain will create a bot for you.

# Getting Started

## Installation

- Fork a copy of this replit template.

- Now open a shell and install the required packages using

```bash
pip install -r requirements.txt
```

- We use OpenAI's embedding model to create embeddings for chunks and ChatGPT API as LLM to get answer given the relevant docs. Make sure that you have an OpenAI account and an API key. If you have don't have an API key, you can create one by visiting [this link](https://platform.openai.com/account/api-keys).

- Click on Secrets and add the required environment variables, you can check sample.env fo reference.

```bash
OPENAI_API_KEY=""
```

## Usage

- Run the development server, using the Run button on top.

- Please note that running the app through Shell might not work as it is unable to read the secrets. Always run your app using the console or the Run button at the top.

- A webview tab will pop open. You can use this tab or copy the link to view your live app.

- By default we have setup a `Naval Ravikant Chat Bot` app.

- Wait for the data to load completely and then ask any query using the input box and then click on Submit.

- Your answer will be displayed in the result box below.

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

### App Types

We have two types of App.

#### 1. App (uses OpenAI models, paid)

```python
from embedchain import App

naval_chat_bot = App()
```

- `App` uses OpenAI's model, so these are paid models. You will be charged for embedding model usage and LLM usage.

- `App` uses OpenAI's embedding model to create embeddings for chunks and ChatGPT API as LLM to get answer given the relevant docs. Make sure that you have an OpenAI account and an API key. If you have dont have an API key, you can create one by visiting [this link](https://platform.openai.com/account/api-keys).

- Once you have the API key, set it in an environment variable called `OPENAI_API_KEY`

```python
import os
os.environ["OPENAI_API_KEY"] = "sk-xxxx"
```

#### 2. OpenSourceApp (uses opensource models, free)

```python
from embedchain import OpenSourceApp

naval_chat_bot = OpenSourceApp()
```

- `OpenSourceApp` uses open source embedding and LLM model. It uses `all-MiniLM-L6-v2` from Sentence Transformers library as the embedding model and `gpt4all` as the LLM.

- Here there is no need to setup any api keys. You just need to install embedchain package and these will get automatically installed.

- Once you have imported and instantiated the app, every functionality from here onwards is the same for either type of app.

### Add Dataset

- This step assumes that you have already created an `app` instance by either using `App` or `OpenSourceApp`. We are calling our app instance as `naval_chat_bot`

- Now use `.add` function to add any dataset.

```python

# naval_chat_bot = App() or
# naval_chat_bot = OpenSourceApp()

# Embed Online Resources
naval_chat_bot.add("youtube_video", "https://www.youtube.com/watch?v=3qHkcs3kG44")
naval_chat_bot.add("pdf_file", "https://navalmanack.s3.amazonaws.com/Eric-Jorgenson_The-Almanack-of-Naval-Ravikant_Final.pdf")
naval_chat_bot.add("web_page", "https://nav.al/feedback")
naval_chat_bot.add("web_page", "https://nav.al/agi")

# Embed Local Resources
naval_chat_bot.add_local("qna_pair", ("Who is Naval Ravikant?", "Naval Ravikant is an Indian-American entrepreneur and investor."))
```

- If there is any other app instance in your script or app, you can change the import as

```python
from embedchain import App as EmbedChainApp
from embedchain import OpenSourceApp as EmbedChainOSApp

# or

from embedchain import App as ECApp
from embedchain import OpenSourceApp as ECOSApp
```

## Interface Types

### Query Interface

- This interface is like a question answering bot. It takes a question and gets the answer. It does not maintain context about the previous chats.

- To use this, call `.query` function to get the answer for any query.

```python
print(naval_chat_bot.query("What unique capacity does Naval argue humans possess when it comes to understanding explanations or concepts?"))
# answer: Naval argues that humans possess the unique capacity to understand explanations or concepts to the maximum extent possible in this physical reality.
```

### Chat Interface

- This interface is chat interface where it remembers previous conversation. Right now it remembers 5 conversation by default.

- To use this, call `.chat` function to get the answer for any query.

```python
print(naval_chat_bot.chat("How to be happy in life?"))
# answer: The most important trick to being happy is to realize happiness is a skill you develop and a choice you make. You choose to be happy, and then you work at it. It's just like building muscles or succeeding at your job. It's about recognizing the abundance and gifts around you at all times.

print(naval_chat_bot.chat("who is naval ravikant?"))
# answer: Naval Ravikant is an Indian-American entrepreneur and investor.

print(naval_chat_bot.chat("what did the author say about happiness?"))
# answer: The author, Naval Ravikant, believes that happiness is a choice you make and a skill you develop. He compares the mind to the body, stating that just as the body can be molded and changed, so can the mind. He emphasizes the importance of being present in the moment and not getting caught up in regrets of the past or worries about the future. By being present and grateful for where you are, you can experience true happiness.
```

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

# How does it work?

Creating a chat bot over any dataset needs the following steps to happen

- load the data
- create meaningful chunks
- create embeddings for each chunk
- store the chunks in vector database

Whenever a user asks any query, following process happens to find the answer for the query

- create the embedding for query
- find similar documents for this query from vector database
- pass similar documents as context to LLM to get the final answer.

The process of loading the dataset and then querying involves multiple steps and each steps has nuances of it is own.

- How should I chunk the data? What is a meaningful chunk size?
- How should I create embeddings for each chunk? Which embedding model should I use?
- How should I store the chunks in vector database? Which vector database should I use?
- Should I store meta data along with the embeddings?
- How should I find similar documents for a query? Which ranking model should I use?

These questions may be trivial for some but for a lot of us, it needs research, experimentation and time to find out the accurate answers.

embedchain is a framework which takes care of all these nuances and provides a simple interface to create bots over any dataset.

In the first release, we are making it easier for anyone to get a chatbot over any dataset up and running in less than a minute. All you need to do is create an app instance, add the data sets using `.add` function and then use `.query` function to get the relevant answer.

# Tech Stack

embedchain is built on the following stack:

- [Langchain](https://github.com/hwchase17/langchain) as an LLM framework to load, chunk and index data
- [OpenAI's Ada embedding model](https://platform.openai.com/docs/guides/embeddings) to create embeddings
- [OpenAI's ChatGPT API](https://platform.openai.com/docs/guides/gpt/chat-completions-api) as LLM to get answers given the context
- [Chroma](https://github.com/chroma-core/chroma) as the vector database to store embeddings

# Author

- Taranjeet Singh ([@taranjeetio](https://twitter.com/taranjeetio))

## Maintainer

- [sahilyadav902](https://github.com/sahilyadav902)
