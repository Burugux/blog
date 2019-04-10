---
title: "Cdpath + gopath"
date: 2019-04-10T20:22:36+03:00
draft: false
description: "How to use cdpath to your advantage while navigating the gopath."
author: "Leslie Burugu"
tags: ["go","shell"]
---
If you've ever used [Go](http://golang.org) I'm pretty sure you have heard of the term **GOPATH**. It refers to the internal directory structure that the Go toolchain uses to find source code and packages.

If i have a project called **weather** and i wanted to `cd` into it, i would have to do something like this (assuming my **$GOPATH** is set to **$HOME/go**).

```
cd $HOME/go/src/github.com/burugux/weather/
```

This works fine but if **weather** is a directory you visit frequently typing all that becomes boring,super repetitive and also a little bit annoying.

But wouldn't it be cool if we could just **cd weather** and we would be able to jump to that directory directly?

### Introdcuing CDPATH

It turns out we can actually do that with the help of **CDPATH**. [Learning the bash shell, 3rd eidition](http://shop.oreilly.com/product/9780596009656.do) defines CDPATH as
>The variable whose value, like that of **PATH**, is a list of directories
separated by colons. Its purpose is to augment the functionality of the cd built-in
command.

By default **CDPATH** is not set. To set it up so that we can jump to our Go projects faster we would do something like this

```
export CDPATH=$HOME/go/src/github.com/$USER/
```

So what does the above command do? Well it assigns a single value to the **CDPATH** variable which is the location we would like `cd` to use while looking for the directory we are interested in. With that exported if we typed **cd weather** we would automatically go to

```
$HOME/go/src/github.com/$USER/weather
```

How cool right?😃

However please note that
>The above would only work if your github username and **$USER** are the same. If not, all you have to do is replace **$USER** with your actually github username.

To make it persitent add the export statement to your **.bashrc** or **.zshrc**.

### Adding more options
Let's imagine a scenerio where you have multiple projects in your **GOPATH** which you contribute to and some of them are not under your github username as is usually the case. Using what we have learnt above is it still be possible to jump to those projects quickly without typing the entire path? Yes.

To do that we would pass a second value to **CDPATH** separated by a colon. For example

```
export CDPATH=$HOME/go/src/github.com/burugux:$HOME/go/src/github.com/
```

With that out of the way we would be able to do something like

```
cd jessfraz/weather
```

And automatically go to

```
$HOME/go/src/github.com/jessfraz/weather
```

Neat! 😁

If we had projects from github and gitlab can we still use **CDPATH**? Again yes. All we have to do is pass a third value to `CDPATH` and it would look something like this.

```
export CDPATH=$HOME/go/src/github.com/burugux:$HOME/go/src/github.com:$HOME/go/src/gitlab.com/
```

With that i can now do

```
cd leslie/cool-project
```

and automatically go to

```
$HOME/go/src/gitlab.com/leslie/cool-project
```

### One caveat to note

If i had a directory **$HOME/weather** and i typed **cd weather** while i was at **$HOME** where would it take me?

```
./weather
```

or

```
$HOME/go/src/github.com/burugux/weather
```

> `./weather` unfourtanely. From [softparanoma](http://www.softpanorama.org/Scripting/Shellorama/cdpath.shtml),
If you put a trailing slash "/" on each path in `$CDPATH`, you'll "cd" to the local directory. If you don't put a trailing slash on each pathname, you'll "cd" to the first pathname in the `$CDPATH` list that contains a matching subdirectory.

To `cd` into the local directory i would have to do `cd ./weather` but from anywhere else i would still be able to do `cd weather` and go to

```
$HOME/go/src/github.com/burugux/weather
```

I hope you have learnt something new and are going to use `CDPATH` to make your life easier while using the terminal.

Thank you for reading!