The primary Linux command for viewing **escaped characters** and other **non-printing characters** in a file is **`cat`** with specific options, or the **`od`** (octal dump) command.

Since "escaped characters" often refer to sequences like `\n` (newline), `\t` (tab), or other control characters, these commands will display them in a visible, standard format.

-----

## 1\. Using the `cat` command

The `cat` command, when used with the right flags, can display non-printing characters using **caret notation** (e.g., `^M` for carriage return, `^I` for tab) or by explicitly showing line endings.

| Command | Description |
| :--- | :--- |
| **`cat -v <filename>`** | Displays non-printing characters (except for tab and newline) using **caret notation** (`^`) and `M-` prefix. |
| **`cat -A <filename>`** | Equivalent to **`cat -vET`**. This is often the best choice for a quick, comprehensive view: \<ul\>\<li\>**`-v`**: Shows non-printing characters.\</li\>\<li\>**`-E`**: Displays a **`$`** at the end of each line (shows where newlines are).\</li\>\<li\>**`-T`**: Displays tab characters as **`^I`**.\</li\>\</ul\> |
| **`cat -vet <filename>`** | An older equivalent that achieves the same result as `-A`. |

**Example:**
If your file `test.txt` contains a tab, a carriage return, and a newline:

```bash
$ cat -A test.txt
This line has a ^I tab and a carriage return^M$
This is the next line.$
```

-----

## 2\. Using the `od` (Octal Dump) command

The `od` command displays file content in various formats (octal, hexadecimal, decimal, etc.), which is excellent for inspecting the exact byte values of every character, including non-printing ones represented by **C-style escape sequences** (like `\t`, `\n`, `\r`).

| Command | Description |
| :--- | :--- |
| **`od -c <filename>`** | Displays the file content as ASCII characters, using **C-style escape sequences** (e.g., `\n`, `\t`) for non-printing characters, and three-digit octal for other non-ASCII characters. |
| **`od -tx1 -tc <filename>`** | A common verbose combination:\<ul\>\<li\>**`-tx1`**: Shows the content in **hexadecimal** (one byte per entry).\</li\>\<li\>**`-tc`**: Shows the content in **ASCII** characters (C-style escaped).\</li\>\</ul\> |

**Example (using `od -c`):**
If your file `test.txt` contains a tab, a carriage return, and a newline:

```bash
$ od -c test.txt
0000000   T   h   i   s       l   i   n   e       h   a   s       a  \t
0000020   t   a   b       a   n   d       a       c   a   r   r   i   a
0000040   g   e       r   e   t   u   r   n  \r  \n   T   h   i   s
0000060   i   s       t   h   e       n   e   x   t       l   i   n   e
0000100  \n
0000101
```

*Note the `\t` for the tab, `\r` for the carriage return, and `\n` for the newline.*

-----

## 3\. Using `vi`/`vim`

If you are comfortable with the **Vim** text editor, you can view the file, and then enable a special mode to display non-printing characters:

1.  Open the file: `vim <filename>`
2.  Enable the list mode by typing a colon followed by `set list` and pressing Enter:
    ```
    :set list
    ```
    *This will show tabs as `^I`, trailing spaces as `-`, and end-of-lines (newlines) as `$`*.
3.  To turn it off, type `:set nolist` and press Enter.
