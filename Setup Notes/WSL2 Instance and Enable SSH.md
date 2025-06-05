[Reference](https://endjin.com/blog/2021/11/setting-up-multiple-wsl-distribution-instances)


1. Download a Linux Distribution. This saves the current Ubuntu instance as a `.tar` file.

``` powershell
wsl --export Ubuntu C:\WSL\ubuntu-custom.tar
```

2. Create a New Custom WSL2 Instance. This creates a new WSL2 instance named `MyWSL`.

```powershell
wsl --import <CustomName> <InstallLocation> <TarFilePath> 

e.g

wsl --import MyWSL C:\WSL\mylinux C:\WSL\ubuntu-custom.tar

```

3. Start the Custom WSL2 Instance

```powershell
wsl -d MyWSL
```

4. Install & Enable SSH in WSL2
	 [Reference](https://www.youtube.com/watch?v=VjkE4dqdHX8)


```
sudo apt update && sudo apt install openssh-server -y
```

Edit this file inside WSL2:

```
sudo nano /etc/wsl.conf
```


```wsl.conf
[boot]
systemd=true

[user]
default=katsu
```

Then **restart WSL**:

```
wsl --shutdown
```

Re-open WSL and now you can use:

```
sudo systemctl enable ssh
sudo systemctl start ssh
```

Find WSl IP Address

```
hostname -I
```

connect to the same server/port, consider adding it to your SSH config file (`~/.ssh/config`):

```
Host myserver
    HostName example.com
    User user
    Port 2222

```

Then connect using:

```
ssh myserver
```

or 

```
ssh -p [PORT] [USER]@[HOST]
```

