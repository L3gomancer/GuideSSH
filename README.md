# GuideSSH
A brief guide to using SSH

SSH (Secure SHell) is a way to connect your computer to another computer or server. You can use this if you have a web project you want to host on a server you own, or if you have a Linux device you need to transfer files to such as a smart-home product or a Raspberry Pi. This guide will cover ways to connect to a Linux server from Mac or Linux and will cover generating your own SSH keys. It assumes basic knowledge of the Bash command line but feel free to read teh manual for every command mentioned here, such as `man ssh-keygen`.

## Summary

To access a server quickly
```bash
ssh <user>@<IP>
```
To transfer a file to the server's Home directory, use 'scp' (Secure CoPy)
```bash
scp <file> <user>@<IP>:~
```
To transfer all files in the current local folder to 'Documents' in server's Home use a glob
```bash
scp * <user>@<IP>:~/Documents
```

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

### Log in

There are two main ways to use SSH. For speed or infrequent use, the `ssh` command can be run with no options and just the server details as the argument. This will require the system password for the server to be entered, much like logging in to a machine locally as a user. It will also prompt you to type "y" to add the server details to a file '~/.ssh/known_hosts'. The folder '.ssh/' will be created in your home directory if it did not already exist.
```bash
ssh <user>@<IP>
```
Note that if your server has a hostname associated with it then that can be used instead of the IP address.
```bash
ssh <user>@<hostname>
```
If the server's settings change in the future because, for example, your device moved on your wireless network and was assigned a new IP then you may get a scary warning when you try to connect again. Ignore it and manually edit 'known_hosts' as the warning suggests.

### Transfer Files

To transfer a file to the server's Home directory, use 'scp' (Secure CoPy)
```bash
scp <file> <user>@<IP>:~
```
To transfer all files in the current local folder to 'Documents' in server's Home use a glob
```bash
scp * <user>@<IP>:~/Documents
```

### Generate The SSH Keys

Another way to use SSH is to generate a pair of secure keys, one public key stored on the server which acts like a lock, and a private key which is stored on your connecting machine. These allow the machines to recognise each other automatically without a password and keep a connection open. Much like real locks and keys, a public key may be publically visible since it is not usable without its counterpart, however a private key should not be shared with anyone and should only be associated with one machine. If a private key is lost then it should be discarded and a new one generated. Public keys are text files with the extension ".pub", private keys do not need an extension.

To specify a private key file name, its location and use the default encryption method
```bash
ssh-keygen -f ~/.ssh/<keyname>
```
Or to follow some prompts use the following and press return to skip entering a passphrase
```bash
ssh-keygen
```

### Place The Public Key

Copy the public key to the server (yes using the private key), enter the password
```bash
ssh-copy-id -i ~/.ssh/<priv-key> <user>@<host>
```
You can now delete the public key from your delevopment machine.

### Connect

It is now possible use the private key to connect to the server using the `-i` option
```bash
ssh -i ~/.ssh/<priv-key> <user>@<host>
```
### Configuration

Optionally, it is possible to make your local machine automatically associate the server with the corresponding private key

Add the private key to an authorisation agent
```bash
ssh-add <priv-key>
```
In the folder '~/.ssh' create a file 'config' and add a shortname to it
```bash
nano ~/.ssh/config
```
A template configuration file from ![](https://linux.die)
```
Host <shortname>
    HostName      <IP>
    IdentityFile  ~/.ssh/<publickey.pub>
    User          <username>
```
Now you can SSH in with
```bash
ssh <shortname>
```

Configuring a shortname makes using the above commands easier. Instead of transferring a file the long way
```bash
scp -i ~/.ssh/<priv-key> <user>@<host>
```
You can use the short way
```bash
scp <shortname>
```


## A Brief Commandline Primer
Unsure what to do after getting in? \
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
The prompt will change. Type `exit` or use ctrl+d to exit back to the shell of your development machine.

You can transfer files to the server in a similar way with the `scp` command. The "quick" way requires a password every time, but with configuration the command can be shorter and without needing a password.

Copy files with 'scp'. It asks for a password:
```bash
scp  a.txt  <user>@<hostname>:~
```
The ":" is needed and defaults to the home folder, though a path can be declared.

If all you want to do is stick files on the server for remote use then that's all you need to know.


