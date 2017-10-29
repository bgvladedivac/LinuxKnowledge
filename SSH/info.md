The term OpenSSH refers to the software implementation of the Scure Shell used in the system. SSH is used to securely
run a shell on a remote system. SSH secures communication through public-key encryption. 

Workflow description:

1. When an ssh client connects to an SSH server, before the client logs in, the server sends it a copy of its public key.
This is used to set up the secure encryption for the communication channel and to authenticat the server to the client.
The first time a user uses ssh to connect to a particular server, the ssh command stores the server's public key in the
user's ~/.ssh/known_hosts file.
