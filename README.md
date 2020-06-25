# GuideSSH
A brief guide to using SSH

SSH (Secure SHell) is a way to connect your computer to another computer or server. You can use this if you have a web project you want to host on a server you own, or if you have a Linux device you need to transfer files to such as a smart-home product or a Raspberry Pi. This guide will cover ways to connect to a Linux server from Mac or Linux and will cover generating your own SSH keys. It assumes basic knowledge of the Bash command line.

## Summary

To access a server quickly, run this command and type "y" to add the server details to '~/.ssh/known_hosts'.
```bash
ssh <user>@<IP>
```

To *print* the current *working directory*
```bash
pwd
```
To *list* files and directories in the current directory
```bash
ls
```
To edit a text file
```bash
nano <file>
```

To transfer a file to the server's Home directory, use 'scp' (Secure CoPy)
```bash
scp <file> <user>@<IP>:~
```
To transfer all files in the current local folder to 'Documents' in server's Home use a glob
```bash
scp * <user>@<IP>:~/Documents
```

---

## More Detail
### Setup
The setup is assumed to be:
* A development computer you have admin/root access to, connected to a local network by router, with OpenSSH
* A server or device you have admin/root access to, connected to the same network or the internet, with OpenSSH

To tell if your device has OpenSSH bundle installed try viewing the manual page
```bash
man ssh
```

Find out the IP of your server. If you physically set it up on your home network you can find this out by accessing the configuration page of your router, often by browsing to a URL or IP address from a device connected to the router. Or if you have direct local access to the server then a command will show its IP
```bash
hostname -I
```

### Generate The SSH Keys

To specify a name, a location and use RSA
```bash
$ ssh-keygen -f ~/.ssh/<keyname>
```
Or to follow prompts, name the file, and skip the passphrase
```bash
ssh-keygen
```
Copy the public key to the server (yes using the private key), enter the pw
```bash
ssh-copy-id -i ~/.ssh/<priv-key> <user>@<host>
```
Add the key to the auth agent
```bash
ssh-add <priv-key>
```
Optionally add a shortname to 'config' (create file). Template from ![](https://linux.die)
```bash
nano ~/.ssh/config
```

### Log in



### Tests

```bash
ssh-add -l
```
```bash
ls ~/.ssh
```
```bash
less ~/.ssh/config
```
```bash
less ~/.ssh/known_hosts
```

### Tests On The Server

```bash
less ~/.ssh/authorized-keys
```
Who is logged in
```bash
w         # or 'who'
```

## AWS

When you rent a server (like AWS) you are given an IP, a default username, a password, and a private key. This means you can skip generating the keys and just SSH in.
```bash
ssh <user>@<IP>
```
Or
```bash
ssh <user>@<hostname>
```
The prompt will change. Type `exit` or use ctrl+d to exit back to the shell of your development machine.

You can transfer files to the server in a similar way with the `scp` command. The "quick" way requires a password every time, but with configuration the command can be shorter and without needing a password.

Copy files with 'scp'. It asks for a password:
```bash
scp  a.txt  <user>@<hostname>:~
```
The ":" is needed and defaults to the home folder, though a path can be declared.

If all you want to do is stick files on the server for remote use then that's all you need to know.


