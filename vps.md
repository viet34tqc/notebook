# VPS

## Create ssh key

It's similar to creating local ssh key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## SSH to VPS

To successfully SSH from a client to a server, the following conditions must be met:

- SSH services must be running. Check by: `systemctl status sshd`
- Server must be reachable
- SSH Port must be open: the default port of SSH is `22`. The server's firewall must allow incoming connections on port 22
- SSH daemon accepts your authentication: there are two types of authentication
  - Password authentication: often applied for normal user, not root. You are prompted to enter the password evertime you ssh
  - SSH key authentication
  	- You generate a key pair on the client: `ssh-keygen -t ed25519 -C "your_email@example.com"`
  	- Then copy the public key to the server: `ssh-copy-id user@server_ip`
  	- The server appends your public key in: `~/.ssh/authorized_keys`
- SSH client must trust the server
  - On first connection, SSH prompts you to verify the server's public key fingerprint.
  - Once accepted, it's stored in `~/.ssh/known_hosts`
- Next time you SSH into the server:
  - Your client sends a signature using your private key
  - The server checks if it matches a public key in authorized_keys
  - If it matches -> login without a password
  
**Summary**: To ssh to a vps using keys pair, we need a pair of key, public and private key. Public key must be put into the `authorized_keys` on VPS. VPS checks the private key to match the public key. If other computer, for example, github actions server, needs to connect to your vps, just copy your private key to the github actions' workflow file

## `known_hosts`

The `known_hosts` contains the list of servers you've connected to before and to verify their identity in the future. It's located in the `.ssh` folder on your machine.

Each line in the file represents one known host, and includes:

- The hostname or IP address
- The encryption algorithm (e.g., ssh-ed25519, ssh-rsa)
- The server's public key (When you SSH into a server for the first time, your computer receives the server's public key)

Example: `example.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC1Gv+eCptkD9L1zK+m9K/1HjOymcDqzTJ+gWZ4F7nOw`

**Usage**:

- Convenience: Prevents having to confirm the fingerprint every time you connect to the same server.

## `authorized_keys`

The `authorized_keys` file is used on an SSH server to store a list of public keys **that are allowed to connect without a password**.

For each user on the server, the file is located in their home directory: `~/.ssh/authorized_keys`. So for `ubuntu` user, it would be: `/home/ubuntu/.ssh/authorized_keys`