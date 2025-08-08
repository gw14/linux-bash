Here's how you can create a passwordless SSH connection between two Linux servers. This process involves generating an SSH key pair on the **client** server and then copying the public key to the **remote** (or destination) server.

## Generating the SSH Key Pair

First, you'll need to generate an SSH key pair on the **client** server. This pair consists of a **private key** (which you must keep secure and private) and a **public key** (which you'll share with other servers).

1.  Open a terminal on your client server.
2.  Run the following command:

    `ssh-keygen -t rsa`

3.  The program will ask you to specify a file to save the key. The default location (`/home/user/.ssh/id_rsa`) is usually fine, so just press **Enter**.
4.  Next, you'll be prompted to enter a **passphrase**. While a passphrase adds an extra layer of security, leaving it blank is necessary for a completely passwordless connection, as it prevents you from having to enter the passphrase every time you connect. For this process, just press **Enter** twice to leave the passphrase empty.

This process creates two files in the `.ssh` directory: `id_rsa` (your private key) and `id_rsa.pub` (your public key). 

***

## Copying the Public Key to the Remote Server

Now that you have your public key, you need to copy it to the remote server. This is where the magic happens—by adding your public key to the remote server's `authorized_keys` file, you're telling that server to trust your client without a password.

The easiest way to do this is with the `ssh-copy-id` command.

1.  From your **client** server's terminal, run the following command, replacing `user` with the username on the remote server and `remote_server_ip` with the remote server's IP address or hostname:

    `ssh-copy-id user@remote_server_ip`

2.  The first time you run this, you'll be asked for the **password** of the remote user. Enter the password, and the command will automatically append your public key to the remote server's `~/.ssh/authorized_keys` file.

If `ssh-copy-id` isn't available, you can manually copy the key using this command:

`cat ~/.ssh/id_rsa.pub | ssh user@remote_server_ip "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"`

This command uses `cat` to read your public key, pipes it through an SSH connection, and then appends it to the `authorized_keys` file on the remote server.

***

## Verifying the Connection

To confirm that the passwordless connection works, try to SSH into the remote server from your client server.

`ssh user@remote_server_ip`

If everything was set up correctly, you should be logged into the remote server without being prompted for a password.

When you're still being prompted for a password after adding a new public key, it's almost always due to incorrect file permissions on the remote server. SSH is extremely particular about permissions to ensure security. If the permissions on the directories or files that make up your SSH configuration aren't strict enough, SSH will refuse to use the public key for authentication and fall back to password authentication.

Here are the most common reasons why this happens and how to fix them. 🛠️

***

### 1. Incorrect Permissions on the `.ssh` Directory

The `.ssh` directory on the remote server must be private. If any other user has write access, SSH will ignore the key.

* **The Problem:** The directory's permissions are too open (e.g., `drwxr-xr-x`).
* **The Fix:** Set the permissions to `700` (read, write, and execute for the owner only).

    `chmod 700 ~/.ssh`

### 2. Incorrect Permissions on the `authorized_keys` File

The `authorized_keys` file itself must also be private and readable only by the owner.

* **The Problem:** The file's permissions are too open (e.g., `-rw-r--r--`).
* **The Fix:** Set the permissions to `600` (read and write for the owner only).

    `chmod 600 ~/.ssh/authorized_keys`

### 3. Incorrect Permissions on the User's Home Directory

In some configurations, SSH also checks the permissions on the user's home directory. If it's writable by a group or other users, the connection will fail.

* **The Problem:** The home directory permissions allow group or other write access (e.g., `drwxrwx---`).
* **The Fix:** Remove write permissions for groups and others.

    `chmod go-w ~`

### 4. Public Key Not Copied Correctly

It's possible that the public key wasn't added to the remote server's `authorized_keys` file at all, or a typo or line break corrupted the key. 

* **The Problem:** The `authorized_keys` file is empty or the key is not in the correct format (one continuous line).
* **The Fix:** Use `ssh-copy-id` as it handles permissions and key appending automatically. If you're doing it manually, ensure the public key is a single line when you append it to the `authorized_keys` file.

### 5. SELinux Issues

If you're using a system with SELinux (like CentOS or Fedora), it might be blocking the SSH service from reading the key.

* **The Problem:** SELinux is preventing the `sshd` process from accessing the `authorized_keys` file.
* **The Fix:** This is a more advanced troubleshooting step. You'd typically need to check the SELinux logs (`/var/log/audit/audit.log`) for denials and use `semanage` to set the correct security context.

For more detailed debugging, you can use the `-v`, `-vv`, or `-vvv` flags when connecting via SSH. This will provide verbose output, which often includes the specific reason why key authentication failed.

`ssh -v user@remote_server_ip`

This video explains why passwordless SSH logins sometimes fail and provides a step-by-step walkthrough for checking and fixing directory permissions. [SSH passwordless login is not working | Why password less ssh not working](https://www.youtube.com/watch?v=_aWa-kzQWV4)
http://googleusercontent.com/youtube_content/0
