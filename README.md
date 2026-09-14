# AI Coding Agent 👾

A toy version of tools like Claude Code, Cursor, and OpenCode — a command-line AI agent that can explore a codebase, read and write files, and execute Python code to complete tasks, powered by free models via [OpenRouter](https://openrouter.ai/).

## How it works

The agent uses an LLM as a decision-making engine in an iterative loop:

1. The user gives a natural-language prompt (e.g. "Fix the bug: 3 + 7 * 2 shouldn't be 20").
2. The LLM decides which function(s) it needs to call to accomplish the task.
3. The agent executes those functions and returns the results to the LLM.
4. The LLM either requests more function calls or produces a final response.
5. This repeats (up to 20 iterations) until the LLM has a complete answer.

## Available agent tools

- **`get_files_info`** — list files and directories with size/type info
- **`get_file_content`** — read file contents (truncated at 10,000 characters)
- **`write_file`** — write or overwrite file contents
- **`run_python_file`** — execute a Python file with optional arguments (30-second timeout)

All file operations are scoped to a fixed working directory (`./calculator`) for safety — the agent cannot read or write files outside of it.

## Setup

1. Clone this repo and install dependencies:
```sh
   uv sync
```

2. Get a free API key from [OpenRouter](https://openrouter.ai/keys).

3. Create a `.env` file in the project root:


## Usage

```sh
uv run main.py "your prompt here"
```

Add `--verbose` to see each function call, its arguments, results, and token usage:

```sh
uv run main.py "list the contents of the pkg directory" --verbose
```

### Example

```sh
uv run main.py "Fix the bug: 3 + 7 * 2 shouldn't be 20."
```

The agent will explore the `calculator/` directory, read the relevant source files, identify the bug, apply a fix, and verify the fix works — all autonomously.

## Project structure

```
ai-coding-agent/
├── main.py                  # entry point, CLI args, agent loop
├── prompts.py                # system prompt
├── call_function.py          # function dispatch/mapping
├── config.py                  # shared config (e.g. MAX_CHARS)
├── functions/
│   ├── get_files_info.py
│   ├── get_file_content.py
│   ├── write_file.py
│   └── run_python_file.py
├── calculator/                # sample project the agent operates on
│   ├── main.py
│   ├── tests.py
│   └── pkg/
│       ├── calculator.py
│       └── render.py
└── test_*.py                  # manual test scripts for each function
```
