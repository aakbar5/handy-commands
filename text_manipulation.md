
<!-- TOC depthto:2 -->

- [Text manipulation](#text-manipulation)
- [sed](#sed)
    - [Intro](#intro)
    - [Flags](#flags)
    - [Useful commands](#useful-commands)
- [grep](#grep)

<!-- /TOC -->
- `-i` -- in place text editing
- `s` -- the substitute command
- `g` -- global (replace all occurrence)
- `original` -- original text
- `new` -- new text

## Useful commands

### Grep lines b/w two strings
```
sed -n '/test1/,/test2/p' file.txt
```
- In found lines search strings will be included.

- To exclude search strings
```
sed -n '/test1/,/test2/{/test1/b;/test2/b;p}' file.txt
```

### Replace a line in a file
```
sed -i '/original/c\new' file.txt
```

### Replace all occurrences of the string in a file
```
sed -i 's/original/new/g' file.txt
```

### Replace all occurrences of the string using env variable
```
sed -i "s/$original/new/g" file.txt
```
- NOTE: `"` is used instead of `'`.
- NOTE: `$original` means env variable is used.

`$original` can be used with [Bash parameter expansion](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion) trick which allows you to use default value if env variable is not set. For example
```
original="${ORIGINAL_ENV:-default_value}"
```
Let's assume `ORIGINAL_ENV` is not set, original variable will end up with having `default_value`.

### Replace all occurrences of the string having `/` (forward-slash)
```
sed -i "s|$original|new|g" file.txt
```

### Replace `,` with newline
```
echo "a,b" | sed 's/,/\n/g'
```

### Replace `/` with newline
```
echo "a/b" | sed 's/\//\n/g'
```

### Replace ` ` (space) with newline
```
echo "a b" | sed 's/\s/\n/g'
```

### Replace one or more space(s) with newline
```
echo "a b" | sed 's/\s\+/\n/g'
echo "a    b" | sed 's/\s\+/\n/g'
```

### Replace leading spaces or tabs
```
echo "    a b" | sed -e 's/^[ \t]*//'
echo "		a b" | sed -e 's/^[ \t]*//'
```

### Replace trailing spaces
```
echo "a b  " | sed -e 's/[[:space:]]*$//'
echo "a b       " | sed -e 's/[[:space:]]*$//'
```
- `:space` -- Use posix standard to specify space which will handle tabs too.

### Remove lines starting with string
```
sed -i '/^string/d' file.txt
```

### Remove tokens which are not starting with `-`
```
echo "--foo removeit -abc this_one_too" | sed -e 's/^/ /g' -e 's/ [^-][^ ]*//g' -e 's/^ *//g'
```

# grep

```bash
somecommand | grep -e foo -e bar -e baz
somecommand | grep -E 'foo|bar|baz'
somecommand | grep -i (case insensitive)
```
