# Git

## Installation

## Bash

Copy: ctrl + insert
Paste: shift + insert

## SSH authentication

1. Check for existing ssh keys. Usually stored in home directory. From Bash run:

```console
ls ~/.ssh
```

You are looking for private and public key pairs named `id_rsa` (.pub is the public key). If you do not have a key pair then continue to the next step.

2. Create a new key-pair. From Bash run:

```console
ssh-keygen -o
```

Note: It is recommended to save them in the default location (continue by pressing enter).
Note: password is not neccessary
Note: -o option saves the private key in a format that is more resistant to brute-force password cracking than is the default format.

3. Use/send your public key to create a connection.

You can check your public key by running this from bash:

```console
cat ~/.ssh/id_rsa.pub
```

On Windows you can copy to clipboard by running:

```console
cat ~/.ssh/id_rsa.pub > /dev/clipboard
```

Connect to GitHub:

1.  Click on profile. Go to Settings
2.  Click SSH and GPG keys on the left navigation bar
3.  Click New SSH key
4.  Name the key (title, use something to identify the pc with the key), set type to Authentication key and paste the copied key into the Key box.
5.  Test the connection:

```console
ssh -T git@github.com
ssh -v -T git@github.com
```
