# UV Package Manager - Complete Learning Guide 📚

*Your Personal Python Package Management Notebook*

---

## Table of Contents
1. [What is UV?](#what-is-uv)
2. [Installation](#installation)
3. [Basic Concepts](#basic-concepts)
4. [Project Management](#project-management)
5. [Package Management](#package-management)
6. [Virtual Environment Management](#virtual-environment-management)
7. [Running Code](#running-code)
8. [Configuration & Settings](#configuration--settings)
9. [Common Workflows](#common-workflows)
10. [Troubleshooting](#troubleshooting)
11. [Comparison with Other Tools](#comparison-with-other-tools)
12. [Advanced Features](#advanced-features)
13. [Best Practices](#best-practices)
14. [Quick Reference](#quick-reference)

---

## What is UV?

**UV** is a modern Python package and project manager written in Rust. Think of it as a **super-fast replacement** for pip, virtualenv, and poetry combined into one tool.

### 🚀 Why UV is Special:
- **Lightning Fast**: 10-100x faster than pip
- **All-in-One**: Replaces pip, virtualenv, poetry, pipenv
- **Automatic**: Handles virtual environments automatically
- **Modern**: Built with modern Python development practices
- **Simple**: Fewer commands to remember

### 🔄 What UV Replaces:
```
Traditional Tools          →    UV
pip install package       →    uv add package
python -m venv myenv      →    uv init (automatic)
source myenv/bin/activate →    (not needed!)
pip freeze > requirements →    pyproject.toml (automatic)
```

---

## Installation

### Method 1: Using pip (Recommended)
```bash
pip install uv
```

### Method 2: Using curl (Linux/Mac)
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Method 3: Using PowerShell (Windows)
```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

### ✅ Verify Installation:
```bash
uv --version
```

---

## Basic Concepts

### 🎯 Key Concepts to Understand:

#### 1. **Project-Based Management**
- UV works with **projects** (folders with Python code)
- Each project gets its own isolated environment
- No need to manually create/activate environments

#### 2. **Automatic Virtual Environments**
- UV creates `.venv` folder automatically
- You never need to activate/deactivate manually
- Each project is isolated from others

#### 3. **pyproject.toml File**
- Modern Python standard for project configuration
- Replaces old `requirements.txt`
- Contains all project information

#### 4. **Lock Files**
- `uv.lock` file stores exact versions
- Ensures reproducible installations
- Similar to `package-lock.json` in Node.js

---

## Project Management

### 🆕 Creating a New Project

#### Method 1: Start from Scratch
```bash
# Create new project
uv init my-awesome-project

# What this creates:
my-awesome-project/
├── .venv/                 # Virtual environment (auto-created)
├── pyproject.toml         # Project configuration
├── README.md             # Project documentation
└── hello.py              # Sample Python file
```

#### Method 2: Initialize in Existing Folder
```bash
# Go to your existing project
cd my-existing-project

# Initialize UV project
uv init

# This adds UV management to existing code
```

### 📁 Project Structure Explained

```
my-project/
├── .venv/                 # 🔒 Virtual environment (don't touch!)
├── pyproject.toml         # 📋 Project configuration
├── uv.lock               # 🔐 Exact versions (auto-generated)
├── README.md             # 📖 Documentation
├── src/                  # 📂 Your Python code
│   └── my_project/
├── tests/                # 🧪 Test files
└── requirements.txt      # 📝 Optional (for compatibility)
```

### 🔍 Understanding pyproject.toml

```toml
[project]
name = "my-awesome-project"
version = "0.1.0"
description = "My amazing Python project"
authors = [{name = "Your Name", email = "you@example.com"}]
dependencies = [
    "numpy>=1.20.0",
    "pandas>=1.3.0",
]

[project.optional-dependencies]
dev = [
    "pytest>=6.0",
    "black>=21.0",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

---

## Package Management

### ➕ Adding Packages

#### Basic Package Installation
```bash
# Add a package
uv add numpy

# Add specific version
uv add "pandas>=1.3.0"

# Add multiple packages
uv add numpy pandas matplotlib
```

#### Development Dependencies
```bash
# Add development-only packages
uv add --dev pytest black flake8

# These won't be installed in production
```

#### Optional Dependencies
```bash
# Add optional dependencies
uv add --optional=dev pytest
uv add --optional=docs sphinx
```

### 📋 Viewing Installed Packages

```bash
# List all packages
uv pip list

# Show package details
uv pip show numpy

# Check outdated packages
uv pip list --outdated
```

### 🔄 Updating Packages

```bash
# Update specific package
uv add "numpy>=1.25.0"

# Update all packages (careful!)
uv lock --upgrade
```

### ➖ Removing Packages

```bash
# Remove a package
uv remove numpy

# Remove multiple packages
uv remove numpy pandas matplotlib
```

### 📥 Installing from Requirements

```bash
# From requirements.txt
uv pip install -r requirements.txt

# From pyproject.toml (recommended)
uv sync
```

---

## Virtual Environment Management

### 🤖 Automatic Environment (Recommended)

```bash
# UV handles everything automatically!
cd my-project
uv add numpy        # Installs to .venv automatically
uv run python       # Uses .venv automatically
```

### 🛠️ Manual Environment Creation (Advanced)

```bash
# Create named environment
uv venv my-special-env

# Create with specific Python version
uv venv --python 3.11 my-env

# Create in specific location
uv venv /path/to/my-env
```

### 🔍 Environment Information

```bash
# Show current environment info
uv pip list

# Show environment path
uv run python -c "import sys; print(sys.executable)"

# Check Python version
uv run python --version
```

### 🧹 Cleaning Environments

```bash
# Remove .venv (will be recreated automatically)
rm -rf .venv

# UV will recreate when you run commands
uv add numpy  # Creates new .venv
```

---

## Running Code

### 🏃‍♂️ Running Python Scripts

```bash
# Run Python file
uv run python script.py

# Run Python module
uv run python -m pytest

# Run with arguments
uv run python script.py --arg1 value1
```

### 🎯 Direct Module Execution

```bash
# Run pytest
uv run pytest

# Run black formatter
uv run black .

# Run custom script
uv run my-script
```

### 🔧 Interactive Python

```bash
# Start Python REPL
uv run python

# Start IPython (if installed)
uv run ipython

# Start Jupyter notebook
uv run jupyter notebook
```

---

## Configuration & Settings

### ⚙️ Global Configuration

```bash
# Show UV configuration
uv config

# Set global Python version
uv python pin 3.11

# Show available Python versions
uv python list
```

### 📁 Project Configuration

#### pyproject.toml Settings
```toml
[tool.uv]
dev-dependencies = [
    "pytest>=6.0",
    "black>=21.0",
]

[tool.uv.sources]
my-package = { git = "https://github.com/user/repo.git" }
```

### 🌍 Environment Variables

```bash
# Custom cache directory
export UV_CACHE_DIR=/path/to/cache

# Disable cache
export UV_NO_CACHE=1

# Custom index URL
export UV_INDEX_URL=https://my-pypi.com/simple
```

---

## Common Workflows

### 🚀 Starting a New Project

```bash
# 1. Create project
uv init my-web-app
cd my-web-app

# 2. Add dependencies
uv add flask requests

# 3. Add development tools
uv add --dev pytest black

# 4. Create your code
echo "print('Hello, UV!')" > app.py

# 5. Run your code
uv run python app.py
```

### 🔄 Working on Existing Project

```bash
# 1. Clone repository
git clone https://github.com/user/project.git
cd project

# 2. Install dependencies
uv sync

# 3. Run tests
uv run pytest

# 4. Start development
uv run python main.py
```

### 📦 Building and Publishing

```bash
# Build package
uv build

# Publish to PyPI
uv publish

# Publish to test PyPI
uv publish --repository testpypi
```

### 🧪 Testing Workflow

```bash
# Add testing dependencies
uv add --dev pytest pytest-cov

# Run tests
uv run pytest

# Run with coverage
uv run pytest --cov=src

# Run specific test
uv run pytest tests/test_specific.py
```

---

## Troubleshooting

### ❌ Common Issues and Solutions

#### Issue: "Package not found"
```bash
# Problem: Package doesn't exist or name is wrong
uv add non-existent-package

# Solution: Check package name on PyPI
# Use exact package name
uv add numpy  # correct
```

#### Issue: "Python version conflict"
```bash
# Problem: Package requires different Python version
# Solution: Check Python version
uv run python --version

# Use specific Python version
uv venv --python 3.11
```

#### Issue: "Environment not activated"
```bash
# Problem: Trying to use regular pip instead of uv
pip install numpy  # Wrong!

# Solution: Use uv commands
uv add numpy      # Correct!
```

#### Issue: "Dependencies conflict"
```bash
# Problem: Package versions conflict
# Solution: Check dependency tree
uv pip tree

# Resolve conflicts manually
uv add "package>=1.0,<2.0"
```

### 🔧 Debug Commands

```bash
# Verbose output
uv add numpy --verbose

# Show what UV is doing
uv --verbose run python script.py

# Check UV cache
uv cache info

# Clean cache
uv cache prune
```

### 🆘 Emergency Reset

```bash
# Nuclear option: Start completely fresh
rm -rf .venv uv.lock
uv sync
```

---

## Comparison with Other Tools

### 📊 UV vs Traditional Tools

| Task | Traditional Way | UV Way |
|------|----------------|---------|
| Create environment | `python -m venv env` | `uv init` (automatic) |
| Activate environment | `source env/bin/activate` | Not needed! |
| Install package | `pip install numpy` | `uv add numpy` |
| Install from file | `pip install -r requirements.txt` | `uv sync` |
| Run script | `python script.py` | `uv run python script.py` |
| List packages | `pip list` | `uv pip list` |
| Update packages | `pip install --upgrade package` | `uv add package@latest` |

### ⚡ Speed Comparison

```bash
# Traditional pip (slow)
time pip install numpy pandas matplotlib
# → ~30-60 seconds

# UV (fast)
time uv add numpy pandas matplotlib
# → ~3-5 seconds
```

### 🎯 When to Use What

**Use UV when:**
- Starting new projects ✅
- Want speed and convenience ✅
- Working with modern Python (3.8+) ✅
- Need automatic dependency management ✅

**Stick with pip when:**
- Legacy systems that require it
- Specific organizational requirements
- Very specialized use cases

---

## Advanced Features

### 🔐 Lock Files

```bash
# Generate lock file (automatic)
uv lock

# Install from lock file (exact versions)
uv sync

# Update lock file
uv lock --upgrade
```

### 🌐 Custom Package Indices

```bash
# Add custom index
uv pip install --index-url https://my-pypi.com/simple numpy

# Add extra index
uv pip install --extra-index-url https://my-pypi.com/simple numpy
```

### 🔗 Git Dependencies

```bash
# Install from Git
uv add git+https://github.com/user/repo.git

# Specific branch/tag
uv add "git+https://github.com/user/repo.git@main"

# Specific commit
uv add "git+https://github.com/user/repo.git@abc123"
```

### 📁 Local Dependencies

```bash
# Install local package
uv add --editable ./my-local-package

# Install from local path
uv add file:///path/to/package
```

### 🐍 Python Version Management

```bash
# Install specific Python version
uv python install 3.11

# Use specific Python for project
uv python pin 3.11

# List available Python versions
uv python list
```

---

## Best Practices

### ✅ Do's and Don'ts

#### ✅ DO:
- Use `uv init` for new projects
- Let UV manage environments automatically
- Use `uv add` instead of `pip install`
- Commit `pyproject.toml` and `uv.lock` to version control
- Use `uv run` to execute scripts
- Keep dependencies organized (main vs dev)

#### ❌ DON'T:
- Manually activate/deactivate environments
- Edit `.venv` folder contents
- Mix pip and uv commands
- Delete `uv.lock` file unless necessary
- Install packages globally when working on projects

### 📋 Project Checklist

Starting a new project:
- [ ] `uv init project-name`
- [ ] Add main dependencies with `uv add`
- [ ] Add dev dependencies with `uv add --dev`
- [ ] Create README.md
- [ ] Initialize git repository
- [ ] Test with `uv run python`

Joining existing project:
- [ ] Clone repository
- [ ] `uv sync` to install dependencies
- [ ] `uv run pytest` to run tests
- [ ] Check `pyproject.toml` for project details

### 🔄 Development Workflow

```bash
# Daily workflow
cd my-project           # Enter project
uv sync                # Sync dependencies (if needed)
uv run pytest          # Run tests
uv run python main.py   # Run application
uv add new-package      # Add new dependencies
git add .               # Commit changes (including uv.lock)
git commit -m "Add feature"
git push
```

---

## Quick Reference

### 📚 Essential Commands

```bash
# Project Management
uv init [project-name]      # Create new project
uv sync                     # Install all dependencies

# Package Management
uv add package             # Add package
uv add --dev package       # Add dev dependency
uv remove package          # Remove package
uv pip list               # List packages

# Running Code
uv run python script.py   # Run Python script
uv run pytest            # Run tests
uv run python            # Start Python REPL

# Information
uv --version              # UV version
uv pip show package       # Package info
uv python list           # Available Python versions
```

### 🆘 Emergency Commands

```bash
# Reset project environment
rm -rf .venv && uv sync

# Clear UV cache
uv cache prune

# Verbose debugging
uv --verbose [command]
```

### 📁 Important Files

```
.venv/           # Virtual environment (auto-managed)
pyproject.toml   # Project configuration (edit this)
uv.lock         # Lock file (don't edit manually)
```

### 🎯 Mental Model

Think of UV as your **smart assistant** that:
1. **Knows** what your project needs
2. **Creates** environments automatically
3. **Installs** packages super fast
4. **Runs** your code in the right environment
5. **Keeps** everything organized

---

## Summary

**UV is like having a smart Python assistant that:**
- Sets up everything automatically
- Keeps your projects isolated and clean
- Installs packages lightning fast
- Never makes you think about virtual environments
- Just works!

**Remember:** With UV, you focus on writing code, not managing environments.

---

*Happy coding with UV! 🚀*

---

## Notes Section

*Use this space for your personal notes and discoveries:*

**My UV Setup:**
- Version: ___________
- Favorite commands: ___________
- Projects using UV: ___________

**Things I learned:**
- ___________
- ___________
- ___________

**Questions to explore:**
- ___________
- ___________
- ___________
