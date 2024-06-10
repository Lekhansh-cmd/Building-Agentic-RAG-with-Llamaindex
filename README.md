# Building Agentic RAG with LlamaIndex

## Overview

**Agentic RAG** is a framework designed to build research agents capable of advanced use, reasoning, and decision-making over data. For instance, it can synthesize data to answer QnA from a research paper. The standard RAG pipeline is effective for simpler questions over a small set of questions and works by retrieving a context.

This guide will walk you through the steps to build an Agentic RAG using LlamaIndex.

## Table of Contents
1. [Routing Query Engine](#routing-query-engine)
2. [Tool Calling](#tool-calling)
3. [Building an Agent Reasoning Loop](#building-an-agent-reasoning-loop)
4. [Building a Multi-Document Agent](#building-a-multi-document-agent)

## Routing Query Engine

### Step 1: Building the Router Query Engine

A router query engine is essential for answering both questions and summarizing data from a single document. This engine uses two types of indexes:

1. **Summary Index**: Used for summarization questions.
2. **Vector Index**: Used for retrieving specific context from the paper.

![Screenshot 2024-06-10 165402](https://github.com/Lekhansh-cmd/Building-Agentic-RAG-with-Llamaindex/assets/78807364/0efaf82c-270f-46df-a77a-30002627f523)

These indexes are converted to query tools with metadata that describes the types of questions they can answer.

### Router Components
- **Summary Tools**: Useful for summarization questions.
- **Vector Tools**: Useful for retrieving specific context from the paper.

### Selectors
Selectors are used to define a router. Available selectors include:
1. **LLM Selector**: Uses the LLM to output a JSON that is parsed, and the corresponding indexes are queried.
2. **Pydantic Selectors**: Use the OpenAI Function Calling API to produce pydantic selection objects, rather than parsing raw JSON.

## Tool Calling

### Step 2: Enabling Tool Calling

Tool calling allows LLMs to interact with external environments through a dynamic interface. This capability helps choose the function to execute and infer the argument to pass through the function. It adds a layer of query understanding on top of an RAG pipeline, enabling users to ask complex queries and get more precise results.

## Building an Agent Reasoning Loop

### Step 3: Implementing the Agent Reasoning Loop

![Screenshot 2024-06-10 185003](https://github.com/Lekhansh-cmd/Building-Agentic-RAG-with-Llamaindex/assets/78807364/a66be693-e34a-45b2-a212-69fdd69484a4)

For complex, multi-step questions, an Agent Reasoning Loop is used instead of simple tool calling. An agent in LlamaIndex consists of two main components:

1. **AgentWorker**: Executes the next steps of the given agent.
2. **AgentRunner**: Overall task dispatcher, responsible for creating tasks, orchestrating runs, and returning the final response to the user.

### Full Agent Reasoning Loop
The agent can maintain conversation history over time using a conversational memory buffer. This buffer can be customized and is typically a rolling list of items depending on the context window of the LLM. This enables the agent to use both current and previous conversation history to perform the next action.

![Screenshot 2024-06-10 185724](https://github.com/Lekhansh-cmd/Building-Agentic-RAG-with-Llamaindex/assets/78807364/43141ba8-960e-42e9-b576-fb127042fa08)

### Benefits of Agent Control
1. **Decoupling of Task Creation and Execution**: Separates task creation from task execution.
2. **Enhanced Debuggability**: Offers deeper insights into each step of the execution process.
3. **Refined Control**: Allows users to modify intermediate steps and incorporate human feedback.

## Building a Multi-Document Agent

### Step 4: Creating a Multi-Document Agent

![Screenshot 2024-06-10 192502](https://github.com/Lekhansh-cmd/Building-Agentic-RAG-with-Llamaindex/assets/78807364/372f0709-3d7c-42b6-970a-54f9f8a6af8e)

We will build agents for multiple documents, such as a 3-document agent and an 11-document agent. To handle multiple documents:

1. **Create a Doc Tool**: For each PDF, create a corresponding vector tool and summary tool.
2. **Mapping Tools**: Map the vector tool to the summary tool for each document.

### Handling Many PDFs
If you have a large number of PDFs (e.g., 100), the LLM may struggle to select the correct tool. To mitigate this:

1. **Perform Retrieval Argumentation**: Instead of retrieving text, perform retrieval on the level of tools.
2. **Relevant Tool Selection**: Retrieve a small set of relevant tools and feed them to the Agent Reasoning Prompt.

![Screenshot 2024-06-10 193115](https://github.com/Lekhansh-cmd/Building-Agentic-RAG-with-Llamaindex/assets/78807364/b83c4963-9a9b-4130-ae62-e0680bbb8810)

This retrieval process is similar to the one used in RAG.

## Conclusion

This guide provides a comprehensive overview of building an Agentic RAG using LlamaIndex, from constructing a router query engine to enabling tool calling, building an agent reasoning loop, and handling multiple documents efficiently. By following these steps, you can create advanced research agents capable of sophisticated data analysis and decision-making.
