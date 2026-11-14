
# Building a Python MCP Server
### A Step-by-Step Guide Using VS Code and GitHub Copilot Agent Mode

**NAME:** Geethika  
**REG NO:** 261fb04040

---

## 1. Project Information

| Detail | Description |
|---|---|
| Project Name | hello-mcp |
| Programming Language | Python |
| Code Editor | Visual Studio Code |
| AI Coding Assistant | GitHub Copilot |
| Client | GitHub Copilot Agent Mode |
| Communication Transport | stdio |
| Dependency | Python MCP SDK with CLI extras |

## 2. Project Overview

This project demonstrates how to build a simple local **Model Context Protocol (MCP) Server** using Python, Visual Studio Code, and GitHub Copilot Agent Mode.

The MCP server exposes Python functions as tools that an MCP-compatible client can discover and execute.

In this project, two MCP tools are implemented:

1. **get_greeting** – Returns a personalized greeting message for a student.
2. **add_numbers** – Accepts two integers and returns their sum.

The server communicates with GitHub Copilot through standard input/output (stdio) transport.

## 3. Technologies Used

- **Python:** Used to develop the MCP server.
- **Visual Studio Code:** Used as the development environment.
- **GitHub Copilot:** AI coding assistant used to create and test the MCP server.
- **MCP SDK:** Python library used to implement MCP tools.
- **stdio Transport:** Enables communication between the MCP server and client.

## 4. Prerequisites

Before starting the project, make sure the following are installed:

- Windows 10/11 (64-bit).
- Python 3.12 or above, subject to MCP SDK compatibility.
- Visual Studio Code.
- GitHub account with GitHub Copilot access.
- GitHub Copilot Agent Mode enabled.
- Internet connection for installing dependencies.

### Verify Python Installation

Open the VS Code terminal and execute:

```bash
py --version
py -0p
```

These commands display the installed Python version and available Python installations.

## 5. Project Setup

### Step 1: Create the Project Folder

Create a folder named `hello-mcp`.

```bash
mkdir "%USERPROFILE%\Projects"
cd /d "%USERPROFILE%\Projects"
mkdir hello-mcp
cd hello-mcp
```

Open the `hello-mcp` folder in Visual Studio Code.

### Step 2: Create a Virtual Environment

A virtual environment isolates project dependencies from other Python projects.

```bash
py -3.12 -m venv .venv
```

Replace `3.12` with your installed and compatible Python version.

### Step 3: Install the MCP SDK

Install the MCP SDK with CLI extras using:

```bash
.venv\Scripts\python.exe -m pip install "mcp[cli]" --default-timeout=120
```

### Step 4: Verify Installation

Check whether the MCP package is installed successfully.

```bash
.venv\Scripts\python.exe -m pip show mcp
```

Check the Python version:

```bash
.venv\Scripts\python.exe --version
```

### Step 5: Save Dependencies

Generate a requirements file:

```bash
.venv\Scripts\python.exe -m pip freeze > requirements.txt
```

## 6. Create the MCP Server

Create a Python file named `server.py` in the project folder.

Add the following code:

```python
from mcp.server.mcpserver import MCPServer

mcp = MCPServer("Hello MCP")


@mcp.tool()
def get_greeting(name: str) -> str:
    """Return a greeting for a student."""
    return f"Hello, {name}! Your first MCP tool works."


@mcp.tool()
def add_numbers(a: int, b: int) -> int:
    """Add two numbers."""
    return a + b


if __name__ == "__main__":
    mcp.run()
```

### Explanation of the Code

1. **Import MCPServer:** Imports the MCP server class required to create the server.
2. **Create MCP Server:** `MCPServer("Hello MCP")` initializes the MCP server.
3. **Register Tools:** The `@mcp.tool()` decorator registers Python functions as MCP tools.
4. **get_greeting():** Accepts a student's name and returns a personalized greeting.
5. **add_numbers():** Accepts two integer values and returns their sum.
6. **Start the Server:** `mcp.run()` starts the MCP server when the Python file is executed directly.

**Note:** This code follows the MCPServer API used in the project example. If the installed SDK version does not provide this import or API, check the documentation for the installed SDK version and update the server code accordingly.

## 7. Configure MCP Server in VS Code

VS Code requires an MCP configuration file to connect to the local server.

### Step 1: Create the Configuration Folder

Create a folder named `.vscode` in the project root.

### Step 2: Create mcp.json

Inside `.vscode`, create a file named `mcp.json`.

### Step 3: Add Configuration

Paste the following JSON:

