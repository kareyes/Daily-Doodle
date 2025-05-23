
open as wsl in powershell
```
wsl.exe -d Debian
```

open wsl with user

```powershell
wsl -d <new distribution name> -u <username>
```


display package source path

```powershell
which moon
```

current user

```powershell
whoami
```

current path

```powershell
pwd
```

Notes:
Add sudo if permission denied


list of WSL 
```
wsl --list --verbose
```

open wsl as root in powershell

```
wsl -d Astrid -u root
```

switch user

```
su <user> 

e.g.

su root
```


list of groups and permission access

```
whoami && groups && sudo -l
```


modify a user account append to sudo group

It **adds the user `katsu` to the `sudo` group**, **without removing** them from any other groups

Note: BEWARE

```
sudo usermod -aG sudo katsu

```

backup WSL 

```
wsl --export <WSL> D:\WSLBackups\wsl-backup.tar
```


