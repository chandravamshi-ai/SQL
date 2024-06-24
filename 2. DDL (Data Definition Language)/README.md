
### Introduction to Python and Basic Syntax

1. Setting Up Your Environment

   1. **Install Python**
      - Download and install the latest version of Python from the official website: [python.org](https://www.python.org/downloads/).
      - Follow the installation instructions for your operating system (Windows, macOS, Linux).
   
   2. **Install a Code Editor**
      - Choose a code editor or Integrated Development Environment (IDE). Popular choices include:
        - [Visual Studio Code (VS Code)](https://code.visualstudio.com/)
        - [PyCharm](https://www.jetbrains.com/pycharm/)
        - [Sublime Text](https://www.sublimetext.com/)
   
   3. **Verify Installation**
      - Open a terminal (Command Prompt on Windows, Terminal on macOS/Linux) and type `python --version` to verify Python is installed correctly.
      - Example:
        ```sh
        python --version
        ```

2. Writing Your First Python Program

   1. **Create a New Python File**
      - Open your code editor and create a new file called `hello.py`.
   
   2. **Write the "Hello, World!" Program**
      - Type the following code into `hello.py`:
        ```python
        print("Hello, World!")
        ```
   
   3. **Run Your Program**
      - Save the file and run it from the terminal by navigating to the file's directory and typing:
        ```sh
        python hello.py
        ```
      - You should see the output: `Hello, World!`

3. Understanding Basic Syntax

   1. **Comments**
      - Single-line comment: `# This is a comment`
      - Multi-line comment:
        ```python
        """
        This is a
        multi-line comment
        """
        ```
   
   2. **Indentation**
      - Python uses indentation to define code blocks. Ensure consistent use of spaces (typically 4 spaces per indentation level).
      - Example:
        ```python
        if 5 > 2:
            print("Five is greater than two!")  # Indented block
        ```
   
   3. **Python Identifiers**
      - Rules for naming identifiers:
        - Must begin with a letter (A-Z, a-z) or an underscore (_)
        - Followed by letters, digits (0-9), or underscores
        - Case-sensitive (`myVariable` and `myvariable` are different)
   
   4. **Print Statement**
      - Use `print()` to display output.
      - Example:
        ```python
        print("Learning Python is fun!")
        ```

4. Practical Exercises

   1. **Exercise 1: Print Your Name**
      - Write a program that prints your name.
      - Example:
        ```python
        print("My name is Alice.")
        ```
   
   2. **Exercise 2: Simple Arithmetic**
      - Write a program that prints the result of adding two numbers.
      - Example:
        ```python
        print(5 + 3)
        ```
   
   3. **Exercise 3: Comments and Indentation**
      - Write a program with comments and proper indentation.
      - Example:
        ```python
        # This program calculates the sum of two numbers
        num1 = 10
        num2 = 20
        sum = num1 + num2
        print("The sum is:", sum)
        ```

