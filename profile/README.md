# Introduction

This is an introduction to getting started with Python.

Contact: <majp@ramboll.com>

Please don't hestitate to contact me if something doesn't work. I wrote this off the top of my head, so I probably missed something...

## Installing a package manager for Python

We will be using the [`micromamba`](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html) package manager. It is a standalone package manager based on [`conda`](https://docs.conda.io/en/latest/).

### Windows

Open a `PowerShell` terminal and run

```powershell
Invoke-Expression ((Invoke-WebRequest -Uri https://micro.mamba.pm/install.ps1).Content)
```

This will place `micromamba.exe` at `C:\Users\<you>\AppData\Local\micromamba\micromamba.exe`.

Next, initialize your shell with

```powershell
C:\Users\<you>\AppData\Local\micromamba\micromamba.exe shell init -s powershell -p ~\micromamba
```

Create a file in your home directory (`C:\Users\<you>` for Windows, `/home/<your>` for Linux) named `.mambarc`.

Paste the following contents into it:

```yaml
channels:
  - defaults
  - conda-forge
update_dependencies: true
```

Confirm that `micromamba` was installed successfully with

```powershell
micromamba info
```

This should list some information about your micromamba installation. Done!

### Linux

Open your favourite terminal and run

```bash
"${SHELL}" <(curl -L https://micro.mamba.pm/install.sh)
```

Next, initialize your shell with

```bash
./bin/micromamba shell init -s bash -p ~/micromamba
```

Confirm that `micromamba` was installed successfully with

```powershell
micromamba info
```

This should list some information about your micromamba installation. Done!

## Create your first environment

The Python programming language uses so-called *packages* that contain reusable code (functions, classes, modules). For example, the `numpy` package provides functionality for fast mathematical operations, and the `polars` package enables easy and fast handling of tabular data.

An *environment* refers to the set of packages that the Python interpreter has access to. The Python interpreter is the process that executes your Python code.

Let's create an environment called `aau`. Open your terminal (`PowerShell` on Windows, `bash` on Linux) and run

```shell
micromamba create --name aau python numpy polars
```

The `--name` argument, which has the value `aau`, is the name for your environment. The values after that are the packages that will be installed in that environment.

After you've created an environment, you need to *activate* it. Python only has access to the packages in the currently active environment. You do this with

```shell
micromamba activate aau
```

## Installing a code editor

We're now ready to install a code editor. The code editor is the programme in which you write your code. It can provide you with many tools that help you write better code.

We will be use Microsoft Visual Studio Code (VS Code for short). This editor has a rich extension ecosystem, and you can find extensions for pretty much anything, including writing and running Python code!

### Windows

> Note: You might need to activate `winget`. To do so, install the ["App Installer"](https://apps.microsoft.com/detail/app-installer/9NBLGGH4NNS1?hl=en-US&gl=US) package from Microsoft Store.

To install VS Code on Windows, execute the following in `PowerShell`:

```powershell
winget install -e --id Microsoft.VisualStudioCode --override '/mergetasks="addcontextmenufiles,addcontextmenufolders,addtopath"'
```

Now you should be able to open VS Code by running

```powershell
code
```

### Linux

TODO

### Configure VS Code

We'll need to configure a few things and install some extensions.

### User configuration

Your user (system-wide) configuration for VS Code is stored as a JSON file in `$Env:APPDATA\Code\User\settings.json`.

You can open that file with

```powershell
code $Env:APPDATA\Code\User\settings.json
```

and then paste the following JSON into the file:

```json
{
    // LANGUAGE-SPECIFIC -----------------------------------------------------
    "[python]": {
        "editor.codeActionsOnSave": {
            "source.organizeImports": true
        },
        "editor.defaultFormatter": "ms-python.black-formatter",
        "editor.formatOnSave": true,
        "editor.formatOnSaveMode": "modifications",
        "editor.formatOnType": false
    },
    // PYTHON ---------------------------------------------------------------
    "black-formatter.args": [
        "--line-length=120"
    ],
    "isort.args": [
        "--profile=black",
        "--line-length=120"
    ],
    "python.analysis.autoImportCompletions": true,
    "python.languageServer": "Pylance",
    "python.terminal.activateEnvInCurrentTerminal": false,
    "python.testing.promptToConfigure": false,
    "python.testing.pytestArgs": [
        "-vv"
    ],
    "python.testing.pytestEnabled": true,
    "pylint.importStrategy": "fromEnvironment",
    "python.analysis.importFormat": "relative",
    "python.terminal.activateEnvironment": false,
    "python.condaPath": "$MAMBA_EXE",
    "python.experiments.enabled": false,
    // GENERAL ---------------------------------------------------------------
    "debug.console.wordWrap": false,
    "editor.fontFamily": "'CaskaydiaCove NF Mono', Consolas, 'Courier New', monospace",
    "editor.fontLigatures": true,
    "editor.formatOnSave": true,
    "editor.minimap.enabled": true,
    "editor.renderWhitespace": "all",
    "editor.rulers": [
        120
    ],
    "editor.unicodeHighlight.allowedCharacters": {
        "️": true
    },
    "editor.wordWrapColumn": 120,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "files.autoSave": "afterDelay",
    "vsicons.dontShowNewVersionMessage": true,
    "window.commandCenter": true,
    "workbench.iconTheme": "vscode-icons",
    "workbench.startupEditor": "none",
    "workbench.tree.indent": 12,
    "workbench.tree.renderIndentGuides": "always",
    "redhat.telemetry.enabled": false,
    "security.workspace.trust.untrustedFiles": "open",
    // FILE FORMATS ----------------------------------------------------------
    "files.exclude": {
        "**/.DS_Store": true,
        "**/.git": false,
        "**/.hg": true,
        "**/.svn": true,
        "**/*__pycache__**": false,
        "**/*.aux": true,
        "**/*.bbl": true,
        "**/*.blg": true,
        "**/*.fdb_latexmk": true,
        "**/*.fls": true,
        "**/*.log": true,
        "**/*.out": true,
        "**/*.synctex.gz": true,
        "**/*.toc": true,
        "**/CVS": true
    },
    "json.maxItemsComputed": 2000,
    // GIT -------------------------------------------------------------------
    "diffEditor.ignoreTrimWhitespace": false,
    "git-graph.date.type": "Commit Date",
    "git-graph.dialog.createBranch.checkOut": true,
    "git-graph.graph.style": "angular",
    "git-graph.repository.commits.initialLoad": 75,
    "git-graph.repository.fetchAndPrune": true,
    "git-graph.repository.fetchAndPruneTags": true,
    "git.autofetch": true,
    "git.confirmSync": false,
    "git.openRepositoryInParentFolders": "never",
    "git.suggestSmartCommit": false,
    "gitlens.defaultDateShortFormat": "DD-MM-YYYY",
    "gitlens.defaultTimeFormat": "hh:mm",
    "gitlens.hovers.currentLine.over": "line",
    // TERMINAL --------------------------------------------------------------
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.defaultProfile.linux": "bash",
    "terminal.integrated.defaultProfile.windows": "PowerShell",
    "terminal.integrated.enableMultiLinePasteWarning": false,
    "terminal.integrated.fontFamily": "'CaskaydiaCove NF Mono'",
    "terminal.integrated.profiles.windows": {
        "PowerShell": {
            "args": [
                "-ExecutionPolicy",
                "ByPass",
                "-NoExit",
                "-nologo"
            ],
            "source": "PowerShell"
        }
    },
    "terminal.integrated.rightClickBehavior": "default",
    "terminal.integrated.automationProfile.windows": {},
    "debug.allowBreakpointsEverywhere": true
}
```

#### Extensions

There are many extensions available for VS Code. We'll install a few so we can more easily work with Python and enhance the experience.

You can find the extension manager in the sidebar on the left-hand side of the window. The icon is four little squares. Here, you can search for extensions. You can run the code below in your terminal to install some recommended extensions:

```shell
code --install-extension eamodio.gitlens
code --install-extension ms-python.python
code --install-extension ms-python.vscode-pylance
code --install-extension ms-python.black-formatter
code --install-extension ms-python.flake8
code --install-extension ms-python.isort
code --install-extension ms-python.pylint
code --install-extension mhutchie.git-graph
code --install-extension vscode-icons-team.vscode-icons
```

## Installing a version control system

It is extremely beneficial to put your code under version control. For this, we use [`git`](https://git-scm.com/). With version control, you can record the change history of your code and

* See what changes were made
* When those changes were made
* Who made the changes
* Go back in history in case you made mistakes or want to run an older version of your code

### Windows

To install `git`, execute the following in `PowerShell`:

```powershell
winget install --id=Git.Git  -e
```

### Linux

Already installed by default.

### Configure git

Let's configure our `git` installation.

Set your email and username with

```shell
git config --global user.name "your name here"
git config --global user.email my.email@domain.com
```

Set the default editor for `git` to VS Code with

```shell
git config --global core.editor code
```
