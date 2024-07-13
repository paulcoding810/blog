+++
title = 'Configure Ssh Aliases'
tags = ["ssh"] 
categories = ["Code"]
date = 2024-07-13T08:42:43+07:00
draft = false
+++

## Tutorial to connect your server with ssh alias

This tutorial show you how to ssh to your server without rememering the server's ip

```bash
ssh your_server
```

### 1.  ssh-keygen
  
- Create an ssh private/public keypair. 
  
  Here I named my key ***personal***. Do replace yours.
- For passwordless experience, let passphrase be empty.
```bash
cd $HOME/.ssh
ssh-keygen -t rsa
```
![ssh-keygen](/ssh-keygen.png "Create ssh key")

### 2. Copy public key to server

You can manully copy the public key or use the command for that

#### 2.1. Manually copy

- Print and copy your public key

```bash
cat ~/.ssh/personal.pub
```

On your server, pass the public key to the end of `$HOME/.ssh/authorized_keys`

![server-authorized-keys](/ssh-authorized_keys.png)

#### 2.2. Use ssh-copy-id

- Copy the public key to server
- My server ip is `192.168.1.111`. Do replace with yours.

```bash
ssh-copy-id -i personal root@192.168.1.111
```
![ssh-copy-id](/ssh-copy-id.png)


- Chmod server's ssh files permission for security (optinal)

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
service ssh restart
```
![security chmod](/ssh-chmod.png)



- Try login by password

```bash
ssh 'root@192.168.1.111'
```
![ssh login by password](/ssh-creds-login.png)

Hura! Now we can connect to server through ssh. Next, let's create an alias for the server.

### 3. SSH Alias

- From your local machine, add these line to the end of  `$HOME/.ssh/config`

```
Host hp
        HostName 192.168.1.111
        User root
        IdentityFile ~/.ssh/personal
```

- Replace `hp`, `192.168.1.111`, `root`, `~/.ssh/personal` with your alias, ip, server user and the private key path


```bash
cd $HOME/.ssh
vi config
```

![ssh config](/ssh-config.png)

### 4. Test

Well done, you can ssh to your server using alias now!
  
![ssh alias login](/ssh-alias-login.png)