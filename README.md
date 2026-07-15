# LangGraph ReAct Agent

A Python-based AI agent implementation using **LangGraph** and **ReAct** (Reasoning + Acting) pattern with OpenAI's GPT-4 and Tavily Search capabilities.

## Overview

This project demonstrates a sophisticated agentic workflow that combines reasoning and tool usage to solve complex queries. The agent uses a graph-based state machine to orchestrate interactions between reasoning steps and tool execution, enabling it to search the web, perform calculations, and provide comprehensive answers.

## Features

- **ReAct Pattern Implementation**: Combines reasoning (thought processes) with acting (tool execution)
- **LangGraph State Management**: Uses message-based state graph for agent orchestration
- **Tool Integration**:
  - **Tavily Search**: Web search capability for real-time information retrieval
  - **Custom Tools**: Extensible tool framework (includes example `triple` function)
- **GPT-4 Powered**: Leverages OpenAI's GPT-4o model for intelligent reasoning and decision-making
- **Automated Workflow Visualization**: Generates Mermaid diagrams of agent flow

## Architecture

### Components

1. **nodes.py** - Agent logic
   - `run_agent_reasoning()`: LLM inference node that reasons about queries
   - `tool_node`: Executes selected tools based on agent decisions

2. **react.py** - Tool definitions
   - LLM initialization with tool bindings
   - Tool definitions (Tavily Search, custom functions)

3. **Main.py** - Application entry point
   - StateGraph construction
   - Conditional routing logic
   - Agent invocation and result handling

### Agent Flow

```
User Input → Agent Reasoning → Decision Point
                                    ↓
                            Tool Execution Needed?
                            ↙              ↘
                          YES              NO
                           ↓               ↓
                       Execute Tools    Return Result
                           ↓
                       Back to Reasoning
```

## Installation

### Requirements
- Python 3.9+

### Dependencies
```
langchain-openai>=0.3.35
langchain-tavily>=0.2.11
langgraph>=0.6.11
python-dotenv>=1.2.1
```

### Setup

1. Clone the repository:
```bash
git clone https://github.com/punithAJ-hub/langGraph-ReAct-Agent.git
cd langGraph-ReAct-Agent
```

2. Create a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```
Or using the included `uv.lock`:
```bash
uv sync
```

4. Set up environment variables:
Create a `.env` file in the root directory with:
```
OPENAI_API_KEY=your_openai_api_key
TAVILY_API_KEY=your_tavily_api_key
```

## Usage

### Basic Example

Run the agent with a sample query:
```bash
python Main.py
```

This will execute:
```python
"What is the weather in Seattle now? List it and triple the number"
```

### Custom Queries

Modify the query in `Main.py`:
```python
res = app.invoke({"messages": [HumanMessage(content="Your custom query here")]})
print(res["messages"][-1].content)
```

### Extending with Custom Tools

Add new tools in `react.py`:
```python
@tool
def your_tool(param: str) -> str:
    """Tool description"""
    return result

tools = [TavilySearch(max_results=1), triple, your_tool]
```

## Project Structure

```
langGraph-ReAct-Agent/
├── Main.py           # Agent orchestration & execution
├── nodes.py          # Agent reasoning and tool execution logic
├── react.py          # LLM and tool definitions
├── pyproject.toml    # Project configuration
├── uv.lock           # Dependency lock file
├── flow.png          # Generated agent flow diagram
└── .env              # Environment variables (not in repo)
```

## Key Concepts

### ReAct Pattern
The ReAct (Reasoning + Acting) pattern enables AI agents to:
- **Reason**: Think through problems step-by-step
- **Act**: Use available tools to gather information or perform actions
- **Loop**: Iterate between reasoning and acting until a solution is found

### Conditional Routing
The agent uses conditional edges to decide whether to:
- Execute tools (if tool calls are present in the LLM response)
- Return results (if no tool calls are needed)

```python
def should_continue(state: MessagesState) -> str:
    if not state["messages"][-1].tool_calls:
        return END
    return ACT
```

## API Keys Required

- **OpenAI API Key**: For GPT-4o access ([Get here](https://platform.openai.com/api-keys))
- **Tavily API Key**: For web search capabilities ([Get here](https://tavily.com))

## Technology Stack

- **LangChain**: LLM integration and tool management
- **LangGraph**: State graph-based agent orchestration
- **OpenAI GPT-4o**: Language model for reasoning
- **Tavily Search**: Web search API integration
- **Python 3.9+**: Programming language

## Output

The agent returns structured responses with:
- Complete reasoning process
- Tool calls and their results
- Final synthesized answer

Example workflow:
1. Parse the user query
2. Search for current weather in Seattle
3. Extract numerical information
4. Apply the `triple` function
5. Return comprehensive result

## Generated Flow Diagram

The agent automatically generates a Mermaid diagram (`flow.png`) visualizing the state graph and transitions.

## Future Enhancements

- Additional tool integrations (calculator, database queries, APIs)
- Multi-step reasoning with memory/context
- Agent performance monitoring and logging
- Streaming responses for real-time output
- Error handling and retry logic

## License

This project is open source and available under the MIT License.

## Contributing

Contributions are welcome! Feel free to:
- Report bugs and issues
- Suggest new features
- Submit pull requests

## Support

For issues or questions, please open a GitHub issue in the repository.

---

**Author**: [@punithAJ-hub](https://github.com/punithAJ-hub)  
**Created**: July 2026
