# GuideSSH
A brief guide to using SSH

SSH (Secure SHell) is a way to connect your computer to another computer or server. Perhaps you have a web project you want to host from your own server, or you have a Raspberry Pi you need to transfer files between, or for automated communication between devices. This guide will cover ways to connect to a Linux server from Mac or Linux and will cover generating your own SSH keys. It assumes basic knowledge of the command line.

## Summary
To access a server quickly
`ssh <user>@<IP>`
Type y to add the server details to '~/.ssh/known_hosts'. If server's settings change (your Pi moves) then ignore the scary warning and manually edit 'known_hosts'

To transfer a file to the server's Home directory do
`scp <target> <user>@<IP>:`
Remember the colon
To transfer all files in the current local folder to 'Documents' in server's Home do
`scp * <user>@<IP>:~/Documents`
