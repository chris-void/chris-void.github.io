---
layout: post
title: Thinking in C
categories: 
- Programming 
tags:
- program language
--


### 题外话 
*先说点题外话，之所以想写这篇blog也是因为我现在在看的Jon Erickson的Hacking The Art of Exploitation。看过之后对C Programming有了完全不一样的看法，其实随着后来接触的Programming Language越来越多，自己对Programming的认识也越来越深入，接触了函数式的Haskell，脚本语言的Python Ruby，发现对C、C++又有了全新的认识。 If you think you know C Programming pretty well, strongly recommand you the book written by Jon Erickson. *

## C Programming Language

本来想着写写对C的新认识，但是后来发现现在流传的C的书籍太多了，所以还是分享一点自己的对于C的总结，这样读者（话说我还不知道读者会是谁...或者会不会有读者）可以自己有针对性的去看书。

### C Outline

C这门语言博大精深，本深就是计算机发展成熟之后给科研人员和Prof使用的语言，因此对于初学者来说就像一把双刃剑，一方面是Powerful，另一方面是Creating Vulnerbilities without acknowledge。强调一下，C这门语言的设计思想就是使用者比设计者更知道自己要做什么，因此很多的地方需要自己去注意。

简单总结一下应该掌握的内容:

先从header入手

对于<stdio.h> 函数`printf` `scanf`的用法
 
对于<stdlib.h> 函数

对于<string.h> 函数

对于<math.h>



*To Be Continued*



 need some explaination how this specific line works. I know that this function counts the number of 1's bits, but how exactly this line clears the rightmost 1 bit?

int f(int n) {
    int c;
    for (c = 0; n != 0; ++c) 
        n = n & (n - 1);
    return c;
}
Can some explain it to me briefly or give some "proof"?

Thanks in advance, BBLN.

Any number 'n' will have the following last k digits: One followed by (k-1) zeroes: 100...0 Note that k can be 1 in which case there are no zeroes.

(n - 1) will be of this format: Zero followed by (k-1) 1's: 011...1

n & (n-1) will therefore be 'k' zeroes: 100...0 & 011...1 = 000...0

Hence n & (n - 1) will eliminate the rightmost '1'. Each iteration of this will basically remove the rightmost '1' digit and hence you can count the number of 1's.



---


up vote
17
down vote
accepted
It's figuring out if n is either 0 or an exact power of two.

It works because a binary power of two is of the form 1000...000 and subtracting one will give you 111...111. Then, when you AND those together, you get zero, such as with:

   1000 0000 0000 0000
&&  111 1111 1111 1111
   ==== ==== ==== ====
 = 0000 0000 0000 0000
Any non-power-of-two input value will not give you zero when you perform that operation.

For example, let's try all the 3-bit combinations:

      <----- binary ---->
 n     n    n-1   n&(n-1)
---   ---   ---   -------
 0    000   111     000 *
 1    001   000     000 *
 2    010   001     000 *
 3    011   010     010
 4    100   011     000 *
 5    101   100     100
 6    110   101     100
 7    111   110     110
You can see that only 0 and the powers of two (1, 2 and 4) result in a 000/false bit pattern, all others are non-zero or true.

See the full Bit Twiddling Hacks document for all sorts of other wonderful (or dastardly, depending on your viewpoint) hackery.

