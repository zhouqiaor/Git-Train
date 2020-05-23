# GitTrain 

https://github.com/HerbertHe/GitCommitTraining



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



## [如何优雅的 git commit](gitCommit-learn.md)

[Developing AngularJS Git Commit Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)


## Conventional Commits
[Git commit message 规范](https://zhuanlan.zhihu.com/p/69989048)

[约定式提交 1.0.0-beta.4](https://www.conventionalcommits.org/zh-hans/v1.0.0-beta.4/)

> 约定式提交规范是一种基于提交消息的轻量级约定。 它提供了一组用于创建清晰的提交历史的简单规则； 这使得编写基于规范的自动化工具变得更容易。 这个约定与 [SemVer](http://semver.org/) 相吻合， 在提交信息中描述新特性、bug 修复和破坏性变更。

> 现在市面上比较流行的方案是`约定式提交规范`（`Conventional Commits`），它受到了`Angular提交准则`的启发，并在很大程度上以其为依据。`约定式提交规范`是一种基于提交消息的轻量级约定。 它提供了一组用于创建清晰的提交历史的简单规则；这使得编写基于规范的自动化工具变得更容易。这个约定与`SemVer`相吻合，在提交信息中描述新特性、bug 修复和破坏性变更。
>

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

### type

`type`为必填项，用于指定commit的类型，约定了`feat`、`fix`两个`主要type`，以及docs、style、build、refactor、revert五个`特殊type`，`其余type`暂不使用。

```text
# 主要type
feat:     增加新功能
fix:      修复bug

# 特殊type
docs:     只改动了文档相关的内容
style:    不影响代码含义的改动，例如去掉空格、改变缩进、增删分号
build:    构造工具的或者外部依赖的改动，例如webpack，npm
refactor: 代码重构时使用
revert:   执行git revert打印的message

# 暂不使用type
test:     添加测试或者修改现有测试
perf:     提高性能的改动
ci:       与CI（持续集成服务）有关的改动
chore:    不修改src或者test的其余修改，例如构建过程或辅助工具的变动
```

### scope

`scope`也为必填项，用于描述改动的范围，格式为项目名/模块名，例如： `node-pc/common` `rrd-h5/activity`，而`we-sdk`不需指定模块名。如果一次commit修改多个模块，建议拆分成多次commit，以便更好追踪和维护。

### body

`body`填写详细描述，主要描述`改动之前的情况`及`修改动机`，对于小的修改不作要求，但是重大需求、更新等必须添加body来作说明。

### break changes

`break changes`指明是否产生了破坏性修改，涉及break changes的改动必须指明该项，类似版本升级、接口参数减少、接口删除、迁移等。

### affect issues

`affect issues`指明是否影响了某个问题。例如我们使用jira时，我们在`commit message`中可以填写其影响的`JIRA_ID`，若要开启该功能需要先打通`jira`与`gitlab`。参考文档：[https://docs.gitlab.com/ee/user/project/integrations/jira.html](https://link.zhihu.com/?target=https%3A//docs.gitlab.com/ee/user/project/integrations/jira.html)

填写方式例如：

```text
re #JIRA_ID
fix #JIRA_ID
```