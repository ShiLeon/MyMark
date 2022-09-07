## git密钥对

```shell
ssh-keygen -t ed25519 -C "your-email"
```

## git 提交文件

```
git add xxxx/.
git commit -m 'xxxxxx'
```

## git分支处理

```
git branch
git branch -r

//切换分支(新版本用switch)
git switch xxx
git checkout xxx
//切换并创建分支
git checkout -b xxx
git switch -c xxx


git log --graph --pretty=oneline --abbrev-commit    //显示分支结构
```

## git解决冲突

```
git merge xxx      //从xxx分支merge到当前分支
git diff --check   //检查分支冲突
cat xxx            //查看文件
```
