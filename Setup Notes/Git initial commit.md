
## Setup

 initialize git

```powershell
git init
```


```powershell
git add .
```


```powershell
git commit -m "first commit"
```



```powershell
git branch -M mai
```


```powershell
git remote add origin <remote url>
```



```powershell
git push -u origi main
```


## Error encounter

_Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref._

## Solution


```powershell
git branch --set-upstream-to=origin/main main
```


```powershell

git pull origin main --allow-unrelated-histories
```

the you can not execute the git push







