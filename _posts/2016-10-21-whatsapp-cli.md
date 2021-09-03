---
title: WhatsApp CLI
tag: [python, selenium]
excerpt: "Send/receive text messages to contacts and groups, switching between different chats, all from command line"
header:
  teaser: /assets/images/whatsapp.png
  overlay_image: /assets/images/whatsapp.png
  overlay_filter: 0.75 
  actions:
    - label: "Get WhatsApp CLI"
      url: "https://github.com/zvovov/whatsapp-web"
---



If you're fairly new to linux (like me) and fell in love with the terminal (like me), chances are you don't want to step outside the terminal to accomplish menial tasks. There is a way to do everything from the command line, and this projects exemplifies that belief.

**script** and **project** refer to [whatsapp web cli](https://github.com/zvovov/whatsapp-web). They are used interchangeably throughout this post.

# Setting the scene

This belief was the motivation for the project. Being able to use instant messaging from command line was the goal. Not on a brand new platform that is going to disrupt the IM space with its swanky features that makes all other platforms look like measly peasants. This Achilles heel of this new platform is that nobody knows about it. It is what Arya Stark aspires to be. It is No One... It only needs more users, like every other disruptive new product. But that is straying away from the goal. Making a new platform is not the goal. That would be easy. Making an existing platform accessible via command line, that, is the goal. Something everyone uses. WhatsApp.

WhatsApp already has a web interface. But I'm not about to rebuild that. I'm going to leverage the existing web interface to provide a command line interface to WhatsApp. 

There's already a third-party library available that enables comminucation with WhatsApp, albeit through its own API. [^1] It also has a command line interface. I'm also not going to build an alternative to that. I'm going to build a way for the user to use WhatsApp from the command line, nothing in between. All the data stays with the user or WhatsApp, unlike third-party solutions.

So, I did exactly that.

# Developing WhatsApp Web CLI

The development took place over two days, a few hours each day. The project is built in python, unsurprisingly. At this point, one can say there is no limit for my love for this language.

Key events during development:

- **Day 1**
    - selenium crashes with Firefox Webdriver. I was on Firefox 47.0, which has issues with selenium. Read that [upgrading to 47.0.1 should work](https://www.mozilla.org/en-US/firefox/47.0.1/releasenotes/). Upgraded. Now I'm on Firefox 49.0. Still crashes. Apparently, the bug again seeped in. Slated to be resolved in Firefox 51.0. [Ain't nobody got time for that](https://www.youtube.com/watch?v=Nh7UgAprdpM).
    - Chrome to the rescue. Downloaded ChromeDriver. It works with selenium.
    - Coding resumes along with reading selenium docs, stackoverflow for a few minutes and now the script opens web.whatsapp.com and prompts user to connect their device. It then opens up the all contacts page.
    - Another couple of hours and getting friendly with XPATHs and now the script can send messages to the chosen contact.
    - The user can also switch to a different chat in the same session.
- **Day 2**
    - Chat was still incomplete as one could only send messages and not receive them.
    - A python thread was spun up which polls the web.whatsapp.com webpage for any new incoming messages and prints the ones that are not yet displayed to the user.
    - Descriptive colors are added to the chat. Courtesy [joeld](http://stackoverflow.com/a/287944)
    - Still there are crashes sometimes, I suspect that it was related to python threading module.

- Minor updates included an easy way to configure the script, message output formatting changes

There are still crashes every now and then, but those are not related to threading module, as I previously thought. They are related to selenium reusing http connections. Out of the scope of this project. The crashes are few and far between. Normal usage won't be affected.

# In all its glory

Live in action.

![WhatsApp Web CLI demo](/assets/images/whatsapp.png)


> Source and Docs:
> [github.com/zvovov/whatsapp-web](https://github.com/zvovov/whatsapp-web)


[^1]: [yowsup](https://github.com/tgalal/yowsup)
