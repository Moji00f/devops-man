
## Getting Started

### hello.sh

```bash
#!/bin/bash

VAR="world"
echo "Hello $VAR!" # => Hello world!
```

Execute the script

```shell script
$ bash hello.sh
```

### Variables

```bash
NAME="Mojtaba"

echo ${NAME}    # => Mojtaba (Variables)
echo $NAME      # => Mojtaba (Variables)
echo "$NAME"    # => Mojtaba (Variables)
echo '$NAME'    # => $NAME (Exact string)
echo "${NAME}!" # => Mojtaba! (Variables)

NAME = "John"   # => Error (about space)
```

### Comments

```bash
# This is an inline Bash comment.
```

```bash
: '
This is a
very neat comment
in bash
'
```

Multi-line comments use `:'` to open and `'` to close


| Expression  | Description                           |
| ----------- | ------------------------------------- |
| `$1` … `$9` | Parameter 1 ... 9                     |
| `$0`        | Name of the script itself             |
| `$1`        | First argument                        |
| `${10}`     | Positional parameter 10               |
| `$#`        | Number of arguments                   |
| `$$`        | Process id of the shell               |
| `$*`        | All arguments                         |
| `$@`        | All arguments, starting from first    |
| `$-`        | Current options                       |
| `$_`        | Last argument of the previous command |


### Functions

```bash
get_name() {
    echo "John"
}

echo "You are $(get_name)"
```


### Conditionals {#conditionals-example}

```bash
if [[ -z "$string" ]]; then
    echo "String is empty"
elif [[ -n "$string" ]]; then
    echo "String is not empty"
fi
```


### Brace expansion

```bash
echo {A,B}.js
```

---

| Expression | Description         |
| ---------- | ------------------- |
| `{A,B}`    | Same as `A B`       |
| `{A,B}.js` | Same as `A.js B.js` |
| `{1..5}`   | Same as `1 2 3 4 5` |


### Shell execution

```bash
# => I'm in /path/of/current
echo "I'm in $(PWD)"

# Same as:
echo "I'm in `pwd`"
```


## Bash Parameter expansions


| Code               | Description         | Example                                                      | Output                     |
| ------------------ | ------------------- | ------------------------------------------------------------ | -------------------------- |
| `${FOO%suffix}`    | Remove suffix       | `filename="doc.txt"`<br>`echo ${filename%.txt}`              | `doc`                      |
| `${FOO#prefix}`    | Remove prefix       | `url="https://example.com"`<br>`echo ${url#https://}`        | `example.com`              |
| `${FOO%%suffix}`   | Remove long suffix  | `filename="backup.tar.gz"`<br>`echo ${filename%%.gz}`        | `backup.tar`               |
| `${FOO##prefix}`   | Remove long prefix  | `path="/home/user/docs"`<br>`echo ${path##*/}`               | `docs`                     |
| `${FOO/from/to}`   | Replace first match | `text="hello world world"`<br>`echo ${text/world/universe}`  | `hello universe world`     |
| `${FOO//from/to}`  | Replace all         | `text="hello world world"`<br>`echo ${text//world/universe}` | `hello universe universe`  |
| `${FOO/%from/to}`  | Replace suffix      | `filename="report.doc"`<br>`echo ${filename/%doc/pdf}`       | `report.pdf`               |
| `${FOO/#from/to}`  | Replace prefix      | `url="http://example.com"`<br>`echo ${url/#http/https}`      | `https://example.com`      |
