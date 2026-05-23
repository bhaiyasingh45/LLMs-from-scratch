# Setting Up a Python Virtual Environment on Mac Using `uv`

[uv](https://github.com) is an extremely fast Python package installer and resolver written in Rust. It serves as a drop-in replacement for standard `pip` and `venv` tools.

## 1. Prerequisites

If you do not have `uv` installed, use Homebrew to install it globally on your Mac:

```bash
brew install uv
```

## 2. Standard Virtual Environment Workflow

### Step 1: Navigate to Your Project
Open your Terminal and change directories to your project folder:
```bash
cd path/to/your/project
```

### Step 2: Create the Virtual Environment
Run the creation command. By default, this creates a folder named `.venv`:
```bash
uv venv
```
*To use a specific Python version (e.g., 3.12), add the version flag:*
```bash
uv venv --python 3.12
```

### Step 3: Activate the Environment
To start using the environment, source the activation script:
```bash
source .venv/bin/activate
```
*(Your terminal prompt will now show `(.venv)` at the beginning of the line).*

### Step 4: Install Packages
Install your dependencies using `uv pip` instead of regular `pip` for maximum speed:
```bash
uv pip install <package_name>
```

### Step 5: Deactivate
When you are finished working, return to your global system Python environment:
```bash
deactivate
```

---

## 3. Quick Reference Guide


| Action | Command |
| :--- | :--- |
| **Create default environment (`.venv`)** | `uv venv` |
| **Create with specific Python version** | `uv venv --python <version>` |
| **Activate environment** | `source .venv/bin/activate` |
| **Install a package** | `uv pip install <package>` |
| **Install from requirements file** | `uv pip install -r requirements.txt` |
| **Deactivate environment** | `deactivate` |

---

## 4. Alternate Modern Workflow (Project Management)

If you prefer `uv` to handle the environment and dependency tracking automatically without manual activation, use the project commands:

*   **Initialize a new project:** 
    ```bash
    uv init my-project
    ```
*   **Add a dependency:** (Automatically creates `.venv` if it doesn't exist and locks the version)
    ```bash
    uv add requests
    ```
*   **Run scripts safely:** (Executes your script locked into the project environment)
    ```bash
    uv run main.py
    ```
