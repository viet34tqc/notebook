# Basic AI

## Context window

- The context window is the number of previous messages that the AI uses to generate a response
- A larger context window allows the AI to consider more context and generate more coherent responses.

## Tokens

A token is a word or a part of a word and differs slightly by language. There's a tool you can use to measure that's recommended by OpenAI, it's called [tokenizer](https://platform.openai.com/tokenizer)

## Retrieval-Augmented generation, RAG core concepts

At its core, RAG involves two main components: a **retriever** and a **generator**.

- **The retriever:** it's responsible for finding relevant information from external data sources that can be used to enhance the AI-generated responses, like a search engine. This information can be in the form of text, images, or any other type of data that is relevant to the context of the conversation, although text is the most common type of data used.

- **The generator:** it takes the retrieved information and uses it to generate a response that is contextually relevant and informative.

RAG can be considered as an advanced form of *prompt engineering*.