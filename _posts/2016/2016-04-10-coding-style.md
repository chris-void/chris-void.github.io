---
layout: post
title: Coding Style
categories: 
- coding
tags:
- coding-style
---

<!--# Linux Coding Style-->


# 15-410 Coding Style

## two types of comments

1. comments at the beginning of each file which describes the file and lists things like author and known bugs.
2. comments are those that describe your functions and data structures. We want you to put comments on all your functions and data structures.

## File comments

```
@file
@brief
@author
@bug
```

## Comments for Functions and Data Structures

```
@brief
@param
@return
```

**in the non-brief section of the function:**

- Anything a user needs to know to decide whether this is the right function for them to use for a given job.
- Usage preconditions: must be called with interrupts disabled, etc.
- Any use of an unusual algorithm (in which case, cite it -- academic format or page title and URL)
- Why the code was written in a non-obvious structure.
- Warnings about how to extend the function without breaking it.


## Coding Style / Coding Standards

### Modularity

The scheduler functions are in a file full of scheduler functions and nothing else; each function is coded up once; etc.

### Documentation

This can be read at multiple levels. For example, people can read a short file to find out if your program is right for them; can read about a module or package to see if they want to steal it for their own programs; can read about a function to decide whether they need to read the code, etc.

### Maintainability

Including basic things such as essentially never putting a hardwired constant such as 1128 into a file, and never using it twice, and never ever putting 1127 into your code because it's 1128 - 1. But you knew all that from 15-213, right? Since you asked, the right way to handle 1128 would be 
```(GREY << COLOR_SHIFT) | (ELEPHANT << ANIMAL_SHIFT) | 16 /* tons */ ```

### Dead Code

There are two very different kinds of "dead code" which students tend to turn in. One kind is 'debug code' which was briefly useful but never will be again, stuff like 
```     // printf("!!!! joe = %d\n", 4 * i / 0); ```
or
```        int threadstatus = THR_RUNNING; // THR_RUNNING|128 
```
or
```     // if (threadid ==17) MAGIC_BREAK; ```

Realistically, even if something like that helped you track down a bug once, you wouldn't re-activate it to find the next one. Since this code will never be run again, leaving it around can't do anything except distract and slow down your audience. Don't turn it in. 
In the other direction, occasional assert()s are fine. In fact, sensibly done, they actually help document the code. See the section on asserts. 
Overall, if debug code is genuinely likely to be useful to readers or maintainers, that argues in favor of keeping it, but the vast majority of what gets stuck in temporarily is just noise, and should be deleted. 

### Asserts

Asserts are statements that check that the assumptions that were made in your program are not violated. For example, let's say you have a variable *nodeptr. If, by design, nodeptr can never point to NULL, then a statement like 
```ASSERT (nodeptr);
```
or, in the Linux idiom, 
```if (!nodeptr)
    BUG();
```

is a way of verifying that this assumption is not violated. This is useful in two ways. Firstly, it documents your assumption. Secondly, in future, if anyone modifying your code violates this assumption, (s)he is informed of this early, rather than having to discover this through a debugging session. Hardcore performance evangelists will argue that such code will almost never be executed, and that the extra conditional statement just serves to affect the branch prediction accuracy. People with such concerns should look at the ```likely()``` and ```unlikely()``` preprocessor macros in the Linux kernel and the associated gcc builtin function ```__builtin_expect()```. However, people who don't have measurable performance problems should probably avoid littering their code with cryptic incantations without actual benefit.




## reference 
[Linux Coding Style]()
[15-410 Coding Style and Doxygen Documentation](https://www.cs.cmu.edu/~410/doc/doxygen.html)
[OpenStack coding style](http://docs.openstack.org/developer/hacking/)
[Doxygen](https://www.stack.nl/~dimitri/doxygen/manual/index.html)
