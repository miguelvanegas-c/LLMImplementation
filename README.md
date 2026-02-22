# LLMImplementation

Practice repository to **get familiar with LangChain** based on the official **LangChain LLM Chain Quickstart** tutorial:  
https://docs.langchain.com/oss/python/langchain/quickstart

Building on that foundation, this repo adapts the workflow to use **a local model via Ollama** (instead of Claude), since the Claude API requires a paid plan for real usage.

## Repository Contents

- `LLMChainTutorial.ipynb`: Main notebook with the step-by-step implementation.
- `README.md`: This file.

## What is implemented in the notebook?

The notebook follows an incremental tutorial-style flow to build an agent with LangChain:

1. **Dependency installation**
   - Installs and updates LangChain packages and the Ollama connector:
     - `langchain`
     - `langchain-community`
     - `langchain_ollama`

2. **Basic agent with a custom tool**
   - Defines a simple tool (`get_weather`) using `@tool`
   - Creates an agent with `create_agent`
   - Connects a local model using `ChatOllama`
   - Runs `agent.invoke(...)` to test the *user → tool → response* cycle

3. **System prompt (agent instructions)**
   - Defines agent behavior through a `SYSTEM_PROMPT`
   - Emphasizes when the agent should use tools to solve user requests

4. **Tools with user context (runtime context)**
   - Defines a `Context` (dataclass) to pass runtime information (for example, `user_id`)
   - Implements a tool that receives `ToolRuntime[Context]` to access that context

5. **Local model configuration**
   - Example with **Qwen 2.5** in Ollama:
     - `model="qwen2.5:7b"`
   - Note: the name must match what `ollama list` shows

6. **Structured output**
   - Defines an output schema using dataclasses (e.g., `ResponseFormat`)
   - Uses `ToolStrategy(ResponseFormat)` to enforce a consistent response format

7. **Conversation memory**
   - Adds in-memory state using `InMemorySaver` (LangGraph checkpointer)
   - Enables multi-turn conversations while keeping history by `thread_id`

8. **Final assembly and multi-turn test**
   - Combines model + tools + prompt + context + response format + memory
   - Runs a two-turn conversation reusing the same `thread_id`

## Requirements

- Python (the notebook suggests a `.venv`-style environment)
- **Ollama installed and running** locally: https://ollama.com/
- A model downloaded in Ollama (example used in the notebook: `qwen2.5:7b`)

## How to run

1. Clone the repository:
   ```bash
   git clone https://github.com/miguelvanegas-c/LLMImplementation.git
   cd LLMImplementation
   ```

2. (Optional) create and activate a virtual environment.

3. Open and run the notebook:
   - `LLMChainTutorial.ipynb`

4. Make sure Ollama is running and the model is available. For example:
   ```bash
   ollama list
   # if you do not have the model:
   ollama pull qwen2.5:7b
   ```

## Important Notes (Claude vs Ollama)

- This project **does not use Claude**.
- The main reason is that **Claude requires a paid API key** for real calls.
- Instead, it uses **Ollama** to run models locally, allowing you to experiment with LangChain without relying on paid providers.

## References

- Base tutorial (LangChain Quickstart / LLM Chain):
  https://docs.langchain.com/oss/python/langchain/quickstart
- Ollama:
  https://ollama.com/

