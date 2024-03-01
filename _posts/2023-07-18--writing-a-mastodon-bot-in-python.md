---
layout: distill
title: "How to write a Mastodon bot that posts random images in Python"
date: 2023-07-18
description: "Instructions for dummies from a dummy."
giscus_comments: true
tags: python mastodon bot coding
authors:
  - name: Mathias Micheel
toc:
    beginning: true
hidden: true

---
As some of you might know, I am a big Suikoden fan. What's Suikoden you ask? A great JRPG series created from Konami back in 1995 for the first PlayStation. Each game stars 108 (more or less) playable characters and the story revolves around political intrigues and personal conflicts. Overall, it's a great series which hasn't seen a new entry since 2012, but will see the first two games remastered for current consoles soon.

Not only do I run the oldest and largest (because it's the only) German Suikoden fanpage [suikoversum.de](https://www.suikoversum.de), but I've also been involved with the Suikoden Revival Movement, a fan movement I started with several other dedicated individuals back in 2011 to campaign for rereleases of older games, see new merch etc. Altogether, the fandom is great, and one of the finest works in the fandom next to the dozens of highly talented fanartists was the [No Context Suikoden](https://twitter.com/nocontextgensui) Twitter account, which posted random bits of dialogue from the games which were mostly very silly in nature.

Now, with Twitter becoming a digital hell and my move to the Fediverse, I wanted to bring such an account also over to Mastodon (after having asked the person who ran the account if it was ok to repost images). I thought this would be easy - I mean, there are dozens of bots on Mastodon that post random cat pictures every hour, so how hard can it be?

Well, pretty hard for someone as tech savy as me. I think the most complex piece of code I've ever written was to change the name of every file in a directory by adding a terminal e, and even that takes me hours to write. So naturally, I was looking for good tutorials on the matter which I could simply copy&paste, but I just couldn't find one that fitted my needs. After a couple of hours, I managed to get the bot running, and I'm so proud that I wanted to share my knowledge here in a short tutorial so if you're also interested in writing a bot, this might help you in getting started! I try to be very detailed - possibly too detailed - in a lot of steps, but if you are a programming beginner like me, I hope you appreciate the effort.

**Disclaimer** As I said, I am no coding expert. The code I'm showing here works, but at what cost? Well, I have no idea, but I would greatly appreciate if people with more expertise in coding (with Python) would give feedback on the code to improve it. Also, for me, it's mostly an educational experience, so I want to learn something, so any feedback is appreciated! You can post it directly as a comment under this post if you have a github account or contact me over at Mastodon.

# introduction

## sources of inspiration

As mentioned earlier, I looked for tutorials, but didn't find any that wanted to do exactly what I do and/or were so elusive that I couldn't follow them. I still consider it good practice to link to them, since they might be useful to you:
* [Easy guide to building Mastodon bots](https://shkspr.mobi/blog/2018/08/easy-guide-to-building-mastodon-bots/)
* [Creating a Mastodon Bot with Python](https://blog.tiagorangel.com/creating-a-mastodon-bot-with-python)
* [Using Mastodon bots for data visualization](https://stefanbohacek.com/blog/using-mastodon-bots-for-data-visualization/)
* [Making a Mastodon bot that posts random images](https://botwiki.org/resource/tutorial/making-a-mastodon-bot-that-posts-random-images/)
* [Tweet-Toot: Building a bot for Mastodon using Python](https://www.ayush.nz/2018/09/tweet-toot-building-a-bot-for-mastodon-using-python)

Going through each of these was very educational, since it helped me understanding where my understanding was still lacking! If you have found other tutorials, please share them!

# prerequisites

To use this guide, you should fulfill the following conditions:
* Github account - we will use github to store our data and use github actions to activate the bot at selected intervals.
* Python - this program is not complicated, but in order to troubleshoot, it is definitely advisable to know some Python or programming basics!
* A Mastodon account - if you want to create a generic bot, https://botsin.space/ is probably the best server, as it is solely dedicated to bot accounts. If you want to set it up on a different server, make sure it is allowed; many servers don't want random bots that post pictures of french fries every 10 minutes as it puts a lot of stress on their server.

And that's essentially it! All of the above is free, but you can of course use different websites and programs to start a bot. I'm not even sure my approach applies with the github ToS ... but that's a problem for another day.

## goal

First, we need to define what our bot should do in the end. I sketched the workflow as follows:
* The bot should post once at defined timepoints (e.g., once a day at a set time).
* The post consists of 1-4 pictures and a text section.
* Each picture should have alt text.
* Since some pictures depict violence/blood or have sexual overtones, so I'd like to be able turn on content warnings and/or toggle visibility of images if needed.

All of this should be automated and my only job is the selection of images and generation of status text and alt text. I don't want to do anything manually afterwards at all!

Even though I talk about *a* bot, the entire thing is actually divided in two parts:

1. The Python program which selects an image and generates the post. This could also be triggered manually.
2. Something which automatically triggeres the Python program in set intervals.

This guide will help you set up both. Let's go!

# setting up mastodon.py

We will use the fantastic [mastodon.py](https://mastodonpy.readthedocs.io/en/stable/index.html) package which will streamline a lot of the process of our small program. However, it also takes a bit of time to get used to, so we let's say "Hello!" to our new friend.

## registering your app and preparing your profile

If your app should communicate with your Mastodon account, you first need to register the app. While this can be done also using mastodon.py, I think it's easier to just do it manually. If you want to start 100 bots, doing it automatically is probably a big time saver.

Log into your Mastodon bot account and go to Settings > Development and click on *new application*. Give your application a name (it doesn't matter, but it will be visible later on the Mastodon interface, so chose something neutral like "Autoposter") and ignore the website field. Here, you select the permissions of your bot. Since the bot I envision only posts text and photo posts, the only permissions I need are
* **write:media**, which will allow the bot to upload photos, and
* **write:statuses**, which will allow the bot to post.

I deselect every other option. Of course, if you want a more elaborate bot that automatically favourites certain posts or should be a bit interactive, you'll need to select these permissions. I won't cover any of that here, though. You can also change the permissions (but not the name of the bot) afterwards if you need to.

After you submitted your application, you find it listed in the Development tab. Click on it and you see a bunch of parameters on top. **Don't share your client key, secret, or access token with anybody.** With these, anyone could access your bot and do all kind of things. We will need them later for our bot.

You're already finished, but it is good practice to mark your account as a bot account. To do so, you find a checkbox under Settings > Profile. Checking it will put a small "bot" icon next to your account name.

## writing a "Hello World!" post

Now we are ready to write our first small program. As a test, we will send a small "Hello world!" message to our profile, because ... well, why not. So create a file called `run.py` (or be more creative than me) and start editing it.

We'll need to first import the Mastodon module:

`from mastodon import Mastodon`

This allows us to use all the nice `mastodon.py` functions! And we will directy start with one, which is probably the most important one, as it tells our program where to find our bot and that it is authenticated:

```
# Setting up Mastodon
mastodon = Mastodon(
  access_token='[ABCDEFGHIJKLMNOPQRSTUVWXYZ123456789]', # this is your access token from the Development tab from the step before
  api_base_url='botsin.space' # this is the URL of your bot's server
)'
```

As I said earlier, the access token is very sensitive information. If you plan on making your bot somehow public, you shouldn't put it in the file as we did now. We will address this later in more detail and how to avoid it!

Finally, we write our small post using the `Mastodon.status_post` function (more details on the function can be found [here](https://mastodonpy.readthedocs.io/en/stable/05_statuses.html#writing)). We can use this function and use lots of attributes. The full function looks like this:

`Mastodon.status_post(status, in_reply_to_id=None, media_ids=None, sensitive=False, visibility=None, spoiler_text=None, language=None, idempotency_key=None, content_type=None, scheduled_at=None, poll=None, quote_id=None)``

And the attributes we'll need are:

* `status` - is the text of the posts
* `media_ids` - will be used later for our photo positions
* `sensitive` - whether the photos will be marked as sensitive or note
* `spoiler_text` - whether our post will include a spoiler/content warning
* `language` - will be default the language set in our profile (in our case, `en`), but I plan to include multi-language posts in the future

We can safely ignore the remaining attributes. To write our small "Hello World!" message, we need to simply input the following:

`Mastodon.status_post("Hello World!")`

The final program looks like this:

```
from mastodon import Mastodon
# Setting up Mastodon
mastodon = Mastodon(
  access_token='[ABCDEFGHIJKLMNOPQRSTUVWXYZ123456789]', # this is your access token from the Development tab from the step before
  api_base_url='botsin.space' # this is the URL of your bot's server
)'
# Write a "Hello World!" post
Mastodon.status_post("Hello World!")
```

Run the program and see how the post is posted. Congrats on the first step!

## write a post with attached media

Let's now try something more complicated and upload an image. First, we'll
