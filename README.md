[English](README.md) | [Italiano](README.it.md)

# Lisa - Code Analyzer for LLMs

Lisa (inspired by Lisa Simpson) is a tool designed to simplify source code analysis through Large Language Models (LLMs). Intelligent and analytical like the character it's named after, Lisa helps to study and interpret code with logic and method.

## Lisa in a Nutshell

Here are the essential steps to get started quickly:

1. **Installation**:
   ```bash
   pip install hyperlisa
   ```

2. **Configuration**:
   ```bash
   hyperlisa-configure
   ```

3. **[Optional] Customization**:
   Edit the configuration file:
   - Windows: `C:\Users\<username>\AppData\Local\hyperlisa\combine_config.yaml`
   - Linux/MacOS: `~/.config/hyperlisa/combine_config.yaml`
   ```yaml
   includes:
     - "*.py"    # add other extensions if needed
     - "*.java"
   excludes:
     - ".git"
     - "venv"
   ```

4. **Usage**:
   ```bash
   # From the project directory
   cmb                            # default filename
   # or
   cmb --output ANALYSIS_NAME     # custom filename
   ```

The generated file will be in the current directory with format: `NAME_YYYYMMDD_HHMM.txt`

## Description

Lisa is an essential tool for those who want to analyze their code or study open source projects through Large Language Models. Its main objective is to generate a single text file that maintains all references and structure of the original code, making it easily interpretable by an LLM.

This approach solves one of the most common problems in code analysis with LLMs: file fragmentation and loss of references between different project components.

## Installation and Configuration

