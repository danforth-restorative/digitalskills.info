---
layout: post
title: Make an Auto-Posting Social Media Quote-bot with Python and GitHub Actions. [1]Text-Quotes
subtitle: Creating a social media quote-bot isn't very hard and can expand the reach of life serving messages
share-description: First, I gathered quotes from transcripts of Marshall Rosenberg. Then I made a bot to automatically post those quotes to Twitter (and Facebook) using GitHub Actions and Python.
cover-img: 
thumbnail-img: 
share-img: 
tags: [intermediate,python]
published: false
---

This post will describe my process for creating a twitter quote-bot, including step-by-step instructions, complete with the code I'm currently using to run [@MBR_Quotes](https://twitter.com/MBR_Quotes).

I've become a Marshall Rosenberg enthusiast over the past year. I'm convinced that the world can greatly benefit from becoming familiar with his message.

During my studies, I found some great trainings and Nonviolent Communication workshop recordings, [including transcripts for some of them](https://github.com/cognitivetech/Marshall-Rosenberg-NVC).

I decided it would be great to make a twitter quote-bot to share his message, and become more familiar with it myself.

Of course, creating a social media quote-bot is also marketable skill, that could be put to use in a variety of ways both personally and professionally.

This first post describes gathering and preparing quotes, getting a developer account on twitter, the source code used to publish these quotes, and the steps necessary to automate that on GitHub.

## How to create a social media quote-bot

Here are the steps that will be detailed in this walk through. 

- [How to create a social media quote-bot](#how-to-create-a-social-media-quote-bot)
- [Prep](#prep)
  - [Gathering Source Material](#gathering-source-material)
  - [Preparing the Quotes](#preparing-the-quotes)
    - [Introduction to VSCode](#introduction-to-vscode)
    - [Our quotes are going to be structured like this](#our-quotes-are-going-to-be-structured-like-this)
    - [Transforming a plain list of quotes into a YAML list of quotes](#transforming-a-plain-list-of-quotes-into-a-yaml-list-of-quotes)
    - [YAML Extension](#yaml-extension)
  - [Get a twitter developer account](#get-a-twitter-developer-account)
    - [Apply](#apply)
      - [How are you planning to use the Twitter API](#how-are-you-planning-to-use-the-twitter-api)
      - [Will your app use Tweet, Retweet, Like, Follow, or Direct Message functionality?](#will-your-app-use-tweet-retweet-like-follow-or-direct-message-functionality)
      - [Developer agreement](#developer-agreement)
    - [Twitter developer account application [ ref:_00DAAK018._5005e26SSlh:ref ]](#twitter-developer-account-application--ref_00daak018_5005e26sslhref-)
- [Making an auto-posting twitter bot](#making-an-auto-posting-twitter-bot)
  - [Python Twitter Script for Publishing Text Quotes](#python-twitter-script-for-publishing-text-quotes)
  - [Dependencies](#dependencies)
    - [Install Python](#install-python)
    - [Install Git](#install-git)
    - [Set Git User\Email](#set-git-useremail)
  - [Make a copy of this project on GitHub](#make-a-copy-of-this-project-on-github)
  - [Create a local clone of your brand-new github repository](#create-a-local-clone-of-your-brand-new-github-repository)
- [Using VSCode to manage your project](#using-vscode-to-manage-your-project)
  - [Save your twitter credentials as environment variables](#save-your-twitter-credentials-as-environment-variables)
    - [On your computer](#on-your-computer)
  

## Prep

### Gathering Source Material

There are many resources online that can be used as a foundation for a quote-bot. You could use the lyrics of your favorite band, transcripts of podcasts or videos online.

If you only have audio, it's possible to auto-generate transcriptions for them. They'll still require some manual editing, but you can quickly create plenty of fodder for your quotebot. 

[FreeTranscriptions.com](https://www.freetranscriptions.com/) offers 300 minutes of free auto-transcriptions, each month. Sometime, I'll write a post on using the [Google Api to generate auto-transcriptions](https://cloud.google.com/speech-to-text) but that's getting pretty technical and beyond the scope of this article..

YouTube automatically transcribes its videos.

![](https://i.imgur.com/zcfmPvd.png)

Click Toggle Timestamps and you can copy the whole thing, and manually extract your favorite quotes.

![](https://i.imgur.com/7YWYmxX.png)

[Project Gutenberg](https://www.gutenberg.org/) has the largest collection of public domain books available online.

[![](https://i.imgur.com/4JR6BFA.png)](https://www.gutenberg.org/ebooks/bookshelf/24)

Those ideas are just to get you started. There are many uses for a twitter quote-bot, but if you just want to learn how to make the bot, and worry about a use-case later, these ideas for source material should get you started.

You could also take the easy route, and get quotes from [Goodreads](https://www.goodreads.com/quotes), or [other quotes sites online](http://www.quotationspage.com/collections.html).

If you're wondering how many quotes you want, just think about how often you'd like to tweet. I wanted to publish 4 tweets a day, and have enough quotes to get me through a year without needing to repeat. I don't think I got that many, maybe I only got enough for 6 months, to give me enough time to recover from the hard work of quote collection :D.

### Preparing the Quotes

For this exercise, we're going to use [YAML](https://yaml.org/spec/1.2/spec.html), a simple data structure that makes it easy for a programming language to read your file. 

It would have worked just as well to put each quote on a new line, and there are plenty of data formats that could also be used for this purpose. This is just the way I did it.

#### Introduction to VSCode

We're going to use [VSCode](https://code.visualstudio.com/) for preparing the quote material. It's easy to use, but also has tons of advanced features that deserve an article of their own.

VSCode or Code allows you to search\replace text within files of entire directories, and there are extensions for different languages and data-types that help you avoid errors. 

VSCode also integrates very nicely with GitHub, which will save you from having to use `git` manually. If you aren't familiar with `git` already, VSCode will prove invaluable.

![](https://i.imgur.com/e3tIXgZ.png)

**Start by opening VSCode and create a new file.**

![](https://i.imgur.com/UXgiYLb.png)

Go to *File > Save as...* you can save it in your Documents directory for now, and name it `quotes.yml`. 

#### Our quotes are going to be structured like this

```yaml
---
- "Always hear the 'Yes' in the 'No'.\n\n#NVC #NonviolentCommunication"
- "Never do anything that isn't play.\n\n#NVC #NonviolentCommunication"
- "Avoid 'shoulding' on others and yourself!\n\n#NVC #NonviolentCommunication"
- "Ask before offering advice or reassurance.\n\n#NVC #NonviolentCommunication"
```

YAML is pretty straightforward. 

* `---` begins the file.
* Each line begins with a dash `-`
* The text is surrounded by "Double Quotes"

`\n` is a newline character, which is automatically turned into an actual new-line by the python interpreter. Be careful if there are quotation-marks in your quote. You could also surround text with a single quote if there are double quotes inside, like so:

```yaml
- 'Single Quote "can contain" double quotes.'
```

It might work to use double quotes instead of single-quotes altogether (as long as you use an even number of quotation marks), however, this is less confusing for the computer.

#### Transforming a plain list of quotes into a YAML list of quotes

If you have a document with each quote on a new line, you can use search and replace in VSCode to make easy work of this:

![](https://i.imgur.com/07bnXPS.png)

That example uses [regular expression](https://docs.microsoft.com/en-us/visualstudio/ide/using-regular-expressions-in-visual-studio?view=vs-2019) (RegEx) which is a wildcard system allowing you to change many instances of text at the same time.

The blue asterisk activates regular expression in your search query. In the top search box `\n` is a newline character. `.*` selects every character on a line, and surrounding it with parentheses `(.*)` saves it to be used in the replace. 

As you type your RegEx query, the matching selection is automatically highlighted on the page.

Then in the replace box `$1` provides the value captured in parentheses above, and if you have multiple expressions surrounded with parentheses, in the same search query, you can reference them sequentially, left to right, like so:

`Results: "$1" and "$2" are my search selections`

Hopefully you get the idea that this expression will do most of the work turning a file with quotes on each line to a yaml file.

![](https://i.imgur.com/shyqJj3.png)

You can see the results. Notice that there is a blank space between one of the lines and its closing parenthesis. 

Now that search and replace took care of the hard work, its easy to do this manually, or even a regular search\replace in Code. 

Search for ` "` and replace with `"` to fix both at the same time.

```
---
- "line 1"
- "line 2"
- "line 3"
- "line 4"
- "line 5"
- "line 6"
```

#### YAML Extension

Click the extensions tab circled in red, and get the YAML Language Support by Red Hat.
![](https://i.imgur.com/DzuKH9H.png)

If you have a lot of quotes, especially, this will come in handy.

For an example, I removed the closing quotation mark from a line, circled in red. You'll notice that the filename turns red on our side panel, and there is a red mark on the scrollbar. 

A grey line goes through the middle of the scrollbar, showing where your cursor is in context with the rest of the document. 

I scrolled down the page until my scrollbar reached the redmark pointing out our YAML error. 

Once I clicked the line where the error occurs, that grey line jumped to the red mark on the scrollbar.

![](https://i.imgur.com/mJgyrKk.png)

### Get a twitter developer account

Now that you've collected and prepared quote material, you can apply for a twitter developer account. You may like to actually apply for the account first, in case you run into any snags, so its ready when you are ready. However, in my experience, it takes more time to gather and prepare quotes than to get the developer account.

If you have any questions about this process, beyond the scope of this article, or questions about using your twitter developer account more generally, you can check out the [Twitter Developer Forums](https://twittercommunity.com/)

#### Apply 

Once I had some quotes formatted, I used this guide to get started with the code:

* [How to write a Twitter Bot with Python and Tweepy](https://dototot.com/how-to-write-a-twitter-bot-with-python-and-tweepy/)
 
It instructed me to login to [dev.twitter.com](https://dev.twitter.com/) with the account I want to tweet from.

![](https://i.imgur.com/ql3VyhW.png)

Click the Apply button to begin. On the next page you'll click "Apply for a twitter developers account". This process will be simple, because we're only making a quote bot. The rules are a bit more complicated if you want to re-tweet, for example.

![](https://i.imgur.com/GTZnKMw.png)

On the next page, click "Hobbyist", "Making a Bot" and then "Get Started".

Fill out what you want them to call you and verify that you are requesting.

##### How are you planning to use the Twitter API

After you fill out basic info, you will be asked "**How are you planning to use the Twitter API or Twitter Data**". It suggests your application will be easier to approve the more descriptive you are, so try to make a few sentences about how you are using the API.

I filled out something like this:

> In the first iteration, I want to schedule post from a collection of quotes and images with quotes printed onto them.
> 
> Later, I plan to also 'auto-like' when users re-tweet or quote-re-tweet.
> 
> I'm making this bot as a learning experience with python and writing each step of the process.

Next it asks if you are analyzing twitter data, in this case we are not.

##### Will your app use Tweet, Retweet, Like, Follow, or Direct Message functionality?

Yes it will, and so the next form we go into a little more detail what the app will do.

> This script will tweet 4-6 times a day a different quote from a list of prepared material. When users re-tweet, my script will like their re-tweet. 
> 
> I will not need use of re-tweet, follow, or direct messaging via API

the rest of the questions I click "No" and go to the next page which reviews the information already submitted, so we can click next again.

##### Developer agreement

Yes, we agree to the developer agreement. Like I said, for a simple tweet bot, this is not a concern. When you develop more complex bots for twitter, it's important to really read the developers agreement and understand how you are expected to use it. Especially if you want to automate some of the use of your personal twitter account, not just a new account to try out some code.

Be sure to verify your e-mail address, and you will see this message:

![](https://i.imgur.com/x5cabsn.png)

When I was making the [@MBR_Quotes](https://twitter.com/MBR_Quotes) bot, I got the developers account almost immediately. When I tried to make a different re-tweet bot, there was a lot of back and forth with the developers support, in which I had to re-describe the intended behavior of my app until it fit within [Developer Agreement and Policy](https://developer.twitter.com/en/developer-terms/agreement-and-policy.html), [Automation Rules](https://help.twitter.com/en/rules-and-policies/twitter-automation), and/or the [Twitter Rules](https://help.twitter.com/en/rules-and-policies/twitter-rules).

#### Twitter developer account application [ ref:_00DAAK018._5005e26SSlh:ref ]

Ok, I filled out my application last night, and today I got this message:

> Before we can finish our review of your developer account application, we need some more details about your use case. 
>
> The types of information that are valuable for our review include:
> 
>     The core use case, intent, or business purpose for your use of the Twitter APIs.
>     If you intend to analyze Tweets, Twitter users, or their content, share details about the analyses you plan to conduct, and the methods or techniques. 
>     If your use involves Tweeting, Retweeting, or liking content, share how you’ll interact with Twitter accounts, or their content.
>     If you’ll display Twitter content off of Twitter, explain how, and where, Tweets and Twitter content will be displayed with your product or service, including whether Tweets and Twitter content will be displayed at row level, or aggregated.
> 
> Just reply to this email with these details. Once we’ve received your response, we’ll continue our review. We appreciate your help! 

Here's my response:

_My core use-case for the Twitter API is to develop my tech skills, share valuable knowledge, and support community growth. I do not intent to make money with this project, but in the future I may apply these skills to profitable ventures._

_I do not intend to analyze tweets, twitter users, or their content. The greatest extent of analyzing twitter data would include keyword\hashtag search, to find other content related to this project._

_The script I'm working on is only for tweeting quotes from a pre-defined list of content. All other interaction with other user and\or their content will be manual._

_If I display content outside of twitter, I would use [publish.twitter.com](https://publish.twitter.com/#)_

_Let me know if you need anything else._

_Regards,_
_David Danforth_

If you give a wrong answer, they will patiently point you to the [Developer Agreement and Policy](https://developer.twitter.com/en/developer-terms/agreement-and-policy.html), [Automation Rules](https://help.twitter.com/en/rules-and-policies/twitter-automation) & [Twitter Rules](https://help.twitter.com/en/rules-and-policies/twitter-rules), requesting you to re-write your use-case in accordance with the policies.

This is the response I got:

> Thanks for your response. We still need some more details for our review of your Twitter developer account application. 
> 
> The information we still need includes: 
> 
> * The core use case, intent, or business purpose for your use of the Twitter APIs.
>  * Please note, “business purpose” in this context includes uses not necessarily connected to a commercial business. We require information about the problem, user story, or the overall goal your use of Twitter content is intended to address.
>   * If you are a student, learning to code, or just getting started with the Twitter APIs, please provide details about potential projects, or areas of focus.
> * If you intend to analyze Tweets, Twitter users, or their content, please share details about the analyses you plan to conduct, and the methods or techniques.
>   * Note that “analyze” in this context includes any form of processing performed on Twitter content. Please provide as detailed and exhaustive an explanation as possible of your intended use case.
> * **If your use involves Tweeting, Retweeting, or liking content, share how you will interact with Twitter users or their content.**
> * If you’ll display Twitter content off of Twitter, please explain how, and where, Tweets and Twitter content will be displayed to users of your product or service, including whether Tweets and Twitter content will be displayed at row level, or aggregated.
> 
> To provide the information, please respond to this email. Where possible, please share links to illustrations, or sample work products. 


## Making an auto-posting twitter bot

### Python Twitter Script for Publishing Text Quotes

Without any further ado, lets review this script!

There it is, 23 lines of python (minus comments). I've tried to explain each line as thoroughly as possible. You should be able to copy this script and set a few variables and be ready to roll. 

First read the script and its comments (all lines starting with `#` are comment lines). After that, we'll walk through the steps to test it out locally and deploy to github.

```python
# Tweet Quotes from Yaml

### This next bit tells the computer how to understand your code
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Import necessary packages
import tweepy, yaml, random, os
## Tweepy helps talk to twitter. 
## Yaml helps process our YAML structured quotes. 
## Random is a "random" number generator. 
## OS lets the script access your local shell environment where we'll be storing our keys, both for local deployment and on GitHub.

# os.environ.get() requests the credentials stored in environment variables (we'll be setting in a moment) and saves them in a python variable.
CONSUMER_KEY = os.environ.get('CONSUMER_KEY')
CONSUMER_SECRET = os.environ.get('CONSUMER_SECRET')
ACCESS_KEY = os.environ.get('ACCESS_KEY')  
ACCESS_SECRET = os.environ.get('ACCESS_SECRET')      

## Authorize your account with twitter.
auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
auth.set_access_token(ACCESS_KEY, ACCESS_SECRET)
api = tweepy.API(auth)

## Ideally this would be displayed as part of your tweet (not working yet).
sourceLabel = "Marshall Rosenberg Quotes"

## This reads your quotes file and saves it as the 'file' variable
with open('quotes.yml','r') as file:
    # The FullLoader converts from yaml to python dict
    # your quotes are saved in the quotes variable
    quotes = yaml.load(file, Loader=yaml.FullLoader)
    # Save a random quote to the item variable
    item = random.choice(quotes)
    # Makes debugging easier by printing the item variable in the terminal
    print(item)
    # Post tweet (I've not figured out source label)
    api.update_status(status=item, source=sourceLabel)
    # closes the file when you are done with it
    file.close()

# This next bit is a trick to avoid duplicates by removing the quote from your list after its been posted. 
## Open quotes and a tmp file
with open('quotes.yml') as og, open("tmp", "w") as dest:
    ## Iterate over each line of the quotes file
    for line in og:
        ## "de-code" the quote and check it against currently printed quote
        decoded_string = bytes(line, "utf-8").decode("unicode_escape") 
        ## Print each line into the tmp file except our current quote
        if item not in decoded_string:
            dest.write(line)
            ## Replace quotes.yml with our tmp file
os.rename("tmp", "quotes.yml")
```

### Dependencies

Now you know what the script looks like and how it works, lets set up our development environment.

#### Install Python

First check to see if you have it, and if so which version(s).

Open a terminal\command-prompt, if you don't have one open already.

* *Windows*: press `Ctrl + r` on your keyboard, and type "Command Prompt" or search from your start menu.
* *Mac*: press `Ctrl + space` on your keyboard and type "terminal"
* *Linux*: press `Ctrl + shift + t` on your keyboard to open a new terminal window.

Type `python -V` or `python3 -V` and your terminal output should look something like this:

```
Python 3.7.0
```

If you don't have it you can get the latest version of Python3 from [Python.org Downloads](https://www.python.org/downloads/).

Once you've installed Python, type `python -V` or `python3 -V` to make sure you've got python3 and you know which command to use.

Its possible your system could have a different version of python already installed that it uses for system packages. 

Python(2) and Python3 commonly live on the same machine, and the `python` command could be referring to either one of them.

If you have any trouble. [Pyenv](https://github.com/pyenv/pyenv) allows you to install multiple versions of python and define which one to use for different projects, and doesn't disturb your system python.. but shouldn't be necessary for most following this guide.

#### Install Git 

Git is a version control system. It keeps track of changes in your project and lets you sync the changes in your project between your computer and GitHub. It is designed for collaborative software development, which you're participating in by following this walk-through.

[Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). 

Verify you can access git from your command line:

`git version` or `git --version`

and the output should look something like this.

```
git version 2.29.0
```

Git is a tough customer, and takes a little time to get used to, but since we're using VSCode, we shouldn't have to worry too much about it, once set up. 

#### Set Git User\Email

If you don't set this then VSCode won't know how to use the GitHub account you're about to open (with the email address you set here). The user name is not as critical but the email should be the same one you open your github account with.

`git config --local user.name "GitHub Actions Tester"`
`git config --local user.email github-testing@danforth-restorative.com`

### Make a copy of this project on GitHub

I have the entire project this article is based on stored in a GitHub template repository, to make it as easy as possible for people brand new to this type of technical work.

Creating a GitHub account is straight-forward, once you are logged in, go to this address:

[https://github.com/digitalskills-info/GitHub-Actions-Quote-Bot](https://github.com/digitalskills-info/GitHub-Actions-Quote-Bot)

**Click 'Use this Template'**
![](https://i.imgur.com/HHjg49n.png)

**Choose a name for your repository, 'quote-bot' works or anything you like**
![](https://i.imgur.com/heZogsx.png)

### Create a local clone of your brand-new github repository

![](https://i.imgur.com/QiHUljm.png)

Click the copy button in the smaller red circle.

Now you need to decide where to work on this project. Navigate to whatever folder you have quotes saved in or create a new space to work.

I don't know on windows where the command prompt default opens to, but you can get to your users home directory by typing:

`cd C:\%HOMEPATH%\Documents` 

on Mac\Linux

`cd ~/Documents`

**Paste the command you copied to clone (download) your copy of this project**

`git clone https://github.com/trying-github-actions/quote-bot.git`

`cd quote-bot`

Now you can install two packages this script depends upon:

`pip3 install pyyaml` (or `pip install pyyaml`)\
`pip3 install tweepy` (or `pip install tweepy`)

I'm pretty sure you have to install these python packages on a project by project basis, that's why we navigated to our working directory before installing.

Now you can also move the quotes file, that you made earlier, to this directory. Of course, you could also clone the project first and work on it from there.

## Using VSCode to manage your project

Go ahead and open VSCode, then select "open folder"

![](https://i.imgur.com/ZN9H9NE.png)

Navigate to your project folder and then click open.

![](https://i.imgur.com/Fv20W2y.png)

Now you should see the directory listing on your left and a window in your right that lets you view\edit most any file you select.

![](https://i.imgur.com/go7wMiH.png)

Before we move forward, you should make your first commit, and sign Code into your GitHub account.

Click the tab on the left. This is how Code manages Git, and where you indicate which files should be synced with your project on GitHub.

Click `+` and enter a message like "My first Commit". The plus lets Code know you want that file added to your project.

![](https://i.imgur.com/GQGOpAg.png)

**Click the circle at the bottom** (this command syncs your local version with the version on GitHub. It first pulls any changes from your github version, and then "pushes" your commit)

**The Extension 'GitHub' wants to sign in using GitHub**
Click allow, to sign into your GitHub account.

![](https://i.imgur.com/aToRcJM.png)

**Make sure you are using the same browser where you are logged into github**
![](https://i.imgur.com/6YOf6iT.png)

Click continue:

![](https://i.imgur.com/v1R6Pt1.png)

**If you see a message like this, you've succeeded in linking VSCode to GitHub**

![](https://i.imgur.com/jIHnbP3.png)

Otherwise, close code, then re-open your folder and try again. 

If you've succeeded in authenticating with GitHub, your sync button shouldn't have any numbers next to it, and you will have added the quotes file you made earlier to your copy of this project on GitHub. Go see if your project has been updated, and if not, try again. :)

![](https://i.imgur.com/w8WwUu2.png)

### Save your twitter credentials as environment variables

#### On your computer

