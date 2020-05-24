# GitTrain 

https://github.com/HerbertHe/GitCommitTraining

[Git官网Book-zh](https://git-scm.com/book/zh/v2)！！！

```shell
~~echo "# GitTrain" >> README.md~~ # Linux
echo # GitTrain >> README.md # Windows
git init
git add README.md
~~git commit -m "first commit"~~
git commit -m ":tada: Initialize Repo"
git remote add origin git@github.com:zhouqiaor/GitTrain.git
git push -u origin master
```

## 安装配置

1. 安装Git

```shell
apt install git
```

1. 配置Git

```sh
git config --global user.name "user_name"
git config —global user.email "email_id"
```

### SSH Keys

[Git - 生成 SSH 公钥](https://git-scm.com/book/zh/v2/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)

[Git：SSH、SSH与HTTP区别、git常用命令](https://blog.csdn.net/liuxiaodong400/article/details/81221834)

> Git 可以使用四种不同的协议：本地协议（Local），HTTP 协议，SSH（Secure Shell）协议及 Git 协议。    

* 本地创建 SSH Keys

```sh
ssh-keygen -t rsa -C "XXX@163.com”
```

> -t 指定密钥类型，默认是 rsa ，可以省略。    
> -C 设置注释文字，比如邮箱。  
> -f 指定密钥文件存储文件名。  


使用默认文件名（推荐）,设置push密码

* 将SSH key上传到github

`vim ~/.ssh/id_rsa.pub`打开id_rsa.pub，拷贝全部内容

登录Github账号 -> Settings  -> Personal setting  -> SSH and GPG keys -> New SSH key

* 测试SSH key

```sh
ssh -T git@github.com
```


```sh
# ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? 

# yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Enter passphrase for key '/root/.ssh/id_rsa': 
Hi XXX! You've successfully authenticated, but GitHub does not provide shell access.
```

仓库初始化`git init`

查看文件的状态`git status`

将工作区的文件保存到暂缓区`git add`

README.md`git add Git.md`

将暂缓区的文件提交到当前分支`git commit`

`git remote add origin https://github.com/user\_name/Mytest.git>`

`git push origin master`

### 取消文件版本控制

[git取消跟踪已版本控制的文件](https://www.cnblogs.com/xxoome/p/9186155.html)

[git rm与git rm —cached - 向着太阳生 - 博客园](https://www.cnblogs.com/toward-the-sun/p/6599656.html)

不再追踪文件改动 `git update-index —assume-unchanged filePath`

恢复追踪文件改动 `git update-index —no-assume-unchanged filePath`

删除暂存区或分支上文件, 同时删除本地文件

```git
git rm file_path
git commit -m ‘delete somefile’
git push
```

删除暂存区或分支上的文件, 保留本地文件

```git
git rm —cached file_path
git rm -r --cached .ipynb_checkpoints
Git commit -m ‘delete remote somefile’
git push
```

### 其它

`git help`

`git config user.name ""`

`git config user.email ""`

查看配置信息命令`git config -l`

[Git清理删除历史提交文件](https://www.jianshu.com/p/7ace3767986a)

[如何解决 GitHub 提交次数过多 .git 文件过大的问题？](https://www.zhihu.com/question/29769130)

垃圾回收`git gc --prune=now`

## commit

### [如何优雅的 git commit](gitCommit-learn.md)

[Developing AngularJS Git Commit Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)


### Conventional Commits
[约定式提交 1.0.0-beta.4](https://www.conventionalcommits.org/zh-hans/v1.0.0-beta.4/)

> 约定式提交规范是一种基于提交消息的轻量级约定。 它提供了一组用于创建清晰的提交历史的简单规则； 这使得编写基于规范的自动化工具变得更容易。 这个约定与 [SemVer](http://semver.org/) 相吻合， 在提交信息中描述新特性、bug 修复和破坏性变更。

[Git commit message 规范](https://zhuanlan.zhihu.com/p/69989048)

> 现在市面上比较流行的方案是`约定式提交规范`（`Conventional Commits`），它受到了`Angular提交准则`的启发，并在很大程度上以其为依据。`约定式提交规范`是一种基于提交消息的轻量级约定。 它提供了一组用于创建清晰的提交历史的简单规则；这使得编写基于规范的自动化工具变得更容易。这个约定与`SemVer`相吻合，在提交信息中描述新特性、bug 修复和破坏性变更。

message 格式如下:
```text
<类型>[可选的作用域]: <描述>
[可选的正文]
[可选的脚注]
```
```text
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

1. type

`type`为必填项，用于指定commit的类型，约定了`feat`、`fix`两个`主要type`，以及docs、style、build、refactor、revert五个`特殊type`，`其余type`暂不使用。

2. scope

`scope`也为必填项，用于描述改动的范围，格式为项目名/模块名，例如： `node-pc/common` `rrd-h5/activity`，而`we-sdk`不需指定模块名。如果一次commit修改多个模块，建议拆分成多次commit，以便更好追踪和维护。

3. body

`body`填写详细描述，主要描述`改动之前的情况`及`修改动机`，对于小的修改不作要求，但是重大需求、更新等必须添加body来作说明。

4. break changes

`break changes`指明是否产生了破坏性修改，涉及break changes的改动必须指明该项，类似版本升级、接口参数减少、接口删除、迁移等。

5. affect issues

`affect issues`指明是否影响了某个问题。例如我们使用jira时，我们在`commit message`中可以填写其影响的`JIRA_ID`，若要开启该功能需要先打通`jira`与`gitlab`。参考文档：[https://docs.gitlab.com/ee/user/project/integrations/jira.html](https://link.zhihu.com/?target=https%3A//docs.gitlab.com/ee/user/project/integrations/jira.html)

填写方式例如：

```text
re #JIRA_ID
fix #JIRA_ID
```

### commit失效问题

[更改远程仓库的 URL - GitHub 帮助](https://help.github.com/cn/github/using-git/changing-a-remotes-url)

[备份本地git库到GitHub时在能够ssh成功但是无法push的错误的解决_运维_vslyu的博客-CSDN博客](https://blog.csdn.net/vslyu/article/details/80356939)

```
(base) root@iZ5gchbe06niz0Z:~/lab/Chaos# git remote -v
origin  git@github.com:zhouqiaor/Chaos.git (fetch)
origin  git@github.com:zhouqiaor/Chaos.git (push)
```

将目录更改为存储库的本地克隆（如果尚未存在），然后运行：

```
git remote set-url origin git@github.com:username/your-repository.git
```

## push 提交

[将提交推送到远程存储库](https://help.github.com/en/github/using-git/pushing-commits-to-a-remote-repository)

使用`git push`在您的本地分支的到远程仓库推送的提交。

该`git push`命令带有两个参数：

- 远程名称，例如， `origin`
- 分支名称，例如， `master`

例如：

```shell
git push  <REMOTENAME> <BRANCHNAME> 
```

`git push origin master`将本地更改推送到在线存储库中

## branch 分支

### [重命名分支](https://help.github.com/en/github/using-git/pushing-commits-to-a-remote-repository#renaming-branches)

要重命名分支，您将使用相同的`git push`命令，但是您将添加另一个参数：新分支的名称。例如：

```shell
git push  <REMOTENAME> <LOCALBRANCHNAME>:<REMOTEBRANCHNAME> 
```

这会将推`LOCALBRANCHNAME`送到您的帐户`REMOTENAME`，但已重命名为`REMOTEBRANCHNAME`。

```sh
# 查看当前分支
git branch

# 新建分支并提交本地修改
git push origin master:my_remote_new_branch
```

### [推送标签](https://help.github.com/en/github/using-git/pushing-commits-to-a-remote-repository#pushing-tags)

默认情况下，没有其他参数，`git push`将发送所有与远程分支同名的匹配分支。

要推送单个标签，您可以发出与推送分支相同的命令：

```shell
git push  <REMOTENAME> <TAGNAME> 
```

要推送所有标签，可以键入以下命令：

```shell
git push  <REMOTENAME> --tags
```

### [删除远程分支或标签](https://help.github.com/en/github/using-git/pushing-commits-to-a-remote-repository#deleting-a-remote-branch-or-tag)

乍一看，删除分支的语法有点奥秘：

```shell
git push  <REMOTENAME> :<BRANCHNAME> 
```

请注意，冒号前面有一个空格。该命令类似于您重命名分支的相同步骤。然而，在这里，你告诉Git的推*没有* 进`BRANCHNAME`的`REMOTENAME`。因此，`git push`删除远程存储库上的分支。