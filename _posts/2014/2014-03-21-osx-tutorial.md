---
layout: post
title: OSX Tutorial
categories:
- OSX
tags:
- mac
- apache
---


## Introducing to Some Useful Tools


### Homebrew 

介绍: Homebrew的作用相当于Ubuntu/Debian下的apt-get, Arch下的pacman / yaourt, Fedora下的yum

配置:

使用:
```
//使用homebrew安装xxx
brew install xxx 
//卸载之前安装的xxx
brew remove xxx

//下面两条相当于apt-get下的upgrade和update,定时更新就好
brew update
brew uograde
```


### Pip

介绍: 是比较有用的Python开发工具, 

配置:

使用:
```
```

### Gem

是ruby安装工具,很有用
配置:
```
//必要时可能需要更改src地址,方校长万岁!

```

使用:
```

```

### TextMate

是OS X下很有用**编辑器**, 其实使用起来功能和Sublime是类似的,不过在Mac下面的集成和配置做的比较好

配置&安装:
```
//我记得是不能直接安装的,必须手动下载并且安装
```


### Sublime 

个人强烈推荐的一个编辑器, 最主要原因是sublime跨平台并且每个OS下面都做得非常好, 具有很好的集成性和自由组合度, 使用package control可以自己搭建适合自己的编辑器环境

配置&安装:
```
//sublime3 PKG control        
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
//sublime2 PKG control        
import urllib2,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')

//手动下载

//需要设置User

//配置     
{
    "ignored_packages":
    [
        "Vintage"
    ],
    "theme": "Soda Dark.sublime-theme",
    "font_size": 12,
    "font_face": "YaHei Consolas Hybrid"
}
```

[ SublimeClang插件问题](http://106201.html.blog.chinaunix.net/uid-28894229-id-3839961.html)


## Some Other Tools

### Xcode

Apple自己的IDE,支持多种语言,最重要的是做iOS开发必备...


### Evernote

记笔记必用,很好用很方便

### VMware Fusion

安装虚拟机必备,我就装了Kali, Ubuntu LTS, WinXP
