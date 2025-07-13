-----

## Awk Cheatsheet

Awk is a powerful text-processing language that excels at pattern matching and data manipulation. Here's a quick reference to its common uses:

### Basic Structure

An Awk program generally follows this structure:

```awk
BEGIN { actions }
/pattern/ { actions }
END { actions }
```

  * **`BEGIN` block**: Executes once before processing any input lines.
  * **`/pattern/` block**: Executes for each line that matches the specified `pattern`. If no pattern is given, the `actions` are performed for every line.
  * **`END` block**: Executes once after all input lines have been processed.

### Common Commands and Concepts

  * **Printing Fields**:

      * `print $0`: Prints the entire current line.
      * `print $1`: Prints the first field.
      * `print $2, $3`: Prints the second and third fields, separated by the Output Field Separator (OFS, default is a space).
      * `print "Text", $1`: Prints literal text along with a field.

  * **Field Separators**:

      * **Input Field Separator (FS)**: Determines how input lines are split into fields.
          * `awk -F ',' '{print $1}' file.txt`: Uses a comma as the field separator.
          * `BEGIN { FS = ":" }`: Sets the field separator within the Awk script.
          * `awk '{print NF}' file.txt`: Prints the number of fields in the current record.
      * **Output Field Separator (OFS)**: Determines how fields are separated when printed.
          * `BEGIN { OFS = "\t" }`: Sets the output field separator to a tab.

  * **Record Separators**:

      * **Input Record Separator (RS)**: Defines how input lines (records) are separated. Default is newline.
      * **Output Record Separator (ORS)**: Defines how output records are separated. Default is newline.

  * **Built-in Variables**:

      * `NR`: Number of the current record (line number).
      * `NF`: Number of fields in the current record.
      * `FILENAME`: Name of the current input file.
      * `FNR`: Record number in the current file (resets for each new file).
      * `ARGC`: Number of command-line arguments.
      * `ARGV`: Array of command-line arguments.

  * **Patterns**:

      * `/regex/`: Matches lines containing the regular expression.
      * `$1 ~ /regex/`: Matches if the first field contains the regular expression.
      * `$1 !~ /regex/`: Matches if the first field does NOT contain the regular expression.
      * `NR == 1`: Matches the first line.
      * `NR >= 5 && NR <= 10`: Matches lines from 5 to 10.
      * `$3 > 100`: Matches lines where the third field's value is greater than 100.
      * `$NF == "last"`: Matches if the last field is "last".

  * **Control Flow**:

      * **`if/else`**:
        ```awk
        {
            if ($1 == "hello") {
                print "Found hello"
            } else {
                print "Not hello"
            }
        }
        ```
      * **`for` loop**:
        ```awk
        {
            for (i = 1; i <= NF; i++) {
                print $i
            }
        }
        ```
      * **`while` loop**:
        ```awk
        {
            i = 1
            while (i <= NF) {
                print $i
                i++
            }
        }
        ```
      * **`next`**: Skips the rest of the actions for the current record and proceeds to the next input record.
      * **`exit`**: Stops processing input and executes the `END` block.

  * **Arrays**:

      * `array[index] = value`: Assigns a value to an array element.
      * `for (i in array)`: Iterates through array indices.
      * `delete array[index]`: Deletes an array element.

  * **Functions**:

      * **String Functions**:
          * `length(string)`: Returns the length of a string.
          * `substr(string, start, length)`: Extracts a substring.
          * `index(string, substring)`: Returns the starting position of a substring.
          * `split(string, array, separator)`: Splits a string into an array.
          * `tolower(string)`, `toupper(string)`: Converts case.
      * **Mathematical Functions**:
          * `int(number)`: Returns the integer part.
          * `sqrt(number)`: Returns the square root.
          * `rand()`: Returns a random number (between 0 and 1).
      * **User-defined functions**:
        ```awk
        function my_function(arg1, arg2) {
            # function body
            return result
        }
        ```

### Examples

  * **Print lines containing "error"**:

    ```awk
    /error/ { print }
    ```

  * **Calculate the sum of the second column**:

    ```awk
    { sum += $2 }
    END { print "Total:", sum }
    ```

  * **Process CSV file and print first and third columns**:

    ```awk
    awk -F ',' '{print $1, $3}' data.csv
    ```

  * **Count occurrences of unique values in the first column**:

    ```awk
    { count[$1]++ }
    END { for (item in count) print item, count[item] }
    ```

This cheatsheet covers the most frequently used features of Awk. For more in-depth information, consult the Awk manual or online resources.
