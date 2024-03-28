# RubyData base stack

```bash
+--------+
|  ruby  +
+--------+
```

This docker image consists of minimal components for data science using Ruby.

Layered on the jupyter/scipy-notebook & 3.1.3 from rubylang/ruby Docker images


Here is the provided text formatted into markdown:


## Docker Image:
This Docker image offers a crafted environment tailored for machine learning, natural language processing, and data science workflows, with a robust foundation for integrating both Python and Ruby-based tools. Here's an updated breakdown:

- **Base Image:** A customized Jupyter/scipy-notebook image (Tag: f3079808ca8c or a designated version) encompassing essential foundational tools.
- **Customization:**
  - **System Updates & Prerequisites:** Installation of commonly employed development tools and libraries (e.g., git, gcc, build essentials, database clients) to facilitate a robust development environment.
  - **LLVM:** Inclusion of the LLVM compiler infrastructure (version 11) for enhanced compilation capabilities.
  - **Python 3.10.7:** Seamless integration of Python 3.10.7, directly copied from the official python:3.10-slim image, ensuring compatibility and reliability.
  - **Ruby 3.1.3:** Seamless integration of Ruby 3.1.3, directly copied from the official rubylang/ruby image, ensuring compatibility and reliability.
- **User & Permissions Setup:** Configuration ensures that the container's primary user possesses appropriate permissions, including sudo access, for efficient resource management.
- **Languages:** Python, Ruby

### Ruby Tools
- **IRB:** The interactive Ruby shell, allowing users to interactively execute Ruby code in the terminal.
- **IRuby:** An implementation of IRB that runs on JRuby, allowing users to interactively execute Ruby code in the terminal on a JVM.
- **Pry:** A gem that provides a REPL (Read-Eval-Print Loop) environment for Ruby, allowing users to interactively explore their code.

### Data Serialization and Parsing
- **JSON:** A gem that provides support for working with JSON data in Ruby.
- **JSONL:** A gem that provides support for working with JSON lines in Ruby.
- **Psych:** A gem that provides support for parsing and generating YAML data in Ruby.
- **REXML:** A gem that provides support for working with XML data in Ruby, allowing users to parse and generate XML documents.

### Database Connectivity and ORM
- **ActiveSupport:** A collection of utility classes and standard library extensions that were found useful for the Rails framework, including support for multibyte strings, internationalization, time zones, and testing.
- **MySQL2:** A gem that provides a database adapter for MySQL databases in Ruby.
- **PG:** A gem that provides a database adapter for PostgreSQL databases in Ruby.
- **Sequel:** An object-oriented SQL database interface for Ruby, supporting multiple databases and database adapters.
- **SQLite3:** A gem that provides a database adapter for SQLite databases in Ruby.

### Logging and Output Formatting
- **Awesome Print:** A gem that beautifies the output of your Ruby objects in the console or IRB, making them easier to read.
- **Colorize:** A gem that adds color support to the console output of Ruby programs.
- **Logging:** A gem that provides support for logging in Ruby, allowing users to log messages to various output streams.
- **Pastel:** A gem that provides support for colorizing text in the console output of Ruby programs.

### Testing Frameworks
- **Minitest:** A gem that provides a testing framework for Ruby, allowing users to write unit and integration tests for their applications.
- **Mocha:** A gem that provides a testing framework for Ruby, allowing users to write behavior-driven development tests for their applications.

### Documentation and Reporting
- **Ruport:** A gem that provides support for generating reports from data in Ruby.
- **RDoc:** A gem that provides support for generating documentation for Ruby programs.
- **Kramdown:** A gem that provides a markdown parser and converter for Ruby, allowing users to convert markdown text to HTML or other formats.

### Version Control
- **Rugged:** A gem that provides support for working with Git repositories in Ruby.

### File and Directory Management
- **Sync:** A gem that provides support for synchronizing files and directories in Ruby.

### Concurrency and Parallel Processing
- **EventMachine:** A gem that provides a concurrency framework for Ruby, allowing users to write concurrent applications using an event-driven model.
- **Parallel:** A gem that provides support for parallel processing in Ruby, allowing users to execute tasks concurrently.

### Foreign Function Interface (FFI)
- **FFI:** A gem that provides a foreign function interface for Ruby, allowing users to call functions written in other languages from Ruby.

### Shell Command Execution and Management
- **ChildProcess:** A gem that provides a cross-platform interface for spawning child processes and managing their input, output, and error streams.
- **TTY-Box:** A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.
- **TTY-Command:** A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.
- **TTY-Config:** A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.

### SSH Client
- **Net-SSH:** A gem that provides a client for connecting to and executing commands on remote SSH servers.




