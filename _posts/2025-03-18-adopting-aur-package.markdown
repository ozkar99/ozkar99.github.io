---
layout: post
title: "adopting an AUR packager"
date: 2025-03-18
comments: true
---

<p class="intro">
Today i ended up adoptin an orphaned package in the AUR, https://aur.archlinux.org/packages/python-gruut 
</p>

The AUR is archlinux user repository, one of arch coolest features, its similar to [BSD ports](https://aur.archlinux.org/).

(its maybe not a super well known fact but archlinux is my favorite linux dristro due to its BSD-like qualities).

As part of a stealth project i've been working on (pertaining AI, which means python), 
ive run into an issue where a python dependency relied on a package with a broken build since 2023.

Thats a whole 2 years where a package has been broken, 2 people have commented on it and it has remained orphaned for a while, 
the root cause is a simple typo on of the dependencies.

I dont particularly enjoy python (i am more of a rubysta) but i thought this was kinda silly.

I figured, what the hell and adopted (and fixed) the package, the process was incredibly simple:

1. Open a AUR account (+ add ssh key for auth)
1. git clone the package
1. Make appropiate changes to fix
1. git push
1. ????
1. profit

Now my stealth project builds on archlinux with no pip packages breaking my clean installation.

In fact, this post took longer to write than it was to adopt the package and fix it.

Heres the diff for the fix: https://aur.archlinux.org/cgit/aur.git/commit/?h=python-gruut&id=4f50985c01633f8628c8211462118f85aa7f9f4b 

```
@@ -14,7 +14,7 @@ pkgbase = python-gruut
 	depends = python-networkx
 	depends = python-num2words
 	depends = python-numpy
-	depends = python-python-crfsuite
+	depends = python-crfsuite
 	source = PKGBUILD_EXTRAS
 	source = https://files.pythonhosted.org/packages/4c/74/40e0bff02cf4daa3908c440e2111b20490c82080259f0114d0cfe07ce126/gruut-2.3.4.tar.gz
 	source = LICENSE
```
