---
layout: distill
title: "How to write a Mastodon bot that posts random images in Python"
date: 2024-07-08
description: "Instructions for dummies from a dummy."
giscus_comments: true
tags: python mastodon bot coding
authors:
  - name: Mathias Micheel
toc:
    beginning: true
featured: true
---
As some of you might know, I am a big Suikoden fan. What's Suikoden you ask? A great JRPG series created from Konami back in 1995 for the first PlayStation. Each game stars 108 (more or less) playable characters and the story revolves around political intrigues and personal conflicts. Overall, it's a great series which hasn't seen a new entry since 2012, but will see the first two games remastered for current consoles soon.

Not only do I run the oldest and largest (because it's by now the only) German Suikoden fanpage [suikoversum.de](https://www.suikoversum.de), but I've also been involved with the Suikoden Revival Movement, a fan movement I started with several other dedicated individuals back in 2011 to campaign for rereleases of older games, see new merch etc. Altogether, the fandom is great, and one of the finest works in the fandom next to the dozens of highly talented fanartists was the [No Context Suikoden](https://twitter.com/nocontextgensui) Twitter account, which posted random bits of dialogue from the games which were mostly very silly in nature.

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
2. Something which automatically triggers the Python program in set intervals.

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
mastodon.status_post("Hello World!")
```

Run the program and see how the post is posted. Congrats on the first step!

## write a post with attached media

Let's now try something more complicated and upload an image. The way `mastodon.py` handles media posts is basically an extension to a status post. First, you need to prepare the media you want to attach to a post via `Mastodon.media_post`:

```
Mastodon.media_post(media_file, mime_type=None, description=None, focus=None, file_name=None, thumbnail=None, thumbnail_mime_type=None, synchronous=False)
```

Then,you attach the media to a post. Simple enough, huh? It is a bit more complicated, though. When you use the code above, you will upload a type of media, which gets then an id, and you need to append this id to a status. (Actually, you get a media dictionary that contains the id.)

So let's give it a try! First, we'll create an image (`koala.png`) to upload in a subfolder called `ìmages`. You then need to prepare your image and then post it.

```
os.chdir("images") # changes the working directory to /images. If you don't want that, simply skip this and store your python script in the same folder as the images.
image = mastodon.media_post("koala.png", # this is the only required argument. you can either give the filename directly or use the "media_file" argument.
                            mime_type ="image/png", # this indicates the filetype. only necessarily needed if you did not use "media_type", otherwise the program will guess the correct file type
                            description ="An image of a koala in a tree!" # adds alt text. you should definitely consider this!
                            )

# Write a "Hello World!" post with an image
mastodon.status_post("Hello world!", # this is the text associated with the message
                      media_ids=image["id"], # as said earlier, the media_post function uploads the image with an id as a dictionary. this calls the correct photo
                      )
```

This should display a post like this:

{% include figure.liquid path="assets/img/2024-07-08-mastobot-tutorial.png" class="img-fluid rounded z-depth-1" %}

Congratulations, you have written the basis for automated Mastodon posts!

## posting randomly one or multiple images

Now we can post images directly from our python console to Mastodon, which is pretty cool, but not yet what we want. Let's recap, our bot should fulfill the following criteria:

* The post consists of 1-4 pictures and a text section.
* Each picture should have alt text.
* Since some pictures depict violence/blood or have sexual overtones, so I'd like to be able turn on content warnings and/or toggle visibility of images if needed.

I tackled the first problem and the other two separately. Let's start with alt text and the sensitivity warning!

### automatically adding alt text

Adding alt text is important. Even if it's only "a picture of a black cat", it helps visually impaired people a lot. I don't know if this is the best way to do it, but I decided to store alt text (and other metadata like sensitivity or spoiler tags) in a separate file. For whatever reason, I did it in YAML format; probably because it's easier to manually edit than a CSV or TXT file., maybe because I've seen it in another tutorial, who knows.

So let's say we have an image called `imagename.png` in the subfolder `images`. Then, within the subfolder, we create a subfolder `descriptions` (so `/images/descriptions`), where we'll store all our files. In this case, we create a file called `imagename.yml` with the following content:

```
status: [text] # this is the text to be shown in the posting.
spoiler_warning:  # if not left blank, this will be spoiler tag shown
description: [alt text] # this is the alt text
language: # we will set the default language in the bot script later, but if you want to post images in different languages, you can manually set them here
sensitivity: # if set to True, the image will be blurred. If left blank or set to False, the post will be shown normally
```

This you have to do for every single image. I actually created a small python script that automatically detects image files and creates a rudimentary description file for each of them to speed up the process.

And that's it! This way you created a file that will be read out in the next step when posting an image.

### file and folder structure

Before coming to the end, let's quickly go through the folder structure and nomenclature of files! This is what it looks like.

```
root
  - images
    - descriptions
```

`root` contains `run.py` which is the script in the next section.

`images` contains all of the images your bot wants to post. It can have any nomenclature you want, but do not end the filename with a number! Why? As outlined earlier, the goal is to be able to post multiple pictures. In my case, these belong together, so sometimes a post has two, sometimes four, sometimes only one picture - but they belong together as a group. To make this easier, I added the numbers from 1-4 in the end of the filename if it's part of such a group. If only a single image is posted, the filename does not end with a number. So it looks like the following:

```
imageA.png # this is an image that will be posted alone in a post
imageB-1.png
imageB-2.png # these two images will be posted in the same post. The order of them in the post will be imageB-1.png followed by imageB-1.png
imageC.png # again a single image
imageD-1.png
imageD-2.png
imageD-3.png
imageD-4.png # a group of four images
```

I hope you get the idea. Please don't forget that the maximum number of images per post on Mastodon is four.

`descriptions` contains the YAML files of the images. Each YAML file has the same filename (without file extension) as the image. So `imageD-1.png` has a description file named `imageD-1.yml`. Every single image needs a separate description file, because the description files contain the alt text of each image.

### selecting one or multiple images and post them on mastodonpy

Now we can finally assemble everything and create the code! We need these modules.

```
from mastodon import Mastodon
from pathlib import Path
import os
import random
import yaml
```

Some of them we already know; `random` is needed to select a random picture and `yaml` is needed to read out the YAML files.

Then we configure our Mastodon access, as usual.

```
# Access Mastodon instance
mastodon = Mastodon(
    access_token=os.environ['MASTODON_ACCESS_TOKEN'],
    api_base_url='botsin.space'
)
```

I will emphasize again that you should check your server's rules ***before*** creating the bot. Many servers don't allow bots. Even botsin.space only allows a (public) post every six hours.

Next, we define two functions which will be used in the final script. The first function will prepare our media file to be posted.

```
# Function to prepare media with image descriptions etc.
def media_description(media): # media is the image name
    description_file = open("images/descriptions/" + media + ".yml", "r") # reads the corresponding YAML description file ...
    description = yaml.load(description_file, Loader=yaml.FullLoader) # ... and sets the description variable with its contents
    alt_text = description["description"] # reads the description part of the YAML file
    # so the next part might be superfluous if you only have one file format and/or trust the automatic file extension detection. Basically, I use the following to see whether the image is a gif, jpeg or png.
    os.chdir("images")  
    filename = media + file_extension
    if file_extension == ".png":
        file_format = "image/png"
    elif file_extension == ".jpeg":
        file_format = "image/jpeg"
    elif file_extension == ".gif":
        file_format = "image/gif"
    post = mastodon.media_post(filename, file_format, description=alt_text) # this creates the media for upload
    media_list.append(post["id"]) # this generates a list. Important because we want sometimes to upload multiple images in one post!
    os.chdir("..") # returns to the parent folder
```

The second function will actually create the post.

```
# Function to post status with image
def post_status_with_image(name, spoiler_warning="False", status="#suikoden", sensitivity="False", language="en"): # this sets some default settings which are overriden by the YAML file if not empty
    description_file = open("images/descriptions/" + name + ".yml", "r") # as above, loads the description YAML file
    description = yaml.load(description_file, Loader=yaml.FullLoader)
    status = description["status"]  # reads the status part of the YAML file. This is the text part of the post. I use it for hashtags etc.
    spoiler_warning = description["spoiler_warning"] # content warning and if yes, what?
    language = description["language"] # only needed if your language differs from the default
    sensitivity = description["sensitivity"] # by default False
    mastodon.status_post(status, media_ids=media_list, spoiler_text=spoiler_warning, sensitive=sensitivity,
                         language=language) # creates the post and adds all media from the media list
```

Probably, there is a more elegant solution, but this one works so it is good by my standards.

Now the actual part of the script:
```
# Choose a random photo out of the /images folder
photo = random.choice([x for x in os.listdir("images") if os.path.isfile(os.path.join("images", x))])

# Get name of photo without ending
name = Path("images/" + photo).stem

# the only reason this is here is to identify errors later when the script is running. Sometimes, your YAML file might be incorrectly filled out, or it's missing altogether. This will help in identifying errors later on!
print(name)

# Get file format of photo
file_extension = Path(photo).suffix

# Create an empty list for all the photos/media we want to add
media_list = [] # This is important when you want to add multiple images in one post. If you only have one photo always, you can keep this, but you can also try to cut it from the code.

# Check if photo is part of a series
if name[-1].isdigit():  # this checks if the last letter of the filename is a number or not.
    # Get both images
    name_1 = name[:-1] + "1"
    name_2 = name[:-1] + "2" # this checks for the image whose name ends with a 2
    name_list = [name_1, name_2] # this creates a list with both images
    # check if image 3 and 4 exist and add to list
    if os.path.exists("images/" + name[:-1] + "3.png"): # checks if there is a third image
        name_3 = name[:-1] + "3"
        name_list.append(name_3) # appends to the list
        if os.path.exists("images/" + name[:-1] + "4.png"): # and finally whether there is a fourth image
            name_4 = name[:-1] + "4"
            name_list.append(name_4) # and also appends it
    for x in name_list:
        media_description(x) # this creates the media description for each image in the list
else:   # in case it does not end with a digit, which means it is not part of a group
    # Get details of post
    media_description(name) # creates the media description for the image

# Post a new status update
post_status_with_image(name) #posts the image to Mastodon
```

Done! You have successfully written a script that randomly selects an image, checks whether it is part of a group, creates the alt text and description based on a description file, and uploads it to Mastodon!

I know that there are some faults here. Because it first randomly selects an image and then checks whether it is part of a group, groups will be posted more often than single images, and larger groups more often than smaller groups. In order to have the same chance for every group of images, you'd need to eliminate duplicates first. Honestly, this is too much work for what my bot is supposed to do, so i don't care for now, but might update it in the future!

## autoposting at selected timepoints

Obviously, you do not want to trigger the code manually each day (even though there is nothing stopping you to do so). In order to automate it, I uploaded the entire thing to github and trigger the code with a github action. As said earlier, I guess this violates their ToS, but well, it is what it is.

After you uploaded your entire folder structure to github, create a new folder called `.github/workflows` and a file `github-actions.yml`. Probably you could also give it a more illustrous name, but it doesn't matter. The file should contain the following:
```
name: [name] # name of your process.

on:
  schedule:
    - cron: 08 01 * * * # this is the time when the script is executed. In this case, it is executed every day at 01:08 am. You can create your cron code here: https://crontab.guru/

jobs:
  TriggerMastobot:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Python  # this tells the bot to use python to execute the file
        uses: actions/setup-python@v4.7.0
        with:
          # Version range or exact version of Python or PyPy to use, using SemVer's version range syntax. Reads from .python-version if unset.
          python-version: 3.10.12
      - name: Install Dependencies  # you need to install these dependencies
        run: |
          pip3 install Mastodon.py  
          pip3 install pyyaml
      - uses: actions/checkout@v2
      - name: run.py  # runs the python script. If you changed the name, change it here as well.
        env:
          MASTODON_ACCESS_TOKEN: ${{ secrets.MASTODON_ACCESS_TOKEN }} # USE THIS LIKE THIS! Explanation below
        run: python run.py
      - uses: gautamkrishnar/keepalive-workflow@v1 # using the workflow with default settings
```

Now you might wonder - what's this `${{ secrets.MASTODON_ACCESS_TOKEN }}`? As written earlier, the API token for your Mastodon should not be shared with anyone - ever. But if you put it on github, it is visible by, well, everyone! So you use this placeholder in the code instead. Then, in your github, go to `Settings > Secrets and variables > Actions` and create a new repository token `MASTODON_ACCESS_TOKEN` with your Mastodon access token. Now github will magically use this token whenever it runs the code without showing it to anyone. Sweet!

Now, your code will be executed at the designated timepoints.

# The end

I hope this post was helpful to you if you, as myself, have no idea what you're doing whatsoever. This was a fun little project, which was actually pretty easy once I understood the logic behind the Mastodon posts.

I am not yet completely happy with my code. For instance, I want to add the functionality to post certain pictures only on certain dates (like a Halloween picture only on October 31), but have not found an elegant way to do it yet. Also, the code is probably redundant at some points and could benefit from a critical review.

I appreciate any feedback on this post whatsoever! Whether you found it helpful, did not understand some parts because of my bad explanation, or have ideas for improvement, I'd be happy to read your comments!
