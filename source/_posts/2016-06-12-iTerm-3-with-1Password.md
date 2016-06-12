title: iTerm 3 with 1Password
tags:
  - productivity
  - os x
  - tools
date: 2016-06-12 17:54:50
---


iTerm 3 is just [released](https://www.iterm2.com/version3.html), and it comes with some awesome features.
One of my favorite is the "trigger" feature. It can detect (with regular expression) certain pattern from the text in your terminal and trigger some actions.
Also, iTerm 3 provides yet another feature called "Password Manager". It can store some password for you, and integrating with trigger, it enables us to enter password quickly. I recorded a short gif to demonstrate this feature:

{% fullimage "/img/2016/6/12-iterm3pm.gif", "iTerm3 with Password Manager", "iTerm3 with Password Manager" %}

It's pretty handy. However, one thing concern me is that I don't want to spread my password everywhere. The reason I use 1Password is that I want to keep all my password in one place. And clearly, using iTerm's password manage violate my expectation. That's why I came up with another solution with Reuven's [sudolikeaboss](https://github.com/ravenac95/sudolikeaboss). After install `sudolikeaboss`, we can setup a trigger like this:

{% fullimage "/img/2016/6/12-iterm3trigger.png", "iTerm3 Trigger", "iTerm3 Trigger" %}

And then we could have iTerm play with 1Password:

{% fullimage "/img/2016/6/12-iterm3-1pw.gif", "iTerm3 with 1Password", "iTerm3 with 1Password" %}
