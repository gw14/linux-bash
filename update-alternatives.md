Installing and managing multiple Java versions on a Linux system can be done using the `alternatives` command, which is a common utility for handling different versions of the same command. Here's a step-by-step guide on how to do it.

## 1\. Install Multiple Java Versions

First, you'll need to download and install the different Java Development Kits (JDKs) you want to use. You can typically download them from Oracle or use your distribution's package manager. For example, to install a specific version using `apt`:

```bash
sudo apt install openjdk-11-jdk
sudo apt install openjdk-17-jdk
```

After installation, the Java executables are usually located in a directory like `/usr/lib/jvm/`. You can check for their presence by listing the contents of this directory.

-----

## 2\. Configure Alternatives

The `alternatives` command is used to set up symbolic links for common commands like `java` and `javac`. This allows you to specify which version is the default.

### Adding the Java Versions to Alternatives

You need to register each Java version with the `alternatives` system. Use the following command syntax:

`sudo update-alternatives --install <link> <name> <path> <priority>`

Here's what each part means:

  * `<link>`: The symbolic link to be created (e.g., `/usr/bin/java`).
  * `<name>`: The name of the `alternatives` group (e.g., `java`).
  * `<path>`: The full path to the Java executable (e.g., `/usr/lib/jvm/java-11-openjdk-amd64/bin/java`).
  * `<priority>`: A number used to determine the default version. A higher number gives the version a higher priority.

Let's register two versions, Java 11 and Java 17:

```bash
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-11-openjdk-amd64/bin/java 1
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-17-openjdk-amd64/bin/java 2
```

We assign a higher priority to Java 17, so it will become the default.

-----

## 3\. Switching Between Java Versions

Once the versions are registered, you can easily switch between them using the `alternatives` command in **interactive mode**.

Run the following command:

`sudo update-alternatives --config java`

This will display a list of the registered Java versions and prompt you to choose which one to use as the default.

```
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                             Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-17-openjdk-amd64/bin/java       2         auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java       1         manual mode
  2            /usr/lib/jvm/java-17-openjdk-amd64/bin/java       2         manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

Just enter the selection number (e.g., `1` for Java 11) and press Enter to switch the default.

You can verify the current default version by checking the `java -version` output:

`java -version`

The `alternatives` command in Linux works by managing symbolic links to different versions of a program or utility. Instead of having to manually change paths in your system, `alternatives` provides a structured and easy way to switch the default version of a command that has multiple installations.

***

### How it Works

The `alternatives` system is a part of the **Debian** and **Red Hat**-based Linux distributions. It maintains a system of symbolic links in a directory like `/etc/alternatives/`. These links point to the actual program binaries located in their respective installation directories.

Here is a breakdown of the key components:

1.  **Symbolic Link (`link`):** This is the user-facing command that you run from the terminal (e.g., `/usr/bin/java`). This link doesn't point directly to a binary. Instead, it points to a symbolic link in the `/etc/alternatives/` directory.

2.  **Alternatives Link (`slave`):** This is the symbolic link in the `/etc/alternatives/` directory (e.g., `/etc/alternatives/java`). This link is the one that `alternatives` actively manages, and it points to the specific version of the program you have chosen.

3.  **Real Path (`path`):** This is the actual location of the program's executable file (e.g., `/usr/lib/jvm/java-17-openjdk-amd64/bin/java`).



***

### The Management Process

When you use the `update-alternatives` command, you're interacting with this system:

* **Installing a Version:** The `install` command registers a new version of a program with the alternatives system. You provide the user-facing link, a name for the alternatives group, the path to the executable, and a **priority** number. Higher priority numbers are used to automatically select a default version.

* **Switching Versions:** The `config` command allows you to manually choose which version of a program the alternatives link should point to. This is done through an interactive menu. When you make a selection, `alternatives` updates the symbolic link in `/etc/alternatives/` to point to the new path, effectively changing the default command.

By managing the symbolic links in this centralized way, `alternatives` ensures that all related scripts and commands on your system use the correct version of the program without you having to manually change `PATH` variables or other configuration files.
