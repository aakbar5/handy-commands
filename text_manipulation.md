
<!-- TOC depthto:1 -->

- [sed](#sed)
- [grep](#grep)

<!-- /TOC -->

# sed

### Common flags
- `-i` -- in place text editing
- `s` -- the substitute command
- `g` -- global (replace all occurrence)
- `original` -- original text
- `new` -- new text

### Grep lines b/w two strings
```bash
sed -n '/test1/,/test2/p' file.txt
```
- In found lines search strings will be included.

- To exclude search strings
```bash
sed -n '/test1/,/test2/{/test1/b;/test2/b;p}' file.txt
```

### Replace a line in a file
```bash
sed -i '/original/c\new' file.txt
```

### Replace all occurrences of the string in a file
```bash
sed -i 's/original/new/g' file.txt
```

### Replace all occurrences of the string using env variable
```
sed -i "s/$original/new/g" file.txt
```
- NOTE: `"` is used instead of `'`.
- NOTE: `$original` means env variable is used.

`$original` can be used with [Bash parameter expansion](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion) trick which allows you to use default value if env variable is not set. For example
```bash
original="${ORIGINAL_ENV:-default_value}"
```

Let's assume `ORIGINAL_ENV` is not set, original variable will end up with having `default_value`.

### Replace all occurrences of the string having `/` (forward-slash)
```bash
sed -i "s|$original|new|g" file.txt
```

### Replace `,` with newline
```bash
echo "a,b" | sed 's/,/\n/g'
```

### Replace `/` with newline
```bash
echo "a/b" | sed 's/\//\n/g'
```

### Replace ` ` (space) with newline
```bash
echo "a b" | sed 's/\s/\n/g'
```

### Replace one or more space(s) with newline
```bash
echo "a b" | sed 's/\s\+/\n/g'
echo "a    b" | sed 's/\s\+/\n/g'
```

### Replace leading spaces or tabs
```bash
echo "    a b" | sed -e 's/^[ \t]*//'
echo "		a b" | sed -e 's/^[ \t]*//'
```

### Replace trailing spaces
```bash
echo "a b  " | sed -e 's/[[:space:]]*$//'
echo "a b       " | sed -e 's/[[:space:]]*$//'
```

- `:space` -- Use posix standard to specify space which will handle tabs too.

### Remove lines starting with string
```bash
sed -i '/^string/d' file.txt
```

### Remove tokens which are not starting with `-`
```bash
echo "--foo removeit -abc this_one_too" | sed -e 's/^/ /g' -e 's/ [^-][^ ]*//g' -e 's/^ *//g'
```

---

# grep

- Searches input stream that contain any of the `foo`, `bar`, `baz` is available
```bash
somecommand | grep -e foo -e bar -e baz
```

- Searches input stream that contain any of the `foo`, `bar`, `baz` is available
- Same as previous command, `-E` means use extended regex format which allows to use `|` (OR)
```bash
somecommand | grep -E 'foo|bar|baz'
```

- Case insensitive search
```bash
somecommand | grep -i (case insensitive)
```

### LC_ALL=C usage with grep

- `LC_ALL=C` usage with grep will improve timing significantly.
- For example: `a_very_large_file.txt` is ~765.0MB

```bash
time grep "foo" a_very_large_file.txt
real	0m13.224s
user	0m0.131s
sys		0m1.026s
```

```bash
time LC_ALL=C grep "food" a_very_large_file.txt
real	0m0.127s
user	0m0.018s
sys		0m0.109s
```

Reason why `LC_ALL=C` is showing made numbers:

In Unix-like systems, `LC_ALL=C` is an environment variable setting that forces the system to use the "C" (or POSIX) locale, which is the simplest and most basic language environment.

Setting `LC_ALL=C` has several key effects:

- Override Precedence: `LC_ALL` is the strongest locale variable; it overrides all other `LC_*` variables (like `LC_TIME` or `LC_NUMERIC`) and the LANG variable. You can use `locale` command to find out all `LC_*`.

- English-Only Environment: It sets the system language to English for command output, error messages, and manual pages.

- Byte-Wise Processing: In this locale, characters are treated as single bytes and character encoding is restricted to ASCII. This avoids complex multi-byte (UTF-8) processing.

NOTE: UTF-8 can represent over 110,000 unique characters, supporting multiple writing systems worldwide.

- Performance Boost: Because it uses simple byte-based comparisons instead of complex language rules, it can significantly speed up commands like `grep`, `sort`, and `sed`.

- Predictable Sorting: It forces "ASCIIbetical" sorting, where uppercase letters (A-Z) come before lowercase letters (a-z) and symbols are sorted by their raw numeric byte values.
