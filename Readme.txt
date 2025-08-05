# SSH Access to Legacy Router (Cisco 7200)

In this project, I used a Cisco 7200 router image (running legacy IOS) inside GNS3.

# ⚠️ Issue:
The router does not support modern SSH cryptographic algorithms used by default in modern OpenSSH clients. When trying to connect, the following error appears:

----Unable to negotiate with 192.168.60.1 port 22: no matching key exchange method found.


This is because the router only supports older algorithms like:
- `diffie-hellman-group1-sha1`
- `diffie-hellman-group14-sha1`
- `ssh-rsa`
- `aes128-cbc`

## ✅ Solution:
To connect successfully from a modern Linux SSH client (e.g. Alpine), I created a custom SSH config to allow these older algorithms.

## nano ~/.ssh/config
Host r3
    HostName 192.168.60.1
    User cisco
    KexAlgorithms +diffie-hellman-group14-sha1
    HostKeyAlgorithms +ssh-rsa
    Ciphers +aes128-cbc


## chmod 600 ~/.ssh/config

                  💡 Usage:
ssh r3

