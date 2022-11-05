<!-- TOC -->

- [Comments](#comments)
- [Echo](#echo)
- [Special variables](#special-variables)
- [Sleep](#sleep)
- [Variables](#variables)
- [Arrays](#arrays)
    - [Indexed](#indexed)
    - [Associative](#associative)
- [Arithmetics](#arithmetics)
    - [Simple](#simple)
    - [Using let](#using-let)
    - [Using backtick & expr](#using-backtick--expr)
- [If statement](#if-statement)
- [Switch statement](#switch-statement)
- [File validation](#file-validation)
- [Compound statement](#compound-statement)
- [Loops](#loops)
- [String manipulation](#string-manipulation)
    - [Length](#length)
    - [Slicing](#slicing)
    - [Concatenate](#concatenate)
    - [Changing case](#changing-case)
    - [Comparison](#comparison)
    - [Substitution](#substitution)
- [Using builtin -z and -n operator](#using-builtin--z-and--n-operator)
    - [For string](#for-string)
    - [For variables](#for-variables)
- [File path manipulation](#file-path-manipulation)
- [Variable substitution](#variable-substitution)
    - [${parameter:-word}](#parameter-word)
    - [${parameter-word}](#parameter-word)
- [Invoking script](#invoking-script)
- [Random number](#random-number)
- [Function](#function)
- [Global and local variables](#global-and-local-variables)
- [getopts](#getopts)

<!-- /TOC -->

# Comments

- Single line comments
```bash
# This is single line comment
```

- Multi lines comments
```bash
<<COMMENTS
Line #1 of Multilines comments
Line #2 of Multilines comments
COMMENTS
```


# Echo
- Simply print message
```bash
echo "Hello world!"
```
> `Hello world!`

- Print message without interpreting special sequence like `\t` `\n` etc
```bash
echo "Hello\tworld!"
```
> `Hello\tworld!`

- Print message as well as interpret special sequence like `\t` `\n` etc
```bash
echo -e "Hello\tworld!"
```
> `Hello	world!`


# Special variables
- `$0` - Name of the bash script
- `$1` - `$9` - Argument passed to the script
- `$#` - Number of passed arguments
- `$@` - List of the passed arguments
- `$$` - The process ID (pid) of the current script
- `$?` - Exit status of the most recently executed process
- `$!` - The process ID (pid) of the most recently executed process
- `$USER` - Retrieve username
- `$HOSTNAME` - Retrieve machine hostname
- `$SECONDS` - The number of seconds since the script was started
- `$RANDOM` - Returns a different random number each time is it referred to
- `$LINENO` - Current line number of the script


# Sleep

```bash
sleep 1  # Sleep for 1 second
sleep 1s # Sleep for 1 second
sleep 1m # Sleep for 1 minute
sleep 1h # Sleep for 1 hour
sleep 1d # Sleep for 1 day
```


# Variables

- Declare variable having integer value
```bash
ivar=10
```

- Declare variable having character value
```bash
cvar='a'
```

- Declare variable having string
```bash
svar="bash"
```

# Arrays
There are two types of arrays

## Indexed
- Integers can be used to refernece an element
- Zero based
- Syntax: `declare -a array`


- Declare an empty list
```bash
declare -a arr=()

echo "Size of array: ${#arr[@]}"
```
> `Size of array: 0`


- Declare an array with two elements
```bash
declare -a arr
arr[0]="entry0"
arr[1]="entry1"

echo "Size of array: ${#arr[@]}"
```
> `Size of array: 2`


- Declare an array with two elements
```bash
declare -a arr=("entry0" "entry1")

echo "Size of array: ${#arr[@]}"
```
> `Size of array: 2`


- Declare an array with two elements
```bash
arr=("entry0" "entry1")
```
> `Size of array: 2`


- Append a new element to the array
```bash
declare -a arr=("entry0" "entry1")
arr+=("entry2")

echo "Size of array: ${#arr[@]}"
```
> `Size of array: 3`


- Declare an array: zero index based array
```bash
iarray=( 10 20 30 40 50 )
```

- Array manipulations
```bash
iarray=( 10 20 30 40 50 )

echo "Count: ${#iarray[@]}" # Show number of elements in the array
echo "Elems: ${iarray[@]}" # Show all elements of the array
echo "Sub elems: ${iarray[@]:1:3}" # Show all elements of the array b/w the range
```
> `Count: 5`
> `Elems: 10 20 30 40 50`
> `Sub elems: 20 30 40`


- Iterate over the array
```bash
for ((i=0; i < ${#iarray[@]}; ++i)); do
  echo "Index # $i: ${iarray[i]}"
done
```
> `Index # 0: 10`
> `Index # 1: 20`
> `Index # 2: 30`
> `Index # 3: 40`
> `Index # 4: 50`


- Remove a specific element from the array
```bash
iarray=( 10 20 30 40 50 )

unset iarray[2] # Remove element # 2
echo ${iarray[@]} # Show all elements of the array
```

- Assign sub-elements to a new array
```bash
iarray=( 10 20 30 40 50 )

iarr=("${iarray[@]:1:3}") # Assign elements # 1,2,3 of iarray to iarr
echo ${iarr[@]} # Show all elements of the array
```

> `20 30 40`


## Associative
- Use arbitrary strings
- Syntax: `declare -A array`

``bash
declare -A aarr
aarr[animal]=lion
aarr[location]=African
```

# Add new key
```bash
aarr+=([group]=Pride)
```

```bash
echo "Count: ${#aarr[@]}" # Show number of elements in the array
echo "Elems: ${aarr[@]}" # Show all elements of the array
```
> `Count: 3`
> `Elems: lion african pride`


- Iterate keys of the array
```bash
for key in "${!aarr[@]}"; do
  echo "Key # $key";
done
```
> `Key # animal`
> `Key # location`
> `Key # group`


- Show all keys
```bash
echo "${!aarr[@]}"
```
> `animal location group`


-  Iterate values of the array
```bash
for val in "${aarr[@]}"; do
  echo "Value # $val";
done
```
> `Value # lion`
> `Value # african`
> `Value # pride`


- Show all values
```bash
echo "${assArray1[@]}"
```
> `lion african pride`


- Iterate array in key-value pair
```bash
for key in "${!aarr[@]}"; do
 echo "$key: ${aarr[$key]}";
done
```
> `animal: lion`
> `location: african`
> `group: pride`


# Arithmetics

## Simple

- Different ways to perform calculation
```bash
x=2
y=10
echo $(( x + y )) # spaces does not matter
echo $((x+y)) # spaces does not matter
echo $(($x+$y)) # having $ for variable does not matter
```
> `12` \
> `12` \
> `12`

- Perform additional calculation
```bash
x=2
((x += 3))
echo x
```
> `5`

The same can be done for:
- Addition (`+`, `+=` )
- Subtraction (`-`, `-=`)
- Division (`/`, `/=`)
- Multiplication (`*`, `*=`)
- Exponent (`**`)
- Modulus (`%`, `%=`)

## Using let

```bash
x=2
# x++ # Error: x++ command not found
let x++ # Using let will allow you to perform it
echo $x
```
> `3`

- Different ways to perform calculation
```bash
x=2
y=3

z=$((x+y))
echo $z

let z=$((x+y))
echo $z

z="$((x+y))"
echo $z

let z="$((x+y))"
echo $z
```
> `5` \
> `5` \
> `5` \
> `5`

## Using backtick & expr
```bash
a=2
b=3
c=`expr $a + $b`
echo $c
```
> `5`


# If statement

- Need a space after `if`, `[` and before `]`
- Only handful of operators can be used in `[ ]`
- You can use `<` inside `[ ]` however it must be escaped by `\` to stop it
  acting as redirection operator.
- Recommendations:
  - Use bash keywords style operator (`lt`, `le`, `ge`, `gt`, `eq`, `ne`) inside `[ ]`
  - or use `(( ))`

```bash
x=2
y=3

if [ $x == 2 ] && [ $y == 3 ]; then
  echo "Value of x is 2 and y is 3"
elif [ $x == 2 ]; then
  echo "Value of x is 2 and y is 3"
else
  echo "Values of x and y are not valid"
fi
```
> `Value of x is 2 and y is 3`

- Same if condition in different style
```bash
if [ $x \< $y ]; then
  echo "Value of x is less than y"
fi

if [ "$x" \< "$y" ]; then
  echo "Value of x is less than y"
fi

if [ $x -le $y ]; then
  echo "Value of x is less than y"
fi

if [ "$x" -le "$y" ]; then
  echo "Value of x is less than y"
fi

if (( $x < $y )); then
  echo "Value of x is less than y"
fi
```

- Using builtin operators for number comparisons
```bash
[[ 2 -eq 2 ]] && echo "Both numbers are equal"
[[ 2 -ne 5 ]] && echo "Both numbers are different"
[[ 2 -lt 6 ]] && echo "First number is less than second number"
[[ 2 -le 7 ]] && echo "First number is less than/equal second number"
[[ 9 -gt 8 ]] && echo "First number is greater than second number"
[[ 9 -ge 9 ]] && echo "First number is greater than/equal second number"
```
> `Both numbers are equal` \
> `Both numbers are different` \
> `First number is less than second number` \
> `First number is less than/equal second number` \
> `First number is greater than second number` \
> `First number is greater than/equal second number`


# Switch statement

```bash
x=7
case $x in
3)
    echo "x is having value 3"
    ;;
4 | 5 | 6 | 7)  # Specifying multiple options
    echo "x is having value b/w 4..7"
    ;;
8)
    echo "x is having value 8"
    ;;
*)
    echo "x is having unknown value"
    ;;
esac
```
> `x is having value b/w 4..7`


# File validation

```bash
# -e can be used to check existence of a file or directory
[[ -e "/path/to/folder" ]]      && echo "Directory is there" || echo "Directory is not there"
[[ -e "/path/to/folder/file" ]] && echo "File is there"      || echo "File is not there"

# -f can be only be used to check existence of a file
[[ -f "/path/to/folder" ]]      && echo "Directory is there" || echo "Directory is not there"
[[ -f "/path/to/folder/file" ]] && echo "File is there"      || echo "File is not there"
[[ -f "/path/to/folder/file" ]] || echo "File is not there"
[[ -f "/path/to/folder/file" ]] || { echo "File is not there"; exit 1; }

# Use -d for directory
[[ -d "/path/to/folder" ]]      && echo "Directory is there" || echo "Directory is not there"
[[ -d "/path/to/folder/file" ]] && echo "File is there"      || echo "File is not there"

# Validation of file path
[[ -r "/path/to/folder/file" ]] && echo "File is readable"   || echo "File is not readable"
[[ -w "/path/to/folder/file" ]] && echo "File is writable"   || echo "File is not writable"
[[ -x "/path/to/folder/file" ]] && echo "File is executable" || echo "File is not executable"
[[ -s "/path/to/folder/file" ]] && echo "File size >0 bytes" || echo "File size is of 0 bytes"
[[ -h "/path/to/folder/file" ]] && echo "File is symblink"   || echo "File is not symlink"

# File comparison
[[ "/path/to/f1" -nt "/path/to/f2" ]] && echo "f1 is more recent than f2"
[[ "/path/to/f1" -ot "/path/to/f2" ]] && echo "f2 is more recent than 1"
[[ "/path/to/f1" -ef "/path/to/f2" ]] && echo "f1 & f2 are the same files"
```


# Compound statement
- second part of the command after `;` will not be executed
```
[ -f test.txt ] && echo "first command"; cat "second command";
```

- use compound statement to execute multiple commands
```
[ -f sample.txt ] && { echo "first command"; cat "second command"; }
```


# Loops

- while loop (for ever)
```
while true; do echo "hello"; sleep 2; done
```

- while loop
```bash
a=1
while [[ $a -le 5 ]]; do
    echo "While loop idx # $a"
    let ++a
done
```
> `While loop idx # 1` \
> `While loop idx # 2` \
> `While loop idx # 3` \
> `While loop idx # 4` \
> `While loop idx # 5`

- For loop with defined range and default increment
```bash
for i in {1..10}; do
    echo "loop # $i with step @ 1"
done
```
> `loop # 1 with step @ 1` \
> `loop # 2 with step @ 1` \
> `loop # 3 with step @ 1` \
> `loop # 4 with step @ 1` \
> `loop # 5 with step @ 1` \
> `loop # 6 with step @ 1` \
> `loop # 7 with step @ 1` \
> `loop # 8 with step @ 1` \
> `loop # 9 with step @ 1` \
> `loop # 10 with step @ 1`


- For loop with defined range and custom increment
```bash
for i in {1..10..2}; do
    echo "loop # $i with step @ 2"
done
```
> `loop # 1 with step @ 2` \
> `loop # 3 with step @ 2` \
> `loop # 5 with step @ 2` \
> `loop # 7 with step @ 2` \
> `loop # 9 with step @ 2`


- For loop c-style
```bash
for ((i = 1 ; i <= 10 ; i++)); do
  echo "c-style loop # $i with step @ 1"
done
```
> `c-style loop # 1 with step @ 1` \
> `c-style loop # 2 with step @ 1` \
> `c-style loop # 3 with step @ 1` \
> `c-style loop # 4 with step @ 1` \
> `c-style loop # 5 with step @ 1` \
> `c-style loop # 6 with step @ 1` \
> `c-style loop # 7 with step @ 1` \
> `c-style loop # 8 with step @ 1` \
> `c-style loop # 9 with step @ 1` \
> `c-style loop # 10 with step @ 1`


- For loop in c-style
```bash
for ((i = 1 ; i <= 10 ; i += 2)); do
  echo "c-style loop # $i with step @ 2"
done
```
> `c-style loop # 1 with step @ 2` \
> `c-style loop # 3 with step @ 2` \
> `c-style loop # 5 with step @ 2` \
> `c-style loop # 7 with step @ 2` \
> `c-style loop # 9 with step @ 2`


- For loop with seq expression
```bash
for i in `seq 1 5`; do
  echo "loop with seq expr # $i"
done
```
> `loop with seq expr # 1` \
> `loop with seq expr # 2` \
> `loop with seq expr # 3` \
> `loop with seq expr # 4` \
> `loop with seq expr # 5`


- `for` loop to iterate over word list
```bash
words="word1 word2 word3"
for word in $words; do
  echo "$word"
done
```
> `word1` \
> `word2` \
> `word3`

```bash
for word in word1 word2 word3; do echo "$word"; done
```
> `word1` \
> `word2` \
> `word3`

- `for` loop to iterate over word list
```bash
words="
word1
word2  word3
word4           word5
"
for word in $words; do
  echo "$word"
done
```
> `word1` \
> `word2` \
> `word3` \
> `word4` \
> `word5`


# String manipulation
## Length

- Length of a string
```bash
str="string"
echo "${#str}"
```
> `6`


## Slicing
- String can be sliced down from front as well from backward. Format will be `${string:position:length}`.

- Extract string starting from the specified character position to the end of the string.
```bash
str="1234567890"
echo "${str:6}"
```
> `7890`

- Extract string starting from the specified character position to the start of the string.
```bash
str="1234567890"
echo "${str:(-6)}"
```
> `567890`

- Extract string starting from the specified character position to the specified number of characters.
```bash
str="1234567890"
echo "${str:6:4}"
```
> `7890`

- Extract string starting from the specified character position to the specified number of characters.
```
str="1234567890"
echo "${str:(-6):3}"
```
> `567`

- Extract specified number characters starting from the string.
```bash
str="1234567890"
echo "${str::3}"
```
> `123`

- Extract string starting from the specified character from the backwards to the start of the string.
```bash
str="1234567890"
echo "${str::-3}"
```
> `1234567`


## Concatenate
```bash
str="string1"
str+=" string2"
str="${str} string3"
echo "${str}"
```
> `string1 string2 string3`


## Changing case

- Convert whole string into uppercase
```bash
str=bash
echo "${str^^}"
```
> `BASH`

- Convert first character to uppercase
```bash
str=bash
echo "${str^}"
```
> `Bash`

- Convert whole string into lowercase
```bash
str=BASH
echo "${str,,}"
```
> `bash`


```bash
str=BASH
echo "${str,}"
```
> `bASH`

- Reverse the each character of the string
```bash
str=baSH
echo "${str~~}"
```
> `BAsh`

- Reverse the first character of the string
```bash
str=baSH
echo "${str~}"
```
> `BaSH`


## Comparison
- Bash if condition with `==`, `!=` can be used for case sensitive comparison.

```bash
str="string"
if [ "${str}" == "string" ]; then
    echo "Both strings are equal"
fi

if [ "${str}" != "String" ]; then
    echo "Both strings are different"
fi
```
> `Both strings are equal`
> `Both strings are different`

- String comparison using single line if statements
```bash
[[ "bash" == "bash" ]] && echo "String are equal" || echo "String are not equal"
[[ "bash" != "bash" ]] && echo "String not equal" || echo "String are equal"
```
> `String are not equal` \
> `String are equal`

See [Compound statement](#compound-statement)

```bash
# You can skip any part
[[ "bash" == "bash" ]] && echo "String are equal"
[[ "bash" != "bash" ]] || echo "String are equal"
```
> `String are not equal` \
> `String are equal`


## Substitution
For pattern, look for https://wiki.bash-hackers.org/syntax/pattern

- `${PARAMETER/PATTERN/STRING}` - replace first occurrence
- `${PARAMETER//PATTERN/STRING}` - replace all occurrences
- `${PARAMETER/PATTERN}` - remove first occurrence of pattern
- `${PARAMETER//PATTERN}` - remove all occurrences of pattern

```bash
str="Bash, hello world! Bash, hello world!"
echo ${str//l/L} # Replace l with L
echo ${str//world} # Remove world
```
> `Bash, heLLo worLd! Bash, heLLo worLd!` \
> `Bash, hello ! Bash, hello !`

- If $substring matches front end of $string, substitute $replacement for $substring.
```bash
str="test hello world test"
echo ${str/#test/L}
```
> `L hello world test`

- If $substring matches back end of $string, substitute $replacement for $substring.
```bash
str="test hello world test"
echo ${str/%test/L}
```
> `test hello world L`

- Remove prefix
```bash
str="test hello world test"
echo ${str/#test}
```
> `hello world test`

- Remove suffix
```bash
str="test hello world test"
echo ${str/%test}
```
> `test hello world`

- Remove long suffix
```bash
${VAR%%suffix}
```

- Remove long prefix
```bash
${VAR##prefix}
```


# Using builtin `-z` and `-n` operator

## For string

```bash
str="bash"
[[ -z $str ]] && echo "string is empty" || echo "string is not empty"
```
> `string is not empty`

```bash
str=""
[[ -z $str ]] && echo "string is empty" || echo "string is not empty"
```
> `string is empty`

```bash
str="bash"
[[ -n $str ]] && echo "string is not empty" || echo "string is empty"
```
> `string is not empty`

```bash
str="bash"
[[ -n $str ]] && echo "string is not empty" || echo "string is empty"
```
> `string is empty`


## For variables
```bash
a=0
if [ -n "$a" ]; then
  echo "a is set (Using -z)"
else
  echo "a is not set (Using -z)"
fi

# b=0
if [ -z "$b" ]; then
  echo "b is set (Using -z)"
else
  echo "b is not set (Using -z)"
fi
```
> `a is set (Using -z)` \
> `b is set (Using -z)`

```bash
x=0
if [ -n "$x" ]; then
  echo "x is set (Using -n)"
else
  echo "x is not set (Using -n)"
fi

# y=0
if [ -n "$y" ]; then
  echo "y is set (Using -n)"
else
  echo "y is not set (Using -n)"
fi
```
> `x is set (Using -n)` \
> `y is not set (Using -n)`


# File path manipulation

```bash
path="/path/to/file.ext"
echo "                  ${path}"
echo "Path + File Name: ${path%.ext}"
echo "Path:             ${path%/*.*}"
echo "File Name + ext:  ${path##*/}"
echo "Extension:        ${path##*.}"
```
> `                  /path/to/file.ext` \
> `Path + File Name: /path/to/file` \
> `Path:             /path/to` \
> `File Name + ext:  file.ext` \
> `Extension:        ext`


# Variable substitution
https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion
https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_06_02

## `${parameter:-word}`
```bash
var=10
echo ${var:-20} # Use 20; if var is not set or null
```
> 10

```bash
echo ${var:-20} # Use 20 as var is null
```
> 20

```bash
var=
echo ${var:-20} # Use 20 as var is null
```
> 20

## `${parameter-word}`
```bash
echo ${var-20} # Use 20 as var is not declared
```
> 20

```bash
var=
echo ${var-20} # 20 will not be used as var is declared
```
>

NOTE: `${var:-20}` vs `${var:20}` - Funtionality wise both are same. `:` is forcing to set the value by `20` even if variable is declared but no value is being assigned.

# Invoking script
- Invoking another script (as a separate process) from within script
- As new script is a separate process so envrionment modifications
  done for the current script won't be visible to the new script
```bash
#!/bin/bash
./script.sh
# - or -
/path/to/script.sh
# - or -
/bin/bash script.sh
```

- Invoking another script with in the process
- Now envrionment modification will be visisble to the new script
```bash
#!/bin/bash
. ./script.sh
# - or -
. /path/to/script.sh
# - or -
source script.sh
```


# Random number
- Generate 32 characters having a-z, A-Z, 0-9
```bash
cat /dev/urandom | tr -dc 'a-zA-Z0-9' | head -c 32
```
- Generate a number between 0-999
```bash
cat /dev/urandom | tr -dc '0-9' | fold -w3 | head -n1
```
Use `-w3` of fold command to control of number of digits in a number.

- Generate numbers between ranges
```bash
shuf -i 100-250 -n 10
shuf -i 100-250 -n 10 --random-source /dev/urandom
```
Generate numbers b/w 100-250.


# Function
[Different style of functions](https://gist.github.com/aakbar5/9ac8073f30965c6d138b041dfc4d69a8)


# Global and local variables
```bash
var=10
function test() {
  var=20 # Global var has been modified
  echo "var in test = $var"
}

test
echo "var in main = $var"
```

```
var in test = 20
var in main = 20
```

Output will be like following:
```bash
var=10
function test() {
  local var=20 # Global var will retain its value
               # A var has been created which is local to this function
  echo "var in test = $var"
}

test
echo "var in main = $var"
```

Output will be like following:
```
var in test = 20
var in main = 10
```

# getopts
[getopts usage](https://gist.github.com/aakbar5/e2cf511c5844929afc849cf0fda46296)

- If `:` is not found at the start of the argument list, `getopts` does not perform any error checking. Otherwise error will be reported on having unkown option or paramter.
- `getopts` does not handle long options (`--long`). `getopt` can do the job however it is not built-in functionality of the Bourne shell.
