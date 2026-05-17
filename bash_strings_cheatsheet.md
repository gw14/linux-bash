## 1. Basics: Length & Default Values

Assume `str="Hello World"` for these examples.

| Operation | Syntax | Example | Result |
| --- | --- | --- | --- |
| **String Length** | `${#var}` | `${#str}` | `11` |
| **Use Default** (if empty) | `${var:-default}` | `${unset_var:-"Backup"}` | `"Backup"` |
| **Assign Default** (if empty) | `${var:=default}` | `${unset_var:="Backup"}` | (Assigns and returns `"Backup"`) |

---

## 2. Substrings (Slicing)

Bash uses zero-based indexing for string slicing: `${var:offset:length}`.

```bash
str="Alphabet"

# Extract from index 4 to the end
echo ${str:4}        # Output: habet

# Extract 3 characters starting from index 1
echo ${str:1:3}      # Output: lph

# Extract the last 2 characters (Note the space before the minus sign!)
echo ${str: -2}      # Output: et

```

---

## 3. Search and Replace

These operations are incredibly useful for cleaning up data or changing paths.

```bash
str="apple-orange-apple-banana"

# Replace FIRST match
echo ${str/apple/kiwi}    # Output: kiwi-orange-apple-banana

# Replace ALL matches (Global)
echo ${str//apple/kiwi}   # Output: kiwi-orange-kiwi-banana

# Replace if it matches at the START (#)
echo ${str/#apple/kiwi}   # Output: kiwi-orange-apple-banana

# Replace if it matches at the END (%)
echo ${str/%banana/kiwi}  # Output: apple-orange-apple-kiwi

# Delete a match (Replace with nothing)
echo ${str/orange-}       # Output: apple-apple-banana

```

---

## 4. Stripping Substrings (Prefix/Suffix Removal)

Think of `#` as looking at the **start** (left side) of the string, and `%` as looking at the **end** (right side).

* **Single character (`#` or `%`)**: Matches the *shortest* possible pattern.
* **Double character (`##` or `%%`)**: Matches the *longest* possible pattern.

```bash
path="/var/log/nginx/access.log"

# Strip shortest prefix from start matching "*/" (Great for getting filenames)
echo ${path##*/}   # Output: access.log

# Strip longest prefix from start matching "*/"
echo ${path#*/}    # Output: var/log/nginx/access.log

# Strip shortest suffix from end matching ".*" (Great for removing extensions)
echo ${path%.*}    # Output: /var/log/nginx/access

# Strip longest suffix from end matching "/*" (Great for getting directories)
echo ${path%/*}    # Output: /var/log/nginx

```

---

## 5. Changing Case

*(Requires Bash 4.0 or newer)*

```bash
str="GoLang"

# Convert FIRST character to uppercase
echo ${str^}    # Output: GoLang (already upper)

# Convert ALL characters to uppercase
echo ${str^^}   # Output: GOLANG

# Convert FIRST character to lowercase
echo ${str,}    # Output: goLang

# Convert ALL characters to lowercase
echo ${str,,}   # Output: golang

# Toggle case of ALL characters
echo ${str~~}   # Output: gOlANG

```

---

## 6. Checking for Substrings (Matching)

The cleanest way to check if a string contains a substring inside an `if` statement is using `[[ ]]` with wildcards.

```bash
str="The quick brown fox"

# Check if string CONTAINS "brown"
if [[ "$str" == *"brown"* ]]; then
    echo "Found brown!"
fi

# Check if string STARTS with "The"
if [[ "$str" == "The"* ]]; then
    echo "Starts with The"
fi

# Check if string ENDS with "fox"
if [[ "$str" == *"fox" ]]; then
    echo "Ends with fox"
fi

```

---

> **Pro-Tip:** Always wrap your variables in double quotes (e.g., `"$str"`) when passing them to commands or evaluations to prevent unexpected word splitting if the string contains spaces!
