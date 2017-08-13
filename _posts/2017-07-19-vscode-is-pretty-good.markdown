---
layout: post
title:  "vscode is a pretty cool guy"
date:   2017-07-20
comments: true
---

<p class="intro"><span class="dropcap">I</span> think vscode is often an underlooked option when choosing an editor</p>  
After using atom for a long time i made the switch to vscode and i think more people should give it a chance.  
In this post i will ilustrate some of the benefits of using vscode as your go-to editor.  

## Its multi-platform
So it runs on windows, macos and linux, and the builds are quite good on each platform, ive noted no discrepancy between platforms in my use.  

## Its open source
The code is all [here.](https://github.com/Microsoft/vscode)  
And thats a pretty cool thing.  

## Its pretty light
For something made in electron, its actually quite fast.  

## Debugging is a first class citizen
Thats right, putting breakpoints, hitting them and inspecting variables works out of the box on most languages.  
This is something that no other editor can claim to have.  
So far ive had satisfactory results with go, ruby and js (node.js).  

Heres a hover after hitting a breakpoint:  
![Hover after hitting a breakpoint]({{ site.url }}assets/posts/2017-07-20-vscode-is-pretty-good/hover.jpeg)

Heres a view of the call stack:  
![Stack view]({{ site.url }}assets/posts/2017-07-20-vscode-is-pretty-good/stack.jpeg)

## Its feature packed.
In sublime and atom world, you would install a plugin for formatting xml.  
In vscode you just open up the command palette, set language for the current file as xml and then format it.
 thats it, no plugin install needed.  

I actually use this feature a lot when debugging webservices that use xml or json.  

## Plugins are (also) pretty good
vscode plugins are often mantained in the main repository for vscode.  
Check out the repo for the [go tooling plugin.](https://github.com/Microsoft/vscode-go)  
theres also a lot of plugins to choose from the plugin repositories, and from the code that i've seen its pretty easy to make new ones.  

## Git integration
I actually dont use this at all, since i do everything git related on console, but hey its there if you need it.  