---
layout: post
title:  "How cli history can help you in efficiency"
date: 2017-05-12 10:20:44
categories: commandline tools efficiency
published: true
---

This is a small trick that I find useful while working on my projects.
We often have to write long and repetitive commands in our terminal.
Sometimes we know we have used some command before, but it's out of our mind and we are starting to look for it in stackoverflow etc.
This is where command line history comes handy. Lets take git for example:

```
git add .
git commit -m 'another commit foo, foo, foo'
git push
```

Pretty long right. But we can merge it to one long line, right?
```
git add . && git commit -m 'another commit foo, foo, foo' && git push
```

Yeah obvious. But typing it again and again is tedious. So if you wrote it
before you can retrieve it from memory with Ctrl+r (unix) or Command + Option + P + R (mac).
But what if you have used it in the previous session and after reopening
the terminal your history is gone?
Well, adjust your history to infinite.

In .bashrc file edit the history size:
```
# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=-1
HISTFILESIZE=-1
```

Why -1? Because it will be treated as infinite.

So now the only trick will be to remember part of the command you want to use and go through your endless history to find it. That shouldn't be
difficult, right?

