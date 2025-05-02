
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
## Bash Arrays

### Defining arrays
```bash
Fruits=('Apple' 'Banana' 'Orange')

Fruits[0]="Apple"
Fruits[1]="Banana"
Fruits[2]="Orange"

ARRAY1=(foo{1..2}) # => foo1 foo2
ARRAY2=({A..D})    # => A B C D

# Merge => foo1 foo2 A B C D
ARRAY3=(${ARRAY1[@]} ${ARRAY2[@]})

# declare construct
declare -a Numbers=(1 2 3)
Numbers+=(4 5) # Append => 1 2 3 4 5
```

### Indexing

| -                  | -             |
| ------------------ | ------------- |
| `${Fruits[0]}`     | First element |
| `${Fruits[-1]}`    | Last element  |
| `${Fruits[*]}`     | All elements  |
| `${Fruits[@]}`     | All elements  |
| `${#Fruits[@]}`    | Number of all |
| `${#Fruits}`       | Length of 1st |
| `${#Fruits[3]}`    | Length of nth |
| `${Fruits[@]:3:2}` | Range         |
| `${!Fruits[@]}`    | Keys of all   |

### Iteration

```bash
Fruits=('Apple' 'Banana' 'Orange')

for e in "${Fruits[@]}"; do
    echo $e
done
```

#### With index

```bash
for i in "${!Fruits[@]}"; do
  printf "%s\t%s\n" "$i" "${Fruits[$i]}"
done

```
### Operations

```bash
Fruits=("${Fruits[@]}" "Watermelon")     # Push
Fruits+=('Watermelon')                   # Also Push
Fruits=( ${Fruits[@]/Ap*/} )             # Remove by regex match
unset Fruits[2]                          # Remove one item
Fruits=("${Fruits[@]}")                  # Duplicate
Fruits=("${Fruits[@]}" "${Veggies[@]}")  # Concatenate
lines=(`cat "logfile"`)                  # Read from file
```

```bash
# Generate a log file with 10 entries, each including timestamp and INFO level
for i in {1..10}; do
    echo "$(date '+%Y-%m-%d %H:%M:%S') [INFO] Log Entry $i - Operation completed" >> mylog.log
done

# Read and print the log file line by line
while IFS= read -r line; do
    echo "$line"
done < mylog.log
```
- `IFS=` ensures that **leading/trailing spaces** are preserved.
- `read -r` prevents **interpretation of backslashes.**

### Arrays as arguments

```bash
function extract()
{
    local -n myarray=$1 # Reference the variable passed as an argument
    local idx=$2
    echo "${myarray[$idx]}"
}
Fruits=('Apple' 'Banana' 'Orange')
extract Fruits 2     # => Orangle
```

## Bash Dictionaries

### Defining

```bash
declare -A sounds
```

```bash
sounds[dog]="bark"
sounds[cow]="moo"
sounds[bird]="tweet"
sounds[wolf]="howl"
```

```bash
declare -A sounds=( [dog]="bark" [cow]="moo" [bird]="tweet" [wolf]="howl" )
```

### Working with dictionaries

```bash
echo ${sounds[dog]} # Dog's sound
echo ${sounds[@]}   # All values
echo ${!sounds[@]}  # All keys
echo ${#sounds[@]}  # Number of elements
unset sounds[dog]   # Delete dog
```

### Iteration

```bash
for val in "${sounds[@]}"; do
    echo $val
done
```

---

```bash
for key in "${!sounds[@]}"; do
    echo $key
done
```

## Bash Conditionals

### Integer conditions

| Condition           | Description                                 |
| ------------------- | ------------------------------------------- |
| `[[ NUM -eq NUM ]]` | <yel>Eq</yel>ual                            |
| `[[ NUM -ne NUM ]]` | <yel>N</yel>ot <yel>e</yel>qual             |
| `[[ NUM -lt NUM ]]` | <yel>L</yel>ess <yel>t</yel>han             |
| `[[ NUM -le NUM ]]` | <yel>L</yel>ess than or <yel>e</yel>qual    |
| `[[ NUM -gt NUM ]]` | <yel>G</yel>reater <yel>t</yel>han          |
| `[[ NUM -ge NUM ]]` | <yel>G</yel>reater than or <yel>e</yel>qual |
| `(( NUM < NUM ))`   | Less than                                   |
| `(( NUM <= NUM ))`  | Less than or equal                          |
| `(( NUM > NUM ))`   | Greater than                                |
| `(( NUM >= NUM ))`  | Greater than or equal                       |

### String conditions

| Condition          | Description                 |
| ------------------ | --------------------------- |
| `[[ -z STR ]]`     | Empty string                |
| `[[ -n STR ]]`     | <yel>N</yel>ot empty string |
| `[[ STR == STR ]]` | Equal                       |
| `[[ STR = STR ]]`  | Equal (Same above)          |
| `[[ STR < STR ]]`  | Less than _(ASCII)_         |
| `[[ STR > STR ]]`  | Greater than _(ASCII)_      |
| `[[ STR != STR ]]` | Not Equal                   |
| `[[ STR =~ STR ]]` | Regexp                      |

### Example

#### String

```bash
if [[ -z "$string" ]]; then
    echo "String is empty"
elif [[ -n "$string" ]]; then
    echo "String is not empty"
else
    echo "This never happens"
fi
```
#### Combinations

```bash
if [[ X && Y ]]; then
    ...
fi
```

#### Equal

```bash
if [[ "$A" == "$B" ]]; then
    ...
fi
```

#### Regex

```bash
if [[ '1. abc' =~ ([a-z]+) ]]; then
    echo ${BASH_REMATCH[1]}
fi
```

#### Smaller

```bash
if (( $a < $b )); then
   echo "$a is smaller than $b"
fi
```

#### Exists

```bash
if [[ -e "file.txt" ]]; then
    echo "file exists"
fi
```

### File conditions {.row-span-2}

| Condition         | Description                            |
| ----------------- | -------------------------------------- |
| `[[ -e FILE ]]`   | <yel>E</yel>xists                      |
| `[[ -d FILE ]]`   | <yel>D</yel>irectory                   |
| `[[ -f FILE ]]`   | <yel>F</yel>ile                        |
| `[[ -h FILE ]]`   | Symlink                                |
| `[[ -s FILE ]]`   | Size is > 0 bytes                      |
| `[[ -r FILE ]]`   | <yel>R</yel>eadable                    |
| `[[ -w FILE ]]`   | <yel>W</yel>ritable                    |
| `[[ -x FILE ]]`   | Executable                             |
| `[[ f1 -nt f2 ]]` | f1 <yel>n</yel>ewer <yel>t</yel>han f2 |
| `[[ f1 -ot f2 ]]` | f2 <yel>o</yel>lder <yel>t</yel>han f1 |
| `[[ f1 -ef f2 ]]` | Same files                             |

### More conditions

| Condition            | Description          |
| -------------------- | -------------------- | ----- | --- |
| `[[ -o noclobber ]]` | If OPTION is enabled |
| `[[ ! EXPR ]]`       | Not                  |
| `[[ X && Y ]]`       | And                  |
| `[[ X                |                      | Y ]]` | Or  |

### logical and, or

```bash
if [ "$1" = 'y' -a $2 -gt 0 ]; then
    echo "yes"
fi

if [ "$1" = 'n' -o $2 -lt 0 ]; then
    echo "no"
fi
```
