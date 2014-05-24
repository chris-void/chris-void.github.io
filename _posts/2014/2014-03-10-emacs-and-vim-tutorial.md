---
layout: post
title: Thoughts of Vim and Emacs
categories: 
- Shell
tags:
- emacs
- vim
---

## Emacs and Vim
 
----

第一次接触编辑器还是因为使用linux系统,当时在同学的帮助下安装了Ubuntu,从此开始了在linux这片大海里面的狗刨. 如果对Unix/Linux有过一定了解就知道(为了方便起见将Unix化到Linux里面吧,毕竟Linux作为个人开发平台还是使用比较广泛),Linux是文件系统,里面所有的存储的都是文件,而不像Windows这样的操作系统,里面的程序等等都是二进制文件.文本文件的好处是容易修改 管理,因此必须要有一个编辑的编辑器,Unix/Linux都统一为了Vi,一般任何一个裸的安装好的Linux系统一定包含Vi.

Vim,是Vi的改进版,包含了更多的功能,更适合个人用户日常的使用.这里推荐CoolShell的[Vim练级攻略](http://coolshell.cn/articles/5426.html),很好的教程.

Emacs是另一种类型的编辑器,是使用一种古老的语言->Lisp写得. 与Vim相比,Emacs本身具有的功能更多,因而个人定制性更高,我就是因为Emacs的高度自由扩展功能抛弃了Vim= =  Unix的发展带来的一大影响就是让它的理念-使用者永远比创造者更知道自己要干什么-产生了很深远的影响,Vim/Emacs也是一样的,本身的程序是很简单的,真正开发出适合自己使用的Vim/Emacs是要靠自己的设置,Vim是在~/.vimrc修改 Emacs是在~/.emacs


### Why should we use Emacs/Vim even we already have TextMate/Sublime?

可以说随着计算机的发展,好用的图形化的编辑器愈来愈多,TextMate/Sublime都是非常好的例子.如果是新接触Linux等等的朋友到是可以从这里开始入手.[Sublime官网](http://www.sublimetext.com/) [TextMate](http://macromates.com/) 不过我上次看的时候TextMate还只是开源,对于非OS X的系统的支持可能不是特别理想. 

这两种编辑器的使用也非常简单,Sublime通过package control来控制插件的下载安装与卸载,TextMate是Bundles.特别是Sublime,跨平台的支持非常棒,Mac/Linux/Windows下都是我装好系统第一个安装的(Server版的就不说了,只能用Vi = =), 因为很多UTF-8和GTK编码的问题很蛋疼...

Sublime安装:

首先点击[Sublime](http://www.sublimetext.com/)选择自己需要的版本(dev代表经常提示更新). 如果是Windows........(没有Linux还来看这个确定不是逗我?!)

```python
cd ~/Downloads
sudo mv ~/Downloads/SublimXXXX /opt
cd /opt
tar -xf SublimeXXXX
cd SublimeXXXXXX 
./sublime
```
*说明:XXX是由于每个版本都不太一样,因此使用tab提示就可以了*

安装package control(必备): 

```code
CTRL+`

//Sublime Text2
import urllib2,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')

//Sublime Text3
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

使用入门:


```
//配置
{
"ignored_packages":
    [
        "Vintage"
    ],
    "theme": "Soda Dark.sublime-theme",
    "font_size": 12
    
}
```


可能有人会问:既然已经有了这些界面功能都完美的编辑器,为什么我们还要学习使用Vim/Emacs这种老古董呢?

我个人觉得有两个原因:
	
* 在服务器的领域,基本上Windows已经失败了,发展的方向肯定是Unix内核的操作系统,和Vi打交道是不可能回避的事情. 
* 好用的适合自己的编辑器可以大大提高工作的效率,而追求以键盘的输入取代鼠标作用的Vim/Emacs必然给使用者带来更好地工作体验.


### How to Use Vim

前面已经给出了CoolShell的[Vim实战教程](http://coolshell.cn/articles/5426.html),我认为已经写的很好了,因此这里不再赘述. 值得注意的一点是Vim和Emacs方面的代码提示做的不是很好,因此涉及网页/C#等等编程的内容必然需要比较复杂的配置,还是使用Sublime比较方便

安装vim:

```
//Ubuntu等Debian内核
sudo apt-get install vim

//Arch Linux
sudo pacman -Sya vim / sudo yaourt vim 

//Fedora 
sudo yum -y vim

//Mac OS X (已安装了Homebrew)
sudo brew install vim 
```

使用操作入门:

```
i/o      可从阅读模式进入编辑模式
 
ESC      退出编辑模式

ESC :w   对当前的编辑进行保存

ESC :q   退出 (一般不使用这条命令而是使用下面两条)

ESC :wq  保存当前内容并退出

ESC :q!  强制退出(放弃更改,如果是新建了new file则放弃创建文件)
```


### Next: Use Emacs

Emacs的使用比Vim更复杂,可以说我目前也是只使用了一部分功能,但是就是这样已经足以让我放弃Vim了. [GNU Emacs官网](http://www.gnu.org/software/emacs/),具体教程建议去[Emacs中文网](http://emacser.com/)查看.

安装Emacs:

```
//Ubuntu等Debian内核
sudo apt-get install emacs
 
//Arch Linux
sudo pacman -Syu emacs / sudo yaourt emacs
 
//Fedora
sudo yum -y emacs
 
//Mac OS X (已安装了Homebrew)
sudo brew install emacs
```

使用操作入门:

```


```



