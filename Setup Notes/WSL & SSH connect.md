
### Create new instance

[Reference](https://endjin.com/blog/2021/11/setting-up-multiple-wsl-distribution-instances)

1. export existing Distributor
```powershell
wsl --export <distribution name> <export file name>
```

e.g

```powershell
wsl --export Ubuntu ubuntu.tar
```


2. import .tar file

```powershell
wsl --import <new distribution name> <install location> <export file name>
```

e.g 
```powershell
wsl --import UbuntuDev1 .\UbuntuDev1 ubuntu.tar
```


### Using new env

```powershell
wsl -d <new distribution name>
```

e.g
```powershell
wsl -d UbuntuDev1
```


### SSH Setup

[Reference](https://www.youtube.com/watch?v=VjkE4dqdHX8)


```ubuntu
sudo apt install openssh-server
```


```ubuntu
sudo nano etc/ssh/sshd_config
```

Change to Port 2200 


Run ssh on linux terminal

```ubuntu
sudo systemctl enable ssh
```

Check ssh status if running

```ubuntu
sudo systemctl status ssh
```

 open config
 
```ubuntu
sudo nano /etc/wsl.conf
```

shut down then restart wsl in new tab

```powershell
wsl --shutdown
```

### Connect VSCode to WSL via SSH 

open VSCode

1.  Search >Remote-SSH: Connect to Host
2. then select Configure SSH Hosts....
3. edit C:>Users\<pc_name>.ssh\config


Host <UbuntuWSL>

    HostName localhost

    Port <Port>

    User <your_username>
   
sudo sh -c "echo 'katsu ALL=(root) NOPASSWD: /usr/sbin/service ssh start' >/etc/sudoers.d/service-ssh-start"

cursor ai 





