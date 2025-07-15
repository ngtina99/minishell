# minishell

A C-based Unix shell built from scratch - aka the magic behind your terminal.

✨ This project was a collaboration with [**Yulia**](https://github.com/leanor13). From late-night debugging sessions to complex C coding: we learned to turn frustration into progress and transforming our effort into a shell we’re genuinely proud of. ✨


## 🛠️ Usage
1️⃣ **Clone the repository**

```bash
git clone https://github.com/yourusername/webserv.git minishell
cd minishell
```

2️⃣ Build the shell

```bash
make
```

3️⃣ Run the shell

```bash
./minishell
```

## 🛕 Structure
minishell-main/
├── Makefile
├── includes/
│   ├── executor.h          --> executor modules
│   ├── lexer.h             --> lexer & parsing modules
│   ├── minishell.h         --> general project definitions
│   └── libft/
│       ├── Makefile        --> libft Makefile
│       └── *.c             --> custom library functions (libft)
├── srcs/
│   ├── built_ins/
│   │   ├── ft_cd.c        --> cd command
│   │   ├── ft_echo.c      --> echo command
│   │   ├── ft_env.c       --> env command
│   │   ├── ft_exit.c      --> exit command
│   │   ├── ft_export.c    --> export command
│   │   ├── ft_pwd.c       --> pwd command
│   │   └── ft_unset.c     --> unset command
│   ├── executor/
│   │   ├── executor_main.c  --> main executor loop
│   │   ├── pipex.c         --> pipe handling
│   │   ├── here_doc.c      --> heredoc handling
│   │   ├── open_fds.c      --> file descriptor setup
│   │   └── utils_*.c       --> free, close helpers, checkers
│   ├── main_flow/
│   │   ├── main_minishell.c --> main shell loop
│   │   ├── init.c          --> initialize structures, env
│   │   ├── signals.c       --> signal management
│   │   └── main_util*.c    --> main utilities, cleanup
│   └── parsing/
│       ├── lexer.c            --> tokenizes input
│       ├── lexer_handle_quote.c --> handles quotes
│       ├── parser.c           --> parses token stream
│       ├── parser_handle_files.c --> parses redirection
│       └── parser_ifutils*.c  --> conditional parsing helpers


User Input
↓
[ parsing/ ]
→ lexer → parser → handle quotes, files, redirections
↓
[ executor/ ]
→ builds command list → manages pipes & heredoc → runs built-in or external
↓
[ built_ins/ ]
→ handles built-in commands (if matched)
↓
[ main_flow/ ]
→ manages shell loop, signal handling, environment init/cleanup

## Features

✅ Command parsing and execution  
✅ Support for pipes (`|`)  
✅ Input/output redirection (`>`, `<`)  
✅ Built-in commands (`cd`, `echo`, `pwd`, `exit`, etc.)  
✅ Environment variable handling  
✅ Custom signal handling (Ctrl+C, Ctrl+\)

⚡ Example commands to show
In your recording, you might run:


./minishell
echo "Hello World"
ls -l | grep minishell
exit
📝 Usage
Example commands:

bash
Copy
Edit
ls -l | grep minishell > out.txt
echo "Hello $USER"
cat << EOF
This is a heredoc
EOF

Usage
Once running, you can:

Execute commands like ls, grep, cat, etc.

Use pipes, e.g., ls | grep minishell

Redirect input/output, e.g., ls > out.txt

Use built-in commands like cd, exit

## ✨ Mandatory Features

✅ Display a prompt when waiting for commands  
✅ Working command history  
✅ Search and execute the right executable (from `$PATH` or given path)  
✅ Use **at most one global variable** (only for received signals)  
✅ Correct handling of:
- Unclosed quotes (not interpreted)
- Single quotes `'` → no meta-character interpretation
- Double quotes `"` → no meta-character interpretation except `$`

✅ Redirections:
- `<` input
- `>` output
- `<<` heredoc (with delimiter, no history update)
- `>>` append output

✅ Pipes: connect commands using `|`

✅ Environment variables:
- `$VAR`
- `$?` → exit status of last foreground pipeline

✅ Signals:
- `ctrl-C` → display new prompt
- `ctrl-D` → exit shell
- `ctrl-\` → do nothing

✅ Built-in commands:
- `echo` (`-n` option)
- `cd` (relative or absolute path)
- `pwd`
- `export`
- `unset`
- `env`
- `exit`

---

## 💥 Bonus Features

✅ Logical operators:
- `&&`  
- `||`

  ✅ Basic simple commands
/bin/ls

/bin/pwd

/usr/bin/whoami

/bin/echo hello

✅ With arguments
/bin/ls -l

/bin/echo hello world

✅ echo command
echo hello world

echo -n no newline

echo "$USER"

echo '$USER'

✅ exit command
exit

exit 42

exit 999

✅ Return value testing
/bin/ls → echo $?

/bin/ls nonexistingfile → echo $?

/bin/false → echo $?

✅ Signals
Start cat and press Ctrl+C, Ctrl+D, Ctrl+\

Just press Ctrl+C, Ctrl+D in an empty prompt

✅ Double quotes
echo "hello world"

echo "cat lol.c | cat > lol.c"

✅ Single quotes
echo '$USER'

echo 'this is a test'

✅ env / export / unset
env

export TESTVAR=hello

env (check it’s there)

unset TESTVAR

env (check it’s gone)

✅ cd
cd /tmp → pwd

cd .. → pwd

cd . → pwd

✅ pwd
pwd

✅ Relative path
./minishell (if you’re in the folder)

../someotherfolder/command

✅ Environment path
ls

wc

awk

unset PATH → try ls (should fail)

export PATH="/bin:/usr/bin" → try again

✅ Redirection
echo hello > file.txt

cat < file.txt

echo world >> file.txt

cat << EOF (then type something, then EOF)

✅ Pipes
ls -l | grep minishell

cat file.txt | grep hello | wc -l

ls nonexist | grep nothing | wc -l (check errors)

✅ Bonus (if implemented)
ls && echo ok

false || echo failover

(echo hello && echo world)

⚡ Special stress tests (for “go crazy”)
dsfsgsdgsdgsd (random command)

cat | cat | ls

very long command: echo + paste a huge line

# Minishell Evaluation Checklist

## ✅ Mandatory Part

### Compile
- [ ] Uses `-Wall -Wextra -Werror`
- [ ] Compiles without errors
- [ ] Makefile does not re-link

### Simple Commands & Global Variables
- [ ] Execute absolute path command (e.g., /bin/ls)
- [ ] Only one global variable (signal number)
- [ ] Handles empty command / only spaces or tabs

### Arguments
- [ ] Commands with arguments (no quotes)

### echo
- [ ] Works with/without `-n` option
- [ ] Handles multiple arguments

### exit
- [ ] Works with/without arguments
- [ ] Relaunchable

### Return Value
- [ ] `$?` gives correct exit status
- [ ] Handles wrong commands gracefully
- [ ] Supports expressions like `expr $? + $?`

### Signals
- [ ] `Ctrl-C` → new prompt, clean buffer
- [ ] `Ctrl-\` → does nothing
- [ ] `Ctrl-D` → exit shell

### Double Quotes
- [ ] Handles quoted arguments (including whitespace)
- [ ] Does not interpret anything except `$`

### Single Quotes
- [ ] Prints literal content (e.g., `echo '$USER'` → `$USER`)

### env
- [ ] Lists environment variables

### export
- [ ] Adds/replaces variables
- [ ] Confirmed with `env`

### unset
- [ ] Removes variables
- [ ] Confirmed with `env`

### cd
- [ ] Changes directory, handles `.` and `..`

### pwd
- [ ] Prints current directory

### Relative Path
- [ ] Runs commands with relative paths

### Environment Path
- [ ] Runs commands without path (using $PATH)
- [ ] Behaves correctly if $PATH is unset/modified

### Redirection
- [ ] Supports `<`, `>`, `>>`, `<<`
- [ ] Handles multiple attempts safely

### Pipes
- [ ] Handles command pipelines
- [ ] Works with errors + mixed redirections

### Go Crazy & History
- [ ] Up/Down arrow history
- [ ] Ignores invalid commands safely
- [ ] Handles long commands
- [ ] Cleans buffer on `Ctrl-C`

### Environment Variables
- [ ] Correctly interpolates `$VARIABLE`
- [ ] Double quotes interpolate variables

---

## 🌟 Bonus

- [ ] `&&`, `||`

## 📋 Memory checks

All features were tested under Valgrind:
- `valgrind --leak-check=full ./minishell`
- No leaks except those allowed from `readline()`

## 🤝 Credits
I couldn’t have asked for a better partner for my first big team project. Working with [**Yulia**](https://github.com/leanor13) on Minishell was both impactful and memorable.

## 💼 Connect
If you have any questions or suggestions, feel free to connect:
🔗 [LinkedIn: Valentina Nguyen](https://www.linkedin.com/in/valentina-nguyen-tina/) 🙋‍♀️

## 📜 License
This project is licensed under the MIT License. See LICENSE for details.

## 🖥️ System Compatibility
This project was developed and tested on Linux (Ubuntu).
