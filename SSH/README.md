
# What is SSH and why do I need it :confused:?

**SSH** is used to `remote login` into another `PC`:computer: and control it using the `command-line`.

This tutorial will talk about:
* :point_right: How to `install` **SSH** on **Ubuntu**
* :v: How to `login` using `RSA Public-Private Keys` instead of `username` and `password`
* :muscle: How to `login` from **Windows**

## Prerequisites 
* **Ubuntu** 21 or later installed on one computer
* **Windows** 10 or later installed on another
* **Putty** installed in **Windows**
* **WinSCP** installed in **Windows**


## Installing SSH/Linux side

:cd: Install the `SSH server`.

```
sudo apt update
sudo apt install openssh-server
```

Check status. Should appear green :white_check_mark:.
```
sudo systemctl status ssh
```

Allow the `SSH server` in the `firewall` :fire:. 
```
sudo ufw allow ssh
```

Install `UFW` if not installed. 
`sudo apt install ufw`

:black_square_button: Optional: 
* Disable the `SSH server` 
```
sudo systemctl stop ssh
sudo systemctl disable ssh
```
* Enable it back
```
sudo systemctl enable ssh
sudo systemctl start ssh
```
* Find local `IP`of **Ubuntu** machine
`ip -4 addr`



## Logging in with RSA Keys:key::key:/Windows side

Open **PuttyGen**:zap: and generate a `public/private` `key-pair`:key::key: and save both to individual files:`public_key, private_key`.

Login with **WinSCP** to the **linux** server:computer: and copy the `public_key`:key: to `/home/myuser/`.

Login with `ssh` using powershell and using `username` and `password`. 
```
ssh myuser@mylinuxhostip_or_hostname
```
Open or create the folder `.ssh` in your `/home/myuser` directory.
```
sudo mkdir /home/myuser.ssh
```
Open or create the file `authorized_keys` in the `.ssh` folder.
```
cd /home/myuser/.ssh
touch authorized_keys
```

Copy content from the `public_key`:key:  file to the `authorized_keys`.
```
cat /home/public_key > /home/myuser/.ssh/authorized_keys
```

Give ownership to `.ssh` folder to your user.
```
chown myuser /home/myuser/.ssh
```

**Linux** is now setup to receive incomming `ssh login requests` with your
`public-private` `key-pair` :key: :key: .

## Automate the RSA Login in Windows

Create a folder `.ssh` in `C:/Users/myuser/`.

Create a file `id_rsa`  in `C:/Users/myuser/.ssh`.

Open **PuttyGen**:zap: and import the `private_key`:key: .

Click on `Conversions` and click on `Export to OpenSSH key`.

You can save it as a `private_key.pem`:key: or any file, extension doesnt matter, but the content of the file will be in the **PEM** format.

Copy the content of `private_key.pem`:key: to `id_rsa`.

## Login with Powershell

Open a `powershell terminal` and type.
```
ssh myuser@myhost
```

You should now be able to `login` to the **linux server** without
a *password* :tada::tada::tada:.

## Troubleshooting :gun:

In case the steps above didn't work.

The login process works using a `public-private` `key-pair` generated with either **PuttyGen**:zap: or any other tool that can generate a **RSA** `public-private` `key-pair`. It must be a **RSA** `public-private` `key-pair` and it must be minimum `2048 bits` in length.

The `public key` is the one that is copied over to the remote machine, in this case, the **linux** machine. 

The `ssh server` must be installed on the machine and the `ssh` `port` of the server must be forwarded through the `firewall`, which is **UFW** in most cases. If not check what your firewall is and allow the `ssh port 22/TCP`.

On the **Windows** side the `private key` must be in the **PEM** format.
The contents of the file must be copied to the `id_rsa` file in the `.ssh` folder 
which resides in the `/Users/YourUser` in **Windows**.

## Using SSH with VSCode

:link:  [SSH with VSCode](https://github.com/octdobre/workspace/blob/main/VSCode/README.md#remote-ssh-with-rsa-key-key)

## :books: Documentation

:link:  [Dual Booting Ubuntu](https://www.youtube.com/watch?v=-iSAyiicyQY&t=564s 'Dual Booting Ubuntu')