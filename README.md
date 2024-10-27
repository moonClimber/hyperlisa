[English](README.md) | [Italiano](README.it.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Español](README.es.md)

# Lisa - Code Analyzer for LLMs

Lisa (inspired by Lisa Simpson) is a tool designed to simplify source code analysis through Large Language Models (LLMs). Intelligent and analytical like the character it's named after, Lisa helps study and interpret code with logic and method.

## Description

Lisa is an essential tool for those who want to analyze their code or study open source projects through Large Language Models. Its main objective is to generate a single text file that maintains all references and structure of the original code, making it easily interpretable by an LLM.

This approach solves one of the most common problems in code analysis with LLMs: file fragmentation and loss of references between different project components.

## Configuration

The project uses a `combine_config.yaml` configuration file that allows you to customize which files to include or exclude from the analysis. The default configuration is:

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

### Inclusion/Exclusion Patterns
- Patterns in `includes` determine which files will be processed (e.g., "*.py" includes all Python files)
- Patterns in `excludes` specify which files or directories to ignore
- You can use the * character as a wildcard
- Patterns are applied to both file names and directory paths
- **Important**: Exclusion rules always take priority over inclusion rules

### Rule Priority
When there are "conflicts" between inclusion and exclusion rules, exclusion rules always take precedence. Here are some examples:

```
Example 1:
/project_root
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
/project_root
    /includes_dir
        /excluded_subdir
            important.py
```
If we have these rules:
- includes: ["includes_dir"]
- excludes: ["*excluded*"]

In this case, `important.py` will NOT be included because it's in a directory that matches an exclusion pattern, even though its parent directory matches an inclusion pattern.

## Usage

The script is run from the command line with:

```bash
cmb [options]
```

> **Note**: The leading underscore in the filename is intentional and allows for shell tab completion.

### Default Structure and Name
To understand which filename will be used by default, consider this structure:

```
/home/user/projects
    /my_test_project     <- This is the root directory
        /scripts
            _combine_code.py
            combine_config.yaml
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
  # Example with default name (from structure above)
  python \scripts\_combine_code.py
  # Output: MY_TEST_PROJECT_20240327_1423.txt

  # Example with custom name
  python \scripts\_combine_code.py --output PROJECT_ANALYSIS
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
   # Create and access a directory for your projects
   mkdir ~/projects
   cd ~/projects
   ```

2. **Clone the project to analyze**:
   ```bash
   # Example with a hypothetical "moon_project"
   git clone moon_project.git
   ```

3. **Integrate Lisa into the project**:
   ```bash
   # Clone Lisa's repository
   git clone https://github.com/yourname/lisa.git

   # Copy Lisa's scripts folder into moon_project
   cp -r lisa/scripts moon_project/
   cp lisa/scripts/combine_config.yaml moon_project/scripts/
   ```

4. **Run the analysis**:
   ```bash
   cd moon_project
   python scripts/_combine_code.py
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