```json
{
  "servers": {
    "helloMcp": {
      "type": "stdio",
      "command": "${workspaceFolder}\\.venv\\Scripts\\python.exe",
      "args": ["server.py"]
    }
  }
}
```

### Configuration Explanation

| Setting | Purpose |
|---|---|
| helloMcp | MCP server identifier |
| type | Specifies stdio communication |
| command | Specifies the virtual environment Python executable |
| args | Specifies the Python server file to execute |

Save the configuration file.

## 8. Start the MCP Server

Follow these steps in VS Code:

1. Save all project files.
2. Press `Ctrl + Shift + P`.
3. Search for `MCP: List Servers`.
4. Select the `helloMcp` server.
5. Choose Start or Restart if prompted.
6. Check the server status.

### Expected Results

- The server starts without errors.
- VS Code displays the server as running or connected.
- Two MCP tools are discovered:
  - `get_greeting`
  - `add_numbers`

## 9. Test MCP Tools Using GitHub Copilot Agent Mode

Open GitHub Copilot Chat in VS Code and select **Agent Mode**.

### Test Case 1: get_greeting

Enter the following prompt:

```text
Use the get_greeting MCP tool from helloMcp
with the name Students. Show me the result.
```

**Expected Output:**

```text
Hello, Students! Your first MCP tool works.
```

### Test Case 2: add_numbers

Enter the following prompt:

```text
Use the add_numbers MCP tool from helloMcp
with a = 10 and b = 20. Show me the result.
```

**Expected Output:**

```text
30
```

These tests verify that GitHub Copilot can discover and invoke the tools exposed by the local MCP server.

## 10. Project Structure

The final project directory should look like this:

```text
hello-mcp/
│
├── server.py
├── requirements.txt
├── README.md
├── .gitignore
│
├── .venv/
│
└── .vscode/
    └── mcp.json
```

### File Description

| File/Folder | Description |
|---|---|
| server.py | Contains the MCP server and its two tools |
| requirements.txt | Records installed Python dependencies |
| .venv/ | Contains the isolated Python environment |
| .vscode/mcp.json | Configures the MCP server in VS Code |
| README.md | Project documentation |
| .gitignore | Excludes unnecessary files from Git |

## 11. Troubleshooting

| Problem | Solution |
|---|---|
| Python not found | Verify installation using `py --version`. |
| MCP module not found | Check installation using `pip show mcp` inside `.venv`. |
| MCPServer import error | Verify SDK version and API compatibility. |
| Python executable not found | Verify the `.venv` folder and interpreter path. |
| Pip installation timeout | Retry with `--default-timeout=120`. |
| Server does not start | Check Python syntax, JSON configuration, and server logs. |
| Copilot does not call tools | Enable Agent Mode and verify that the server is running and tools are discovered. |

## 12. Git Configuration

Create a `.gitignore` file and add the following entries:

```text
.venv/
__pycache__/
*.py[cod]
```

This prevents unnecessary virtual environment files and Python cache files from being committed to GitHub.

## 13. Final Verification Checklist

- [ ] Python 3.12 or above is installed and compatible with the MCP SDK.
- [ ] VS Code is installed and the project folder is open.
- [ ] Virtual environment `.venv` is created.
- [ ] MCP SDK is installed successfully.
- [ ] `server.py` contains both MCP tools.
- [ ] `.vscode/mcp.json` is configured correctly.
- [ ] MCP server is running in VS Code.
- [ ] Both tools are discovered successfully.
- [ ] `get_greeting` returns the expected greeting.
- [ ] `add_numbers` returns 30 for inputs 10 and 20.

## 14. Learning Outcomes

By completing this project, I learned:

- Fundamentals of Model Context Protocol (MCP).
- How to create an MCP server using Python.
- How to create and register custom MCP tools.
- How to configure stdio transport.
- How to create and manage Python virtual environments.
- How to configure MCP servers in Visual Studio Code.
- How to use GitHub Copilot Agent Mode to discover and execute MCP tools.
- How AI applications interact with locally hosted MCP servers.

## 15. Conclusion

This project demonstrates the implementation of a Python-based Model Context Protocol server using Visual Studio Code and GitHub Copilot Agent Mode.

By developing the `get_greeting` and `add_numbers` tools, I gained practical knowledge of MCP server creation, tool registration, stdio communication, dependency management, and integration with an AI-powered development environment.

The project provides a foundation for developing more advanced MCP servers with additional tools and functionalities.

---

**PROJECT:** hello-mcp  
**DEVELOPED BY:** Geethika  
**REGISTRATION NUMBER:** 261fb04040  
**TECHNOLOGY:** Python + VS Code + GitHub Copilot Agent Mode
