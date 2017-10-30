## Systemd
System startup and server processes are managed by the systemd System and Service Manager. Systemd provides a method for activating resources, server daemons and other processes, both at boot time and on a running system. 

Daemons are processes that wait or run in the background performing various tasks. Generally, daemons start automatically at boot time and continue to run until shutdown or until they are manually stopped. By convention, the names of many daemon programs end in the letter "d".

Systemd and its features:
* on-demand starting of daemons without requiring a separate service.
* a method of tracking related processes together by using Linux control groups.

## Units
The **units** are the main systemd object.
systemd records initialization instructions for each daemon in a configuration file that uses a declarative language. Unit file types
include **service**, **socket**, **device**, **mount**, **automount**, **swap**, **target**, **path**, **timer**, **snapshot**, **slice** and **scope**.

Unit files are loaded from a set of paths determined during compilation. 


| Path          | Descrition           
| --------------           |:-------------:| -----:|
| /etc/systemd/system      | Local configurations |
| /run/systemd/system      | Runtime units      |
| /usr/lib/systemd/system  | Units of installed packages      |    

Key generation is done using the **ssh-keygen** command. This generates the private key **~/.ssh/id_rsa** and the public key **~/.ssh/id_rsa.pub**. Before key-based authenticatoin can be used, the public key needs to be copied to the destination system. This can be done with **ssh-copy-id**.

## Passphrase
During key generation, there is the option to specify a passphrase which must be provided in order to access your private key. However, this means the passphrase must be eneterd whenever the key is used, making the authentication process no longer password-less. This can be avoided using **ssh-agent**.

## Workflow description

1. When an ssh client connects to an SSH server, before the client logs in, the server sends it a copy of its public key.
This is used to set up the **secure encryption** for the communication channel and to **authenticate the server** to the client.
The first time a user uses ssh to connect to a particular server, the ssh command stores the server's public key in the
user's **~/.ssh/known_hosts** file. Every time the user connects after that, the client makes sure it gets the same public key from the server by comparing the server's entry in the **~/.ssh/known_hosts** file to the public key the server sent. If the keys do not match, the client assumes that the network traffic is being hijacked or that the server has been compromised, and breaks the connection.

2. The client sends his public key to the server, so that the server will create a **decryption challenge** that could be answered with only 1 private key. The server is aware about the expected answer. The client public key will be stored in **~/.ssh/authorized_keys**.

3. If the client decrypts the message, the server lets the client in.

## Customizing SSH Service Configuration
Various aspects of the OpenSSH server can be modified in the configuration file **/etc/ssh/sshd_config**.
1. Prohibit the root user from logging in using SSH => **PermitRootLogin** yes/no
2. Prohibit password authentication using SSH => **PasswordAuthentication** yes/no
3. Configuring alternative ports => **Port** 22
4. Limiting user access => **Allow users**
