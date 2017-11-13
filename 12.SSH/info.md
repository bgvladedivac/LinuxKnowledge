The term OpenSSH refers to the software implementation of the Scure Shell used in the system. SSH is used to securely run a shell on a remote system. SSH secures communication through public-key encryption. 

## SSH key-based authentication
Users can authenticate ssh logins without a password by using public key authentication, a private key and a public one. The private key file is used as the authentication credential and must be kept secret and secure. The public key is copied to systems the user wants to log into, and is used to verify the private key. An SSH server that has the public key can issue a challenge that can only be solved with a specific private key. This allows you to access systems in a way that does not require typing a password every time, but is still secure.

Key generation is done using the **ssh-keygen** command => **~/.ssh/id_rsa** and the public key **~/.ssh/id_rsa.pub**. Before key-based authentication can be used, the public key needs to be copied to the destination system. This can be done with **ssh-copy-id**.

## Passphrase
During key generation, there is the option to specify a passphrase which must be provided in order to access your private key meaning the passphrase must be entered whenever the key is used, making the authentication process no longer password-less. This can be avoided using **ssh-agent**.

## Workflow description

1. When an ssh client connects to an SSH server, before the client logs in, the server sends it a copy of its public key.
This is used to set up the **secure encryption** for the communication channel and to **authenticate the server** to the client.
The first time a user uses ssh to connect to a particular server, the ssh command stores the server's public key in the
user's **~/.ssh/known_hosts** file. Every time the user connects after that, the client makes sure it gets the same public key from the server by comparing the server's entry in the **~/.ssh/known_hosts** file to the public key the server sent. If the keys do not match, the client assumes that the network traffic is being hijacked or that the server has been compromised and breaks the connection.

2. The client sends his public key to the server, so that the server will create a **decryption challenge** that could be answered with only 1 private key. The server is aware about the expected answer. The client public key will be stored in **~/.ssh/authorized_keys**.

3. If the client decrypts the message, the server lets the client in.

## Customizing SSH Service Configuration
Various aspects of the OpenSSH server can be modified in the configuration file **/etc/ssh/sshd_config**.
1. Prohibit the root user from logging in using SSH => **PermitRootLogin** yes/no
2. Prohibit password authentication using SSH => **PasswordAuthentication** yes/no
3. Configuring alternative ports => **Port** 22
4. Limiting user access => **Allow users**
