Ah, **CRLF**—the classic headache of cross-platform development! You are dealing with line endings. Windows uses **CRLF** (Carriage Return + Line Feed), while Mac and Linux use just **LF** (Line Feed).

If you don't configure this, Git will constantly think files have been modified even if you haven't changed a single line of code.

Here is how to configure **`core.autocrlf`** based on your operating system:

---

## ⚙️ The Recommended Settings

### 🖥️ If you are on Windows

You want Git to convert LF to CRLF when you check out code, but convert it *back* to LF when you commit it to the repository.

```bash
git config --global core.autocrlf true

```

### 🍏 / 🐧 If you are on Mac or Linux

You only want Git to make sure it fixes any accidental CRLF endings to LF when you commit. You don't need it to change anything when you check out code.

```bash
git config --global core.autocrlf input

```

### 🚫 If you want to turn it off completely

If you want Git to just leave your line endings exactly as they are (usually only recommended for specific, single-platform environments):

```bash
git config --global core.autocrlf false

```

---

## 🔍 How to check your current setting

If you want to see what your system is currently set to, run:

```bash
git config --global core.autocrlf

```

> 💡 **Pro-Tip for Teams:** To completely prevent line-ending drama across a team, create a file named `.gitattributes` in the root of your project repository and add this line:
> `* text=auto`
> This forces Git to handle line endings intelligently for everyone on the project, regardless of their personal global settings.
