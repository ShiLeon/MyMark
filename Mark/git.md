#              git

## git配置用户信息

```shell
$ git config --global user.name "Leon"
$ git config --global user.email "1768808494@qq.com"
```



## git密钥对

```shell
ssh-keygen -t ed25519 -C "your-email"
```

必须登记公钥，才能git clone别人的库

## git 提交文件

```shell
git add xxxx/.
git commit -m 'xxxxxx'
git remote add origin git@github.com:xxxxx//创建远程仓库
git push (-u) origin  gh-pages //推送（可加-u参数简化之后推送）
```

## git删除分支 

```shell
git branch -d xxx
git branch --delete xxx
```

## git远程分支操作

```shell
git pull origin 远程分支：本地分支	//将远程指定分支拉取到本地指定分支上
git pull origin 远程分支		   //将远程指定分支拉取到本地当前分支上
git pull						 //将与本地当前分支同名的远程分支拉取到本地当前分支上(需要做关联)

git push origin 本地分支名:远程分支名 //将本地指定分支推送到远程指定分支上
git push origin 本地分支名		  //将本地指定分支推送到与本地指定分支同名的远程分支上
git push 					     //将本地当前分支推送到与本地当前分支同名的远程分支上(需要做关联)
```

## git删除远程分支

```shell
git branch -a //查看所有分支
git push origin -d xxx
```

## git分支处理

```git
git branch
git branch -r

//切换分支(新版本用switch)
git switch xxx
git checkout xxx
//切换并创建分支
git checkout -b xxx
git switch -c xxx

git log --graph --pretty=oneline --abbrev-commit    //显示分支结构
```

## git tag使用

```
git tag -a v1.0 -m “Release 1.0” //打上标签并添加注释
git push origin v1.0	//将标签推送到远程
```



## git解决冲突

```git
git merge xxx      //从xxx分支merge到当前分支
git diff --check   //检查分支冲突
cat xxx            //查看文件
git merge --abort //中止merging
```

## git版本回退

```git
git log         //只展示当前版本之前的版本
git reflog      //展示全部版本
git log --pretty=oneline
git reset --hard //需要回退版本的key
git reset --hard HEAD^
reset --hard HEAD~ //需要回退版本次数
```

## git rebase

```
git pull origin master:feature --rebase
```

## git log使用

`git log --pretty=format:"%h - %an, %cd : %s" --graph`

### 一些参数

```git
git log -p -2 //仅显示最近两次更新
git log --stat //增改行数统计
git log --graph//显示分支图形
```

![2023-02-02-15-54-29-image](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/2023-02-02-15-54-29-image.png)

### 格式化

+ 简单模式

```git
git log --pretty=oneline
git log --pretty=short
git log --pretty=full
git log --pretty=fuller
```

+ 定制格式

```git
git log --pretty=format:"%h - %an, %cd : %s"
```

![2023-02-02-15-54-11-image](https://gitee.com/ShiLeon/my-mark-image-repository/raw/master/2023-02-02-15-54-11-image.png)

