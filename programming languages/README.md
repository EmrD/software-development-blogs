# How Programming Languages Work: Compilation vs. Interpretation

The software we use in our daily lives runs thanks to high-level code written by developers. However, computer processors cannot understand this code directly. In this article, you can find how the code we write is converted into a format computers can understand, along with the methods programming languages use during this process.

## High-Level vs. Low-Level Languages

Programming languages are categorized into levels based on their proximity to human language versus computer hardware. The term "level" here does not indicate the quality of the language, but rather its level of abstraction.

### Low-Level Languages
These are languages that are very close to machine code (0s and 1s). **Assembly** and **Machine Code** fall into this group. They provide direct control over hardware, but writing and understanding them is quite difficult.

### High-Level Languages
These languages have a syntax close to human language (specifically English). Frequently used languages today such as **Python**, **Java**, **C#**, and **JavaScript** belong to this group. They simplify the developer's workflow, but they must be converted into machine language before execution.

## Converting Code to Machine Language

For high-level code to be executed by a processor (CPU), two main methods are used: **Compilation** and **Interpretation**.

``[High-Level Code] ==> (Translator Mechanism) ==> [010101... Machine Code]``

## Compiled Languages

In compiled languages, the entirety of the written code is converted into machine code all at once by a special program called a **Compiler**. As a result of this conversion, an executable file like an `.exe` or a binary is generated.

### Working Principle
1. The source code is sent to the compiler.
2. The compiler checks the entire code for syntax errors.
3. If there are no errors, the entire code is translated into the target platform's machine language and an executable file is created.
4. When the user runs the program, compilation is not repeated; the pre-compiled executable file runs directly.

**Example Languages:** C, C++, Go, Rust.

## Interpreted Languages

In interpreted languages, there is no compilation step or resulting binary file. Instead, a program called an **Interpreter** reads the source code line by line, translating and executing it on the fly.

### Working Principle
1. The interpreter reads the first line of code.
2. It converts that line into machine language and executes it.
3. It moves to the next line and repeats the same process.
4. If an error is encountered on any line, execution stops immediately at that point.

**Example Languages:** Python, JavaScript, Ruby, PHP.

## Compilation vs. Interpretation Comparison

To see the core differences between the two approaches more clearly, you can check the table below:

| Feature | Compiled Languages | Interpreted Languages |
| :--- | :--- | :--- |
| **Execution Speed** | Very high (Since it is pre-translated) | Slower compared to compiled languages |
| **Error Detection** | All errors are caught before execution | Stops right when it reaches the line with the error |
| **Platform Independence** | Specific to the compiled platform (Windows, Linux, etc.) | Runs on any platform that has the interpreter |
| **Extra File** | Produces executable files (e.g., `.exe`) | Does not produce extra files; source code is required |

## Hybrid Approach (Bytecode and JIT)

To offer both performance and flexibility, many modern programming languages use a hybrid architecture that combines both compilation and interpretation steps.

### Bytecode Concept
For example, code written in languages like **Java** or **C#** is not compiled directly into machine code. Instead, it is first converted into an intermediate code called **Bytecode** (`.class` or `.dll`). This bytecode is then interpreted and executed by a virtual machine (**JVM** or **.NET CLR**).

### JIT (Just-In-Time) Compiler
To boost performance in modern interpreted languages and virtual machines, a **JIT Compiler** is used. While the program is running, the JIT identifies frequently executed code blocks, compiles them into machine code on the fly, and caches them in memory. Thus, when the same function is called again, the fast pre-compiled version runs directly instead of being re-interpreted.

## Summary

In short, computers only understand 0s and 1s. When converting human-oriented code into a language a computer can understand:

- **Compiled Languages** are preferred if speed and hardware control are the primary focus,
- **Interpreted Languages** are preferred if rapid development and platform independence are key,
- **Hybrid Systems** are chosen when aiming for both performance and flexibility.
