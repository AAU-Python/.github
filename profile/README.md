# 1. Introduction

This is an introduction to getting started with Python.

Contact: <majp@ramboll.com>

Please don't hestitate to contact me if something doesn't work. I wrote this off the top of my head, so I probably missed something...

- [1. Introduction](#1-introduction)
  - [1.1. Installing a package manager for Python](#11-installing-a-package-manager-for-python)
    - [1.1.1. Windows](#111-windows)
    - [1.1.2. Linux/MacOS](#112-linuxmacos)
  - [1.2. Create an environment](#12-create-an-environment)
  - [1.3. Installing a code editor](#13-installing-a-code-editor)
    - [1.3.1. Windows](#131-windows)
    - [1.3.2. Linux](#132-linux)
    - [1.3.3. Configure VS Code](#133-configure-vs-code)
    - [1.3.4. User configuration](#134-user-configuration)
      - [1.3.4.1. Extensions](#1341-extensions)
  - [1.4. Installing a version control system](#14-installing-a-version-control-system)
    - [1.4.1. Windows](#141-windows)
    - [1.4.2. Linux](#142-linux)
    - [1.4.3. Configure git](#143-configure-git)

## 1.1. Installing a package manager for Python

We will be using the [`micromamba`](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html) package manager. It is a standalone package manager based on [`conda`](https://docs.conda.io/en/latest/).

### 1.1.1. Windows

Open a `PowerShell` terminal and run

```powershell
Invoke-Expression ((Invoke-WebRequest -Uri https://micro.mamba.pm/install.ps1).Content)
```

This will place `micromamba.exe` at `$Env:LocalAppData\micromamba\micromamba.exe`.

> **NOTE**: `$Env:LocalAppData` is a variable that contains the path to `AppData\Local`. Usually this is located under `C:\Users\<you>\AppData\Local`, where `\Users` depends on the system language and `<you>` is you user name. You can see the value of the path if you run
>
> ```powershell
> echo $Env:LocalAppData
> ```

Next, initialize your shell with

```powershell
$Env:AppData\micromamba\micromamba.exe shell init -s powershell -p ~\micromamba
```

> **NOTE:** Depending one the system's security settings, you might get a permission error later on. Allow unsigned script execution by opening PowerShell as administrator (right click on PowerShell, select "Run as administrator") and executing the following command
>
> ```powershell
> set-executionpolicy remotesigned
>```

Create a text file in your home directory named `.mambarc`.

> **NOTE**: The home directory is the directory defined in the `$Env:Home` variable. You can see the value of this variable with
>
> ```powershell
> echo $Env:Home
> ```

Paste the following contents into the `.mambarc` file:

```yaml
channels:
  - defaults
  - conda-forge
update_dependencies: true
```

Close the PowerShell terminal, and open a new one. Then, confirm that `micromamba` was installed successfully with

```powershell
micromamba info
```

This should list some information about your micromamba installation. Done!

### 1.1.2. Linux/MacOS

Open your favourite terminal and run

```bash
"${SHELL}" <(curl -L https://micro.mamba.pm/install.sh)
```

Next, initialize your shell with

```bash
./bin/micromamba shell init -s bash -p ~/micromamba
```

Create a text file in your home directory named `.mambarc`.

> **NOTE:** The home directory is defined in the `$HOME` variable.You can see the value of this variable with
>
> ```bash
> echo $HOME
> ```

Paste the following contents into it:

```yaml
channels:
  - defaults
  - conda-forge
update_dependencies: true
```

Close the terminal and open a new one. Then, confirm that `micromamba` was installed successfully with

```shell
micromamba info
```

This should list some information about your micromamba installation. Done!

## 1.2. Create an environment

The Python programming language uses so-called *packages* that contain reusable code (functions, classes, modules). For example, the `numpy` package provides functionality for fast mathematical operations, and the `polars` package enables easy and fast handling of tabular data.

An *environment* refers to the set of packages that the Python interpreter has access to. The Python interpreter is the process that executes your Python code.

There are several ways of creating environments. Often, you will have an `environment.yml` file which specified what packages to install and what name the environment has.

You can create an environment from a file with

```shell
micromamba env create -f <path to environment.yml>
```

where you replace `<path to environment.yml>` with the actual path to the file. During environment creation you will get a `[Y/n]` prompt. To answer the prompt, type out your answer (in this case `Y`) and hit `Enter`

After you've created an environment, you need to *activate* it. Python only has access to the packages in the currently active environment. You do this with

```shell
micromamba activate <environment name>
```

where you replace `<environment name>` with the environment name. The name is listed in the `name:` section in `environment.yml`, like so:

```yaml
name: my_environment
```

## 1.3. Installing a code editor

We're now ready to install a code editor. The code editor is the programme in which you write your code. It can provide you with many tools that help you write better code.

We will be use Microsoft Visual Studio Code (VS Code for short). This editor has a rich extension ecosystem, and you can find extensions for pretty much anything, including writing and running Python code!

### 1.3.1. Windows

> Note: You might need to activate `winget`. To do so, install the ["App Installer"](https://apps.microsoft.com/detail/app-installer/9NBLGGH4NNS1?hl=en-US&gl=US) package from Microsoft Store.

To install VS Code on Windows, execute the following in `PowerShell`:

```powershell
winget install -e --id Microsoft.VisualStudioCode --override
```

If an installer opens up, make sure to select the `Add code to PATH` option if not already selected.

Close PowerShell and open a new PowerShell temrinal for changes to take effect. Confirm that VS Code was succesfully installed by running

```powershell
code --version
```

> **NOTE**: If this didn't work, you can manually add VS Code to your path variable with this command:
>
> ```powershell
> $Env:Path += ";$Env:LocalAppdata\Programs\Microsoft VS Code\"
> ```
>
> Close PowerShell, open a new PowerShell terminal, and try to run `code --version` again.

### 1.3.2. Linux

TODO

### 1.3.3. Configure VS Code

We'll need to configure a few things and install some extensions.

### 1.3.4. User configuration

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

#### 1.3.4.1. Extensions

There are many extensions available for VS Code. We'll install a few so we can more easily work with Python and enhance the experience.

You can find the extension manager in the sidebar on the left-hand side of the window. The icon is four little squares. Here, you can search for extensions. You can run the code below in your terminal to install some recommended extensions:

For Windows:

```powershell
code --install-extension eamodio.gitlens `
&& code --install-extension ms-python.python `
&& code --install-extension ms-python.vscode-pylance `
&& code --install-extension ms-python.black-formatter `
&& code --install-extension ms-python.flake8 `
&& code --install-extension ms-python.isort `
&& code --install-extension ms-python.pylint `
&& code --install-extension mhutchie.git-graph `
&& code --install-extension vscode-icons-team.vscode-icons `
&& code --install-extension ms-toolsai.jupyter
```

For Linux/MacOS:

```bash
code --install-extension eamodio.gitlens \
&& code --install-extension ms-python.python \
&& code --install-extension ms-python.vscode-pylance \
&& code --install-extension ms-python.black-formatter \
&& code --install-extension ms-python.flake8 \
&& code --install-extension ms-python.isort \
&& code --install-extension ms-python.pylint \
&& code --install-extension mhutchie.git-graph \
&& code --install-extension vscode-icons-team.vscode-icons \
&& code --install-extension ms-toolsai.jupyter
```

## 1.4. Installing a version control system

It is extremely beneficial to put your code under version control. For this, we use [`git`](https://git-scm.com/). With version control, you can record the change history of your code and

- See what changes were made
- When those changes were made
- Who made the changes
- Go back in history in case you made mistakes or want to run an older version of your code

### 1.4.1. Windows

To install `git`, execute the following in `PowerShell`:

```powershell
winget install --id=Git.Git  -e
```

### 1.4.2. Linux

Already installed by default.

### 1.4.3. Configure git

Let's configure our `git` installation.

Set your username with

```shell
git config --global user.name "your name here"
```

where you replace `your name here` with your username. It can be whatever name you like. Set your email with

```shell
git config --global user.email my.email@domain.com
```

where you replace `my.email@domain.com` with your actual email address.

Set the default editor for `git` to VS Code with

```shell
git config --global core.editor code
```
