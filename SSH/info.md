The term OpenSSH refers to the software implementation of the Scure Shell used in the system. SSH is used to securely
run a shell on a remote system. SSH secures communication through public-key encryption. 

## SSH key-based authentication
Users can authenticate ssh logins without a password by using public key authentication, 2 keys are generated, a private key and a public one. The private key file is used as the authentication credential, and like a password must be kept secret and secure. The public key is copied to systems the user wants to log into, and is used to verify the private key. An SSH server that has the public key can issue a challenge that can only be answered by a system holding a specific private key. As a result, you could authenticate using he presence of your key. This allows you to access systems in a way that does not require typing a password every time, but is still secure.

Key generation is done using the ssh-keygen command. This generates the private key ~/.ssh/id_rsa and the public key ~/.ssh/id_rsa.pub.
Before key-based authenticatoin can be used, the public key needs to be copied to the destination system. This can be done with ssh-copy-id.

## Passphrase
During key generation, there is the option to specify a passphrase which must be provided in order to access your private key. However, this means the passphrase must be eneterd whenever the key is used, making the authentication process no longer password-less. This can be avoided using ssh-agent.

## Workflow description:

1. When an ssh client connects to an SSH server, before the client logs in, the server sends it a copy of its public key.
This is used to set up the secure encryption for the communication channel and to authenticat the server to the client.
The first time a user uses ssh to connect to a particular server, the ssh command stores the server's public key in the
user's ~/.ssh/known_hosts file. Every time the user connects after that, the client makes sure it gets the same public key from the server by comparing the server's entry in the ~/.ssh/known_hosts file to the public key the server sent. If the keys do not match, the client assumes that the network traffic is being hijacked or that the server has been compromised, and breaks the connection.