### 1. Prerequisites
Before starting, make sure you have:
- Python 3.6 or higher installed on your system
- A code editor (we recommend Visual Studio Code, or VSCode)
- Access to the terminal (we'll see how to use it both from VSCode and from the system)

### 2. Package Installation

#### Using Visual Studio Code (Recommended for beginners)
1. Open VSCode
2. Open your project folder using `File > Open Folder`
   (example: select `C:\projects\my_project` on Windows or `/home/user/projects/my_project` on Linux/MacOS)
3. Open the integrated terminal in VSCode:
   - Press `Ctrl + ` (backtick, the key under Esc)
   or
   - From the menu: `View > Terminal`
4. In the terminal you'll see something like:
   ```bash
   # Windows
   C:\projects\my_project>

   # Linux/MacOS
   user@computer:~/projects/my_project$
   ```
5. Run the installation command:
   ```bash
   pip install hyperlisa
   ```

#### Using the System Terminal
1. Open your system's terminal:
   - **Windows**: Search for "cmd" or "PowerShell" in the Start menu
   - **MacOS**: Search for "Terminal" in Spotlight (Cmd + Space)
   - **Linux**: Use the shortcut Ctrl + Alt + T or search for "Terminal"
2. Navigate to your project folder:
   ```bash
   # Windows
   cd C:\projects\my_project

   # Linux/MacOS
   cd ~/projects/my_project
   ```
3. Run the installation command:
   ```bash
   pip install hyperlisa
   ```

### 3. Post-Installation Configuration
After installation, you **must** run the configuration command.

#### From VSCode or system terminal:
```bash
# The prompt might look like this on Windows:
C:\projects\my_project> hyperlisa-configure

# Or like this on Linux/MacOS:
user@computer:~/projects/my_project$ hyperlisa-configure
```

This command should show a series of messages like these:
```
Configuring HyperLisa...
✓ Creating configuration directory
✓ Generating default configuration file
✓ Setting permissions
Configuration completed successfully!
```

The configuration command performs the following operations:
1. Creates Lisa's configuration directory:
   - Windows: `C:\Users\<username>\AppData\Local\hyperlisa\`
   - Linux/MacOS: `~/.config/hyperlisa/`
2. Generates the default configuration file `combine_config.yaml` in the created directory
3. Sets the correct permissions for files and directories:
   - Windows: read and write permissions for the current user
   - Linux/MacOS: 755 permissions for directories and 644 for files

If you run the command and see permission-related errors:
- **Windows**: Run Command Prompt or PowerShell as administrator
- **Linux/MacOS**: Use `sudo hyperlisa-configure` (you'll be asked for your password)

> **IMPORTANT**: 
> - This command should be run only once after installation
> - If run again, the command will first check for existing configuration:
>   - If it finds existing configuration, it will ask for confirmation before overwriting
>   - In case of overwrite, previous customizations will be lost
>   - If you want to keep customizations, make a backup copy of the `combine_config.yaml` file before running the command again

### 4. Configuration File
The `combine_config.yaml` file allows you to customize which files to include or exclude from the analysis. The default configuration is:

```yaml
# Inclusion patterns (extensions or directories to include)
includes:
  - "*.py"  
  # You can add other extensions or directories

# Exclusion patterns (directories or files to exclude)
excludes:
  - ".git"
  - "__pycache__"
  - "*.egg-info"
  - "venv*"
  - ".vscode"
  - "agents*"
  - "log"
```

#### Inclusion/Exclusion Patterns
- Patterns in `includes` determine which files will be processed (e.g., "*.py" includes all Python files)
- Patterns in `excludes` specify which files or directories to ignore
- You can use the * character as a wildcard
- Patterns are applied to both file names and directory paths
- **Important**: Exclusion rules always take priority over inclusion rules

#### Pattern Examples
```
Example 1:
C:\projects\my_project    # Windows
/projects/my_project      # Linux/MacOS
    /src_code
        /utils
            /logs
                file1.py
                file2.py
            helpers.py
```
If we have these rules:
- includes: ["*.py"]
- excludes: ["*logs"]

In this case, `file1.py` and `file2.py` will NOT be included despite having the .py extension because they are in a directory that matches the "*logs" exclusion pattern. The `helpers.py` file will be included.

```
Example 2:
C:\projects\my_project    # Windows
/projects/my_project      # Linux/MacOS
    /includes_dir
        /excluded_subdir
            important.py
```
If we have these rules:
- includes: ["includes_dir"]
- excludes: ["*excluded*"]

In this case, `important.py` will NOT be included because it's in a directory that matches an exclusion pattern, even though its parent directory matches an inclusion pattern.

## Usage

The program can be run using one of the following commands from the terminal:

```bash
# Windows
C:\projects\my_project> cmb [options]

# Linux/MacOS
user@computer:~/projects/my_project$ cmb [options]
```

Alternative commands are also available:
- `combine-code`: complete original command
- `lisacmb`: descriptive alias
- `hyperlisacmb`: even more descriptive alias

### Default Structure and Name
To understand which filename will be used by default, consider this structure:

```
# Windows
C:\projects
    \my_test_project     <- This is the root directory
        \src
            main.py
        \tests
            test_main.py

# Linux/MacOS
/home/user/projects
    /my_test_project     <- This is the root directory
        /src
            main.py
        /tests
            test_main.py
```

In this case, the default name will be "MY_TEST_PROJECT" (the root directory name in uppercase).

### Available parameters:

- `--clean`: Removes previously generated text files
- `--output NAME`: Specifies the output file name prefix
  ```bash
  # Windows
  # Example with default name
  C:\projects\my_project> cmb
  # Output: MY_PROJECT_20240327_1423.txt

  # Example with custom name
  C:\projects\my_project> cmb --output PROJECT_ANALYSIS
  # Output: PROJECT_ANALYSIS_20240327_1423.txt

  # Linux/MacOS
  # Example with default name
  user@computer:~/projects/my_project$ cmb
  # Output: MY_PROJECT_20240327_1423.txt

  # Example with custom name
  user@computer:~/projects/my_project$ cmb --output PROJECT_ANALYSIS
  # Output: PROJECT_ANALYSIS_20240327_1423.txt
  ```

### Output

The script generates a text file with the format:
`NAME_YYYYMMDD_HHMM.txt`

where:
- `NAME` is the prefix specified with --output or the default one
- `YYYYMMDD_HHMM` is the generation timestamp

## Usage with GitHub Projects

To use Lisa with a GitHub project, follow these steps:

1. **Environment preparation**:
   ```bash
   # Windows
   C:> mkdir C:\projects
   C:> cd C:\projects

   # Linux/MacOS
   $ mkdir ~/projects
   $ cd ~/projects
   ```

2. **Clone the project to analyze**:
   ```bash
   # Example with a hypothetical "moon_project"
   # Windows/Linux/MacOS
   git clone https://github.com/user/moon_project.git
   ```

3. **Install and configure Lisa**:
   ```bash
   # Windows/Linux/MacOS
   pip install hyperlisa
   hyperlisa-configure
   ```

4. **Run the analysis**:
   ```bash
   # Windows
   C:\projects\moon_project> cmb

   # Linux/MacOS
   user@computer:~/projects/moon_project$ cmb
   ```

### Best Practices for Analysis
- Before running Lisa, make sure you're in the root directory of the project to analyze
- Check and customize the `combine_config.yaml` file according to the specific needs of the project
- Use the `--clean` option to keep the directory tidy when generating multiple versions

## Additional Notes

- Lisa maintains the hierarchical structure of files in the generated document
- Each file is clearly delimited by separators indicating its relative path
- Code is organized maintaining directory depth order
- Generated files can be easily shared with LLMs for analysis

## Using the Generated File with LLMs

Lisa generates a file that can be effectively used with various Large Language Models. Here's a practical example of how to make the most of this tool.

### Example: Analyzing LangChain
Let's say we want to use the LangChain library but aren't familiar with its structure or its most recent features.

1. **Preparation**:
   ```bash
   # Clone LangChain
   git clone https://github.com/langchain-ai/langchain.git
   cd langchain

   # Generate the analysis file with Lisa
   cmb --output LANGCHAIN_ANALYSIS
   ```

2. **Using with ChatGPT/Claude**:
   Upload the generated file `LANGCHAIN_ANALYSIS_20240327_1423.txt` to the chat. You can use prompts like these:

   ```
   I've generated a source code analysis of LangChain using Lisa.
   The file contains the complete code structure with all references.
   Please:
   1. Analyze the code structure
   2. Identify the main modules
   3. Suggest the best way to implement [describe your use case]
   ```

   Specific prompt examples:

   **To explore recent features**:
   ```
   In the code I've provided, look for the most recent implementations
   for OpenAI model integration. I want to create a chain that uses 
   GPT-4 to analyze PDF documents, extracting key information and 
   generating a summary. Can you show me the necessary code using 
   the latest available APIs?
   ```

   **To understand specific parts**:
   ```
   Analyze how LangChain implements memory management in conversations. 
   I want to create a chatbot that maintains context from previous 
   conversations but optimizes token usage. Can you explain how it 
   works and provide an implementation example based on the current code?
   ```

   **For custom projects**:
   ```
   Based on the provided source code, help me create a custom agent that:
   1. Accesses a SQL database
   2. Processes natural language queries
   3. Generates and executes appropriate SQL queries
   4. Formats results in a user-friendly way
   Show me the necessary code using the most suitable LangChain 
   components.
   ```

### Advantages of This Approach
- **Access to Latest Features**: The LLM can see the most recent code, even if not yet documented
- **Deep Understanding**: Having access to the complete source code, the LLM can provide more accurate and contextualized suggestions
- **Effective Debugging**: If you encounter issues, you can ask the LLM to analyze specific implementations
- **Informed Customization**: You can create custom solutions based on actual internal implementations

### Tips for Effective Use
1. **Be Specific**: Clearly describe your use case and desired functionality
2. **Ask for Explanations**: If something isn't clear, ask questions about internal workings
3. **Iterate**: Use the LLM's responses to refine your questions and get better solutions
4. **Verify**: Always test generated code and ask for clarification if needed
5. **Explore Alternatives**: Ask the LLM to suggest different approaches based on the source code

## Contributing

If you want to contribute to the project, you can:
- Open issues to report bugs or propose improvements
- Submit pull requests with new features
- Improve documentation
- Share your use cases and suggestions

## License

MIT License

Copyright (c) 2024

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.