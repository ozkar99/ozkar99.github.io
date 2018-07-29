---
layout: post
title: "elalmirante update"
date: 2018-07-29
comments: true
---

<p class="intro">
elalmirante has now reached a stable cycle, and its being used in production.
whats new?
</p>

## elalmirante-web

we have a new interface, a web-based one.

the CLI is still going to be mantained in parallel to this one, but i believe this one is easier to use in the long run.

some screenshots:

![interface]({{ site.url }}/assets/posts/2018-07-29-elalmirante-update/interface.jpg)
![query]({{ site.url }}/assets/posts/2018-07-29-elalmirante-update/query.jpg)
![deploy]({{ site.url }}/assets/posts/2018-07-29-elalmirante-update/deploy.jpg)
![deployerror]({{ site.url }}/assets/posts/2018-07-29-elalmirante-update/deployerror.jpg)
![error]({{ site.url }}/assets/posts/2018-07-29-elalmirante-update/error.jpg)



## elalmirante-agent

elalmirante-agent is now is gRPC based instead of http.

### why?
i wanted a more robust solution than a REST-ful api.