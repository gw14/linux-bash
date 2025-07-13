No problem, here's a cheatsheet for Bash flow control operations.

-----

## Bash Flow Control Operations Cheatsheet

Bash provides several constructs for controlling the flow of execution in your scripts.

### 1\. Conditional Statements

#### `if` statement

Executes a block of code if a condition is true.

```bash
if [ condition ]; then
    # commands if true
fi
```

#### `if-else` statement

Executes one block if true, another if false.

```bash
if [ condition ]; then
    # commands if true
else
    # commands if false
fi
```

#### `if-elif-else` statement

Multiple conditions can be chained.

```bash
if [ condition1 ]; then
    # commands if condition1 is true
elif [ condition2 ]; then
    # commands if condition2 is true
else
    # commands if no condition is true
fi
```

#### `[[ ... ]]` vs `[ ... ]`

  * `[ ... ]` (test command): POSIX compliant, behaves like a regular command. Requires spaces around brackets and operators.
  * `[[ ... ]]` (new test command): Bash-specific, more powerful, and forgiving. Allows regular expression matching (`=~`), globbing without quotes, and logical operators (`&&`, `||`) directly within. Generally preferred in Bash scripts.

#### Common Conditions

  * **String Comparison:**
      * `[ "$VAR" = "string" ]` (equal)
      * `[ "$VAR" != "string" ]` (not equal)
      * `[ -z "$VAR" ]` (string is empty)
      * `[ -n "$VAR" ]` (string is not empty)
  * **Integer Comparison:**
      * `[ "$VAR" -eq 5 ]` (equal)
      * `[ "$VAR" -ne 5 ]` (not equal)
      * `[ "$VAR" -gt 5 ]` (greater than)
      * `[ "$VAR" -ge 5 ]` (greater than or equal)
      * `[ "$VAR" -lt 5 ]` (less than)
      * `[ "$VAR" -le 5 ]` (less than or equal)
  * **File Testing:**
      * `[ -e /path/to/file ]` (file exists)
      * `[ -f /path/to/file ]` (file exists and is a regular file)
      * `[ -d /path/to/directory ]` (directory exists)
      * `[ -r /path/to/file ]` (file is readable)
      * `[ -w /path/to/file ]` (file is writable)
      * `[ -x /path/to/file ]` (file is executable)
      * `[ file1 -nt file2 ]` (file1 is newer than file2)
      * `[ file1 -ot file2 ]` (file1 is older than file2)
  * **Logical Operators (within `[[ ... ]]` or with `-a`, `-o` in `[ ... ]`):**
      * `[[ condition1 && condition2 ]]` (AND)
      * `[[ condition1 || condition2 ]]` (OR)
      * `[ ! condition ]` (NOT)

### 2\. Case Statement

Provides a multi-way branch based on pattern matching.

```bash
case "$variable" in
    pattern1)
        # commands for pattern1
        ;;
    pattern2 | pattern3) # Multiple patterns
        # commands for pattern2 or pattern3
        ;;
    *) # Default case
        # commands for any other value
        ;;
esac
```

### 3\. Loops

#### `for` loop (Iterating over a list)

```bash
for item in item1 item2 "item with spaces"; do
    echo "Processing $item"
done
```

#### `for` loop (C-style iteration)

```bash
for (( i=0; i<5; i++ )); do
    echo "Iteration $i"
done
```

#### `for` loop (Iterating over command output)

```bash
for file in $(ls *.txt); do
    echo "Found text file: $file"
done
```

#### `while` loop

Executes commands as long as a condition is true.

```bash
count=0
while [ "$count" -lt 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

#### `until` loop

Executes commands as long as a condition is false.

```bash
count=0
until [ "$count" -ge 5 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### 4\. Loop Control

  * **`break`**: Exits the current loop entirely.

    ```bash
    for i in {1..5}; do
        if [ "$i" -eq 3 ]; then
            break
        fi
        echo "Processing $i"
    done
    # Output: Processing 1, Processing 2
    ```

  * **`continue`**: Skips the rest of the current iteration and proceeds to the next iteration of the loop.

    ```bash
    for i in {1..5}; do
        if [ "$i" -eq 3 ]; then
            continue
        fi
        echo "Processing $i"
    done
    # Output: Processing 1, Processing 2, Processing 4, Processing 5
    ```

### 5\. Functions

Organize your code into reusable blocks.

```bash
function my_function {
    echo "Hello from my_function!"
    echo "Arguments: $*" # All arguments
    echo "First argument: $1"
    return 0 # 0 for success, non-zero for failure
}

# Call the function
my_function "arg1" "arg2"
```

### 6\. Exit Status

Every command in Bash returns an exit status (0 for success, non-zero for failure).

  * `$?`: Contains the exit status of the most recently executed foreground command.
  * `exit N`: Exits the script with status `N`.

<!-- end list -->

```bash
ls /nonexistent_directory
if [ "$?" -ne 0 ]; then
    echo "Command failed!"
fi
```

### 7\. Logical Operators for Commands

Execute commands conditionally based on the success/failure of preceding commands.

  * **`command1 && command2`**: `command2` runs only if `command1` succeeds (exit status 0).
  * **`command1 || command2`**: `command2` runs only if `command1` fails (non-zero exit status).

<!-- end list -->

```bash
# Create a directory if it doesn't exist
mkdir my_dir && echo "Directory created."

# Try to compile, if it fails, print an error
gcc main.c -o my_app || echo "Compilation failed!"
```

-----
