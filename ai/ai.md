# AI

## Table of contents

- [AI](#ai)
  - [Table of contents](#table-of-contents)
  - [Artificial intelligence](#artificial-intelligence)
    - [Generative AI](#generative-ai)
  - [Neural network](#neural-network)
  - [Machine learning](#machine-learning)
    - [Machine learning model](#machine-learning-model)
  - [Deep learning](#deep-learning)
    - [Transformer architecture](#transformer-architecture)
    - [Attention mechanism](#attention-mechanism)
  - [AI model](#ai-model)
  - [Large language model](#large-language-model)
    - [Prompt](#prompt)
    - [Context window](#context-window)
    - [Token](#token)
    - [Chatbot](#chatbot)
    - [Generative pre-trained transformer](#generative-pre-trained-transformer)
  - [AI agent](#ai-agent)
    - [Session](#session)
  - [AI workflow](#ai-workflow)
  - [Retrieval-augmented generation](#retrieval-augmented-generation)
  - [Model context protocol](#model-context-protocol)
  - [Harness](#harness)
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

## Neural network

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

## Machine learning

[↑ **Machine learning**](https://en.wikipedia.org/wiki/Machine_learning) or **ML** is a field of study in [artificial intelligence](#artificial-intelligence) concerned with the development and study of statistical algorithms that can learn from data and generalize to unseen data, and thus perform tasks without being explicitly programmed.

### Machine learning model

A **machine learning model** is a computational model whose behavior is learned from data rather than being entirely specified through manually written rules.

## Deep learning

**Deep learning** or **DL** is a subset of [machine learning](#machine-learning) that uses artificial [neural networks](#neural-network) with multiple layers to learn complex patterns from data.

The word _deep_ refers to the number of layers in a neural network.

### Transformer architecture

A [↑ **transformer architecture**](<https://en.wikipedia.org/wiki/Transformer_(deep_learning)>)is a type of deep learning [neural network](#neural-network) architecture designed to process sequences of data, such as text, using a mechanism called attention.

Transformers are the foundation of most modern [LLMs](#large-language-model).

### Attention mechanism

An [↑ **attention mechanism**](<https://en.wikipedia.org/wiki/Attention_(machine_learning)>) is a technique used in [neural networks](#neural-network) that allows a model to determine which parts of the input are most relevant when processing a particular piece of information.

An attention mechanism allows the model to assign different weights to different [tokens](#token) based on their relevance. Attention weights are learned through training, rather than being manually programmed.

Transformers typically use **multi-head attention**, meaning they calculate several sets of attention patterns in parallel.

Different attention heads can learn different relationships, such as:

- Grammatical dependencies
- Relationships between pronouns and nouns
- Long-distance contextual dependencies
- Semantic relationships between words

The outputs of these attention heads are combined and projected to produce the final attention output.

## AI model

An **AI model** is a computational model used to perform tasks associated with [artificial intelligence](#artificial-intelligence), such as reasoning, prediction, classification, or content generation.

An [LLM](#large-language-model) is a specific type of AI model.

## Large language model

A [↑ **large language model**](https://en.wikipedia.org/wiki/Large_language_model) or **LLM** is a [machine learning model](#machine-learning) (typically a [neural network](#neural-network)) trained on a vast amount of text for natural language processing tasks, especially language generation.

LLMs can typically generate, summarize, translate, and analyze text in many contexts. They are the basis for many modern [chatbots](#chatbot), such as ChatGPT, Claude, Gemini, Grok, Qwen, and DeepSeek.

All LLMs are [AI models](#ai-model), but not all AI models are LLMs.

### Prompt

A **prompt** is the input provided to an [AI model](#ai-model) to instruct it what to do or guide the response it generates.

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

### Generative pre-trained transformer

A [↑ **generative pre-trained transformer**](https://en.wikipedia.org/wiki/Generative_pre-trained_transformer) or **GPT** is a type of [AI model](#ai-model) designed to understand and generate human-like text, code, images, and other media.

The model uses a neural network architecture called a [transformer](#transformer-architecture), which relies on an [attention mechanism](#attention-mechanism) to understand relationships between tokens.

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

### Session

A **session** is a sequence of interactions between a user and an [AI agent](#ai-agent) that share a common conversational context, including previous messages, instructions, and results of executed actions.

A session may span multiple tasks, can often be paused and resumed, and typically ends when explicitly terminated or replaced by a new session.

A session is not the same as a [context window](#context-window). A session is a logical conversation, which can continue over time. A context window is a limited amount of information the AI model can process at once. A session might last several hours and contain hundreds of interactions. When its context window fills up, the AI agent can compact earlier conversation history and continue within the same session.

## AI workflow

An **AI workflow** is a sequence of predefined steps that uses AI to automate or assist with completing a task.

AI workflow example:

```text
A customer sends an email → AI classifies it → predefined logic selects a department → AI generates a response
```

An AI workflow typically combines [AI models](#ai-model), traditional programming logic, external tools, and data sources to achieve a specific goal.

An AI workflow typically consists of:

1. **Trigger**: an event that starts the workflow, such as an incoming email, API request, or scheduled task
2. **AI model**: an LLM or another AI model that processes information, makes classifications, or generates content
3. **Tools**: external services, APIs, databases, or applications used to perform actions
4. **Control logic**: predefined rules, conditions, and branching that determine the execution sequence
5. **Output**: the final result, such as a generated document, database update, or email response

## Retrieval-augmented generation

[↑ **Retrieval-augmented generation**](https://en.wikipedia.org/wiki/Retrieval-augmented_generation) or **RAG** is an AI technique that improves the accuracy and relevance of [LLM](#large-language-model) responses by retrieving information from external sources before generating an answer.

RAG doesn't teach an LLM new knowledge by modifying its weights. Instead, it supplies relevant external information at the moment the model generates an answer.

## Model context protocol

The [↑ **Model Context Protocol**](https://en.wikipedia.org/wiki/Model_Context_Protocol) or **MCP** is an open standard that allows AI applications to connect to external tools, services, and data sources through a standardized interface.

By default, an LLM cannot directly access external systems such as databases, GitHub repositories, or local files. Without MCP, developers must implement custom integrations for each AI application and external service. MCP solves this problem by providing a common protocol for connecting AI applications to external systems.

Think of MCP as a USB-C port for AI applications: different devices can connect through the same standardized interface.

## Harness

A [↑ **harness**](https://en.wikipedia.org/wiki/Agent_harness) is the software infrastructure that surrounds an [AI model](#ai-model) and manages how it interacts with tools, data, users, and its environment.

An [LLM](#large-language-model) by itself can process input and generate output, but it cannot independently execute code, access databases, or interact with external services. A harness provides these capabilities.

For example, when you ask an AI coding assistant to fix a bug, the harness might:

1. Read relevant source files
2. Send their contents to the LLM
3. Receive the LLM's request to modify a file
4. Execute the requested operation
5. Run tests and collect their results
6. Send the results back to the LLM
7. Repeat until the task is complete or a limit is reached

The LLM makes decisions, while the harness orchestrates their execution

A coding harness may provide:

- File access: Read and modify source code.
- Terminal access: Execute dotnet build or dotnet test.
- Context management: Select relevant files and preserve conversation history.
- Tool integration: Communicate with MCP servers.
- Safety controls: Restrict dangerous operations or request approval.
- Execution loop: Repeatedly call the LLM and execute tools until the task is finished.

## Hugging Face

[↑ Hugging Face](https://huggingface.co/) is the central hub and ecosystem for open-source AI and machine learning.

Often called the "GitHub of AI," it provides a platform where developers, researchers, and companies host, share, and collaborate on AI models, datasets, and web applications.
