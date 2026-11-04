# AI

## Table of contents

- [AI](#ai)
  - [Table of contents](#table-of-contents)
  - [Artificial intelligence](#artificial-intelligence)
    - [Generative AI](#generative-ai)
  - [Machine learning](#machine-learning)
    - [Deep learning](#deep-learning)
    - [Neural network](#neural-network)
  - [Machine learning model](#machine-learning-model)
  - [Large language model](#large-language-model)
    - [Prompt](#prompt)
    - [Context window](#context-window)
    - [Token](#token)
    - [Chatbot](#chatbot)
  - [AI agent](#ai-agent)
  - [Retrieval-augmented generation](#retrieval-augmented-generation)
  - [Hugging Face](#hugging-face)

## Artificial intelligence

[↑ **Artificial intelligence**](https://en.wikipedia.org/wiki/Artificial_intelligence) or **AI** is the ability of computational systems to perform tasks typically associated with human intelligence, such as learning, reasoning, problem-solving, perception, and decision-making.

Prominent applications of AI include advanced web search engines, [chatbots](#chatbot), virtual assistants, autonomous vehicles, play and analysis in strategy games (e.g., chess and Go), and content generation (e.g., text, images, audio, and videos).

### Generative AI

[↑ **Generative AI**](https://en.wikipedia.org/wiki/Generative_AI) or **GenAI** is a type of [artificial intelligence](#artificial-intelligence) that can create new content, such as text, images, music, videos, and computer code, based on patterns learned from large amounts of training data.

Unlike traditional AI systems, which primarily analyze, classify, or predict information, generative AI produces new content in response to user [prompts](#prompt):

| Traditional AI               | Generative AI                    |
| ---------------------------- | -------------------------------- |
| Detects spam emails          | Writes emails                    |
| Recognizes objects in images | Generates images                 |
| Predicts house prices        | Creates property descriptions    |
| Classifies customer feedback | Generates responses to customers |
| Detects bugs in code         | Generates code to fix bugs       |

A generative model can also perform classification and analysis.

## Machine learning

[↑ **Machine learning**](https://en.wikipedia.org/wiki/Machine_learning) or **ML** is a field of study in [artificial intelligence](#artificial-intelligence) concerned with the development and study of statistical algorithms that can learn from data and generalize to unseen data, and thus perform tasks without being explicitly programmed.

### Deep learning

**Deep learning** or **DL** is a subset of [machine learning](#machine-learning) that uses artificial [neural networks](#neural-network) with multiple layers to learn complex patterns from data.

The word _deep_ refers to the number of layers in a neural network.

### Neural network

A [↑ **neural network**](<https://en.wikipedia.org/wiki/Neural_network_(machine_learning)>) is a computational model inspired by the structure and functions of biological neural networks.

A neural network consists of connected units or nodes called _artificial neurons_, which loosely model the neurons in the brain. These are connected by edges, which model the synapses in the brain. Each artificial neuron receives signals from connected neurons, then processes them and sends a signal to other connected neurons. The "signal" is a real number, and the output of each neuron is computed by some non-linear function of the totality of its inputs, called the _activation function_. The strength of the signal at each connection is determined by a weight, which adjusts as part of the training process.

The most important concept is that _a neural network learns by adjusting its weights and biases to reduce errors_.

The key idea: a neural network is a mathematical function with adjustable parameters. Training is the process of finding parameter values that make the function perform a task well.

When you type a question into a [chatbot](#chatbot):

1. Your text is converted into [tokens](#token)
2. Tokens are transformed into numerical vector representations
3. Those representations pass through many neural network layers
4. The model computes probabilities for possible next tokens
5. It generates output tokens sequentially to form its response

## Machine learning model

A **machine learning model** is a computational model whose behavior is learned from data rather than being entirely specified through manually written rules.

## Large language model

A [↑ **large language model**](https://en.wikipedia.org/wiki/Large_language_model) or **LLM** is a [machine learning model](#machine-learning) (typically a [neural network](#neural-network)) trained on a vast amount of text for natural language processing tasks, especially language generation.

LLMs can typically generate, summarize, translate, and analyze text in many contexts. They are the basis for many modern [chatbots](#chatbot), such as ChatGPT, Claude, Gemini, Grok, Qwen, and DeepSeek.

### Prompt

A **prompt** is the input provided to an AI model to instruct it what to do or guide the response it generates.

### Context window

A **context window** is the maximum amount of text—measured in [tokens](#token) (chunks of characters, words, or sub-words roughly equivalent to $0.75$ words in English)—that a Large Language Model can process and "remember" at any single moment.

Think of it as the model's short-term working memory or RAM during a session. It encompasses everything active in the current interaction: the system [prompt](#prompt), uploaded documents, the entire chat history, and the model's own generated response.

### Token

A **token** is a basic unit of text that an [LLM](#large-language-model) processes.

When you send a prompt to an AI model, the text is divided into smaller pieces called **tokens**. These can represent entire words, parts of words, punctuation marks, or spaces combined with text.

The process of splitting text into tokens is called **tokenization**.

A token is not necessarily a complete word.

For English, a common approximation is: 1 token ≈ 4 characters, 100 tokens ≈ 75 words. The actual ratio depends on the language, vocabulary, formatting, and tokenizer. For example, Russian text can require more tokens per word than English text with some tokenizers.

The model doesn't directly manipulate words as humans do. It operates on numerical representations of tokens.

One of the fundamental tasks of an autoregressive language model is predicting the next token based on previous tokens. The model selects a token according to its decoding strategy, adds it to the sequence, and repeats the process until it finishes generating the response.

Many LLM providers charge based on token usage. For example, imagine hypothetical pricing of:

- Input: $2 per million tokens
- Output: $8 per million tokens

A request using 10,000 input tokens and generating 2,000 output tokens would cost:
$$
\frac{10,000}{1,000,000}\times2+
\frac{2,000}{1,000,000}\times8
=\$0.036
$$

More tokens generally require more computation. Long prompts can increase latency, while generating more output tokens takes additional time.

### Chatbot

A [↑ **chatbot**](https://en.wikipedia.org/wiki/Chatbot) is a software application designed to converse through text or speech, functioning as a conversation partner.

[Generative AI](#generative-ai) chatbots such as ChatGPT, Gemini, Claude, DeepSeek and Grok are based on deep learning. These chatbots typically use fine-tuned large language models to generate text.

## AI agent

An [↑ **AI agent**](https://en.wikipedia.org/wiki/AI_agent) is an AI-powered system that can autonomously perform tasks, make decisions, and interact with external tools to achieve a specific goal.

Unlike a traditional [chatbot](#chatbot), which primarily answers questions, an AI agent can take actions on your behalf. The key difference is that the agent can decide what to do next based on the results of previous actions.

An AI agent typically consists of several components:

- [LLM](#large-language-model): reasons about the task and selects actions
- Tools: functions for interacting with APIs, filesystems, databases, browsers, and terminals
- Memory/state: information about previous actions, observations, and task progress
- Orchestration: software that manages the execution loop, tool calls, retries, and stopping conditions
- Guardrails: permissions and constraints controlling what the agent is allowed to do

Popular examples include OpenAI Codex, Claude Code, OpenClaw.

## Retrieval-augmented generation

[↑ **Retrieval-augmented generation**](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) or **RAG** is an AI technique that improves the accuracy and relevance of [LLM](#large-language-model) responses by retrieving information from external sources before generating an answer.

RAG doesn't teach an LLM new knowledge by modifying its weights. Instead, it supplies relevant external information at the moment the model generates an answer.

## Hugging Face

[↑ Hugging Face](https://huggingface.co/) is the central hub and ecosystem for open-source AI and machine learning.

Often called the "GitHub of AI," it provides a platform where developers, researchers, and companies host, share, and collaborate on AI models, datasets, and web applications.
