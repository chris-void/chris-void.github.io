---
layout: post
title: About Git and Github 
categories: 
- git
- workflow
tags:
- linux
---

## Git tricks

**give up changes and go back**
```sh
git checkout -- .
```


### Configure remote
```sh
git remote -v
git remote add upstream xxx.gitB
```

### Sync a fork
```sh
git fetch upstream
git checkout master
git merge upstream/master
```

### Use PR
```sh
```




## reference 
[Configure remote](https://help.github.com/articles/configuring-a-remote-for-a-fork/)   
[Sync fork](https://help.github.com/articles/syncing-a-fork/)   

[Use PR](https://help.github.com/articles/using-pull-requests/)
