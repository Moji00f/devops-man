
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
| `$!`        | Process ID of the most recently executed background job |

✅ `$@` **behaves like an array in loops,** meaning each argument is treated as a separate element.<br/>
✅ `$*` **behaves like a single quoted string in loops,** meaning all arguments are joined into one string.

#### Example to illustrate the difference in a loop
```bash
#!/bin/bash
for arg in "$@"; do
    echo "$arg"
done

for arg in "$*"; do
    echo "$arg"
done
```
#### Input
```bash
./script.sh arg1 arg2 arg3
```

#### Output
```bash
Using "$@":
arg1
arg2
arg3

Using "$*":
arg1 arg2 arg3  # Treated as a single string
```

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

### Syntax 

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

#### Substrings

| Expression      | Description                     | Example                        | Output            |
| -------------- | ------------------------------ | ------------------------------ | ------------------- |
| `${FOO:0:3}`   | Substring _(position, length)_ | `FOO="abcdef"`<br>`echo ${FOO:0:3}` | `abc`          |
| `${FOO:(-3):3}`| Substring from the right       | `FOO="abcdef"`<br>`echo ${FOO:(-3):3}` | `def`       |

#### Length

| Expression  | Description       | Example                        | Output      |
| ----------- | ---------------- | ------------------------------  | ----------- |
| `${#FOO}`  | Length of `$FOO`  | `FOO="hello"`<br>`echo ${#FOO}` | `5`         |

#### Default values

| Expression         | Description                              | Example                                                 | Output                             |
| ------------------ | ----------------------------------------- | ------------------------------------------------------- | ---------------------------------- |
| `${FOO:-val}`      | `$FOO`, or `val` if unset                 | `unset FOO`<br>`echo ${FOO:-"default"}`                 | `default`                          |
| `${FOO:=val}`      | Set `$FOO` to `val` if unset              | `unset FOO`<br>`echo ${FOO:="default"}`                 | `default`                          |
| `${FOO:+val}`      | `val` if `$FOO` is set                    | `FOO="hello"`<br>`echo ${FOO:+world}`                   | `world`                            |
| `${FOO:?message}`  | Show message and exit if `$FOO` is unset  | `unset FOO`<br>`echo ${FOO:?"Error: FOO is not set"}`   | `bash: FOO: Error: FOO is not set` |

### Substitution

```bash
echo ${food:-Cake}  #=> $food or "Cake"
```

```bash
STR="/path/to/foo.go"
echo ${STR%.go}    # /path/to/foo
echo ${STR%.go}.o  # /path/to/foo.o
echo ${STR%/*}     # /path/to
echo ${STR%%/*}     #(empty string)

echo ${STR##*.}     # go (extension)
echo ${STR##*/}     # foo.go (basepath)

echo ${STR#*/}      # path/to/foo.go
echo ${STR##*/}     # foo.go

echo ${STR/foo/bar} # /path/to/bar.go

```

### basepath & dirpath

```bash
SRC="/path/to/foo.go"
```

```bash
BASEPATH=${SRC##*/}
echo $BASEPATH  # => "foo.go"

DIRPATH=${SRC%$BASEPATH}
echo $DIRPATH   # => "/path/to/"
```

### Slicing
```bash
name="Mojtaba"
echo ${name}           # => Mojtaba  # Prints the full variable content
echo ${name:0:2}       # => Mo       # Extracts the first 2 characters (starting at index 0)
echo ${name::2}        # => Mo       # Same as above, using shorthand syntax
echo ${name::-1}       # => Mojtab   # Removes the last character
echo ${name:(-1)}      # => a        # Extracts the last character
echo ${name:(-2)}      # => ba       # Extracts the last 2 characters
echo ${name:(-2):2}    # => ba       # Extracts 2 characters starting from the second-to-last

length=2
echo ${name:0:length}  # => Mo       # Extracts the first 'length' characters
```

### Transform
```bash
STR="MOJTABA AHMADI"
echo ${STR,}   # => mOJTABA AHMADI  # Converts the first letter to lowercase
echo ${STR,,}  # => mojtaba ahmadi  # Converts all letters to lowercase

STR="mojtaba ahmadi"
echo ${STR^}   # => Mojtaba ahmadi  # Capitalizes the first letter
echo ${STR^^}  # => MOJTABA AHMADI  # Converts all letters to uppercase

ARR=(mojtaba Ahmadi)
echo "${ARR[@],}" # => mojtaba ahmadi  # Converts the first letter of each element to lowercase
echo "${ARR[@]^}" # => Mojtaba Ahmadi  # Capitalizes the first letter of each element
```
