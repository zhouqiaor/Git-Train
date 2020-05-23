# 如何优雅的 git commit

https://github.com/Linyccc/gitCommit-learn



由于不规范的 commit message，在 git log 和 code review 时显得很无用，同时也不利于编写 changelog。这里推荐采用 [Angular 团队的规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)。这是目前使用最广的写法，比较合理和系统化，并且有配套的工具。

下面从`模板`和`配套工具`两个方面介绍，如何优雅的处理 commit

## 一、commit message 模板

每次 commit 由 header，body，footer 三部分组成（横线分割）:

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

其中只有 header 是必须要有的，body 和 footer 可以省略。

### 1.1 header

`type`

必须是以下之一，表明当前修改的类型

- feat: 新功能
- fix: Bug 修复
- docs: 文档更新
- style: 代码的格式，标点符号的更新
- refactor: 代码重构,即非修复 bug，也非增加 feature
- perf 性能优化
- test 增加或修改测试用例
- build 构建系统或者包依赖更新
- ci CI 配置，脚本文件等更新
- chore 非 src 或者 测试文件的更新
- revert commit 回退

ssssasdsds

`scope`

scope 是 header 里的可选项，可以以任意名词指明 commit 影响的范围，比如：XX 组件、xx 模块、xx 文件。如果超过一种，可以用 `*` 表示

`subject`

是对修改作出的简洁描述，不超过 70 字.

> 以动词开头，比如：修改，增加，删除等
> 全部小写(.)
> 结尾不加句号 (.)

### 1.2 body

是对修改作出的详细描述。可以分成多行。

> 应该说明代码变动的动机，以及与以前行为的对比

### 1.3 footer

Footer 只用于两种情况:

一种是不兼容变动 （Breaking Changes）

如果当前代码与上一个版本不兼容，则 Footer 部分以 BREAKING CHANGE 开头，后面是对变动的描述、以及变动理由和迁移方法。

```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

另一种是指明关闭某个 issue （Referencing issues）

如果当前 commit 针对某个 issue，那么可以在 Footer 部分关闭这个 issue 。

```
Closes #234
```

也可以一次关闭多个 issue 。

```
Closes #123, #245, #992
```

### 1.4 特殊情况 revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以 revert:开头，后面跟着被撤销 Commit 的 Header。

```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

> Body 部分的格式是固定的，必须写成 This reverts commit &lt;hash>.，其中的 hash 是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的 Reverts 小标题下面。

## 二、自动化工具

以上是对模板内容的介绍，只是书面的约定。人肉执行起来难免会有遗漏或错误，所以还必须要有一套自动化工具来保证。

### 2.1 message 向导

下面我们需要引入向导工具：**[commitizen](https://github.com/commitizen/cz-cli)** 和 **[cz-conventional-changelog](https://github.com/commitizen/cz-conventional-changelog)**

以项目级安装为例：

```bash
yarn add commitizen cz-conventional-changelog --dev
```

以后，凡是用到`git commit`命令，一律改为使用`git cz`。这时，就会出现选项，用来生成符合格式的 Commit message

![git-cz](./screenshots/git-cz.jpg)

**commitizen**，是一款标准化 git commit 信息的工具

**cz-conventional-changelog**，是一个符合 Angular 团队规范的 preset。使得 commitizen 按照我们指定的规范帮助我们生成 commit message.

如果是全局安装，必须在项目根目录添加`.czrc`配置文件，内容如下：

```javascript
{ "path": "cz-conventional-changelog" }
```

如果是项目级安装，需要在 package.json 的 config 字段配置 path

```javascript
{
  "config": {
    "commitizen": {
      "path": "node_modules/cz-conventional-changelog"
    }
  },
}
```

以上两种配置方式，效果相同

### 2.2 校验 message

首先，安装校验器和校验规范

```bash
yarn add @commitlint/config-conventional @commitlint/cli --dev
```

同时需要在项目目录下创建配置文件 .commitlintrc.js, 写入:

```javascript
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {}
};
```

[@commitlint/cli](https://github.com/conventional-changelog/commitlint) 可以帮助我们 lint commit messages。

同样的, 它也需要一份校验的配置, 这里推荐@commitlint/config-conventional(符合 Angular 团队规范).

其次，安装 [Husky](https://github.com/typicode/husky)帮我们在 commit 时自动校验 message，如果我们提交的不符合指向的规范, 直接拒绝提交。

```bash
yarn add husky --dev
```

package.json 添加 husky 自动，配置如下：

```javascript
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
}
```

### 2.3 导出 changelog

**[conventional-changelog-cli](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli)** 是[conventional-changelog](https://github.com/conventional-changelog)生态圈的核心命令行工具。它可以帮我们根据项目的 commit 和 metadata 信息自动生成 changelog

以项目级安装为例：

```
yarn add conventional-changelog-cli --dev
```

然后执行就可以生成一个 CHANGELOG 文档

```
conventional-changelog -p angular -i CHANGELOG.md -s
```

参数说明：

- `-p` 指定使用的 commit message 标准，如：atom, codemirror, ember, eslint, express, jquery
- `-i` -i CHANGELOG.md 表示从 CHANGELOG.md 读取 changelog
- `-s` -s 表示读写 changelog 为同一文件

上面这条命令产生的 changelog 是基于上次 tag 版本之后的变更（Feature、Fix、Breaking Changes 等等）所产生的，所以如果你想生成之前所有 commit 信息产生的 changelog 则需要使用这条命令：

```
conventional-changelog -p angular -i CHANGELOG.md -s -r 0
```

其中 -r 表示生成 changelog 所需要使用的 release 版本数量，默认为 1，全部则是 0。

更多参数见 `conventional-changelog --help`

## 参考文档

- [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
- [git commit 、CHANGELOG 和版本发布的标准自动化](https://zhuanlan.zhihu.com/p/51894196)
- [优雅的提交你的 Git Commit Message](https://zhuanlan.zhihu.com/p/34223150?from_voters_page=true)
- [使用 husky 避免糟糕的 git commit](https://zhuanlan.zhihu.com/p/35913229)