| **Functionality**                      | **Gem**       | **Version**    | **Description**                                                                                                                                                                                         |
|----------------------------------------|---------------|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Interactive Ruby Shell                 | irb           | >= 1.3.8.pre.9 | The interactive Ruby shell, allowing users to interactively execute Ruby code in the terminal.                                                                                                          |
|                                        | iruby         | >= 0.7.4       | An implementation of IRB that runs on JRuby, allowing users to interactively execute Ruby code in the terminal on a JVM.                                                                                |
|                                        | pry           |                | A gem that provides a REPL (Read-Eval-Print Loop) environment for Ruby, allowing users to interactively explore their code.                                                                             |
|                                        | pry-doc       |                | A gem that provides documentation for Pry, the REPL environment for Ruby.                                                                                                                               |
| Data Serialization and Parsing         | json          |                | A gem that provides support for working with JSON data in Ruby.                                                                                                                                         |
|                                        | jsonl         |                | A gem that provides support for working with JSON lines in Ruby.                                                                                                                                        |
|                                        | psych         |                | A gem that provides support for parsing and generating YAML data in Ruby.                                                                                                                               |
|                                        | rexml         |                | A gem that provides support for working with XML data in Ruby, allowing users to parse and generate XML documents.                                                                                      |
|                                        | yaml          |                |                                                                                                                                                                                                         |
| Database Connectivity and ORM          | activesupport |                | A collection of utility classes and standard library extensions that were found useful for the Rails framework, including support for multibyte strings, internationalization, time zones, and testing. |
|                                        | mysql2        |                | A gem that provides a database adapter for MySQL databases in Ruby.                                                                                                                                     |
|                                        | pg            |                | A gem that provides a database adapter for PostgreSQL databases in Ruby.                                                                                                                                |
|                                        | sequel        |                | An object-oriented SQL database interface for Ruby, supporting multiple databases and database adapters.                                                                                                |
|                                        | sqlite3       |                | A gem that provides a database adapter for SQLite databases in Ruby.                                                                                                                                    |
| Logging and Output Formatting          | awesome_print |                | A gem that beautifies the output of your Ruby objects in the console or IRB, making them easier to read.                                                                                                |
|                                        | colorize      |                | A gem that adds color support to the console output of Ruby programs.                                                                                                                                   |
|                                        | logging       |                | A gem that provides support for logging in Ruby, allowing users to log messages to various output streams.                                                                                              |
|                                        | pastel        |                | A gem that provides support for colorizing text in the console output of Ruby programs.                                                                                                                 |
| Testing Frameworks                     | minitest      |                | A gem that provides a testing framework for Ruby, allowing users to write unit and integration tests for their applications.                                                                            |
|                                        | mocha         |                | A gem that provides a testing framework for Ruby, allowing users to write behavior-driven development tests for their applications.                                                                     |
| Documentation and Reporting            | ruport        |                | A gem that provides support for generating reports from data in Ruby.                                                                                                                                   |
|                                        | rdoc          |                | A gem that provides support for generating documentation for Ruby programs.                                                                                                                             |
|                                        | kramdown      |                | A gem that provides a markdown parser and converter for Ruby, allowing users to convert markdown text to HTML or other formats.                                                                         |
| Version Control                        | rugged        |                | A gem that provides support for working with Git repositories in Ruby.                                                                                                                                  |
| File and Directory Management          | sync          |                | A gem that provides support for synchronizing files and directories in Ruby.                                                                                                                            |
|                                        | open3         |                | A gem that provides a cross-platform interface for spawning child processes and managing their input, output, and error streams.                                                                        |
|                                        | open4         |                | A gem that provides a cross-platform interface for spawning child processes and managing their input, output, and error streams, with additional features for managing pipes.                           |
| Concurrency and Parallel Processing    | eventmachine  |                | A gem that provides a concurrency framework for Ruby, allowing users to write concurrent applications using an event-driven model.                                                                      |
|                                        | parallel      |                | A gem that provides support for parallel processing in Ruby, allowing users to execute tasks concurrently.                                                                                              |
| Foreign Function Interface (FFI)       | ffi           |                | A gem that provides a foreign function interface for Ruby, allowing users to call functions written in other languages from Ruby.                                                                       |
| Shell Command Execution and Management | childprocess  |                | A gem that provides a cross-platform interface for spawning child processes and managing their input, output, and error streams.                                                                        |
|                                        | tty-box       |                | A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.                                                                    |
|                                        | tty-command   |                | A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.                                                                    |
|                                        | tty-config    |                | A component of the TTY toolkit that allows running shell commands with pretty logging and capturing stdout, stderr, and exit status.                                                                    |
|                                        | net-ssh       |                | A gem that provides a client for connecting