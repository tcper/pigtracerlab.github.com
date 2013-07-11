---
layout: post
title: "Git rebase概念"
description: "详细解释git rebase概念"
category: 
tags: [git, rebase]
---
{% include JB/setup %}

##详细解释git rebase概念

其实对于大部分需求来说，使用git几个基本命令已经可以了，git有很多神奇的功能，我介绍一下rebase命令。

rebase命令是基于patch的，

- ```branch1 ```从master分支切出，并做了改变
- 一段时间之后```branch1```要和```master```合并
- 但此时```master```的```HEAD```和```branch1```切出时不一样了
- 当然可以通过```git merge master```合并代码
- 也可以通过```git rebase master```合并

二者之间的区别在于，

- ```merge```通过合并分支合并branch，并生成一个新commit
- ```rebase```通过patch方式，能够得到一个稍微```干净```的commit log

注意一点：

- ```rebase```会将执行命令的branch修改置于后面，即rebase当前branch的commit会最靠近HEAD

###rebase另外一个神奇的功能

- ```branch1```从```master```切出作出改变
- ```branch2```从```branch1```切出，基于```branch1```作出了改变
- ```branch2```的修改可以merge到master上了，但是```branch1```还没ready
- ```git rebase --onto master branch1 branch2```

这条命令的作用就是将```branch2```剥离```branch1```的commit，直接提交到master上。

这条命令的本质，是将```branch2```和```master```做一个patch，然后apply到master上。等branch1 ready之后，再rebase到master上。

整个工作流就完成了。

###rebase的黑坑

- rebase命令的本质是patch，也就是说，使用rebase就是将放弃以前的commit，git会更新已有commit的hash，如果你把已经发布到public repo之上的commit做了rebase，那么别的开发者将会pull下来两个看起来一样的commit，因为log, date都是一样的，但是hash不同，git将这些当作不同的commit，给别人造成困惑。
- *所以一定不要在public repo上使用rebase*.

最后祝大家rebase用得愉快.



