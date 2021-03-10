---
layout: post
title: Make an Auto-Posting Social Media Quote-bot with Python and GitHub Actions. Part 1 Text-Quotes
subtitle: Creating a social media quote-bot isn't very hard and can expand the reach of life serving messages
share-description: First, I gathered quotes from transcripts of Marshall Rosenberg. Then I made a bot to automatically post those quotes to Twitter (and Facebook) using GitHub Actions and Python.
cover-img: 
thumbnail-img: 
share-img: /assets/img/making-social-media-quote-bot_python-twitter-API-github-actions.webp
tags: [intermediate,python]
---

This post will describe my process for creating a twitter quote-bot, including step-by-step instructions, complete with the code I'm currently using to run [@MBR_Quotes](https://twitter.com/MBR_Quotes).

--- 

I've become a Marshall Rosenberg enthusiast over the past year, and have become convinced that the world can greatly benefit from becoming familiar with [his teaching](https://danforth-restorative.com/2020/07/22/nonviolent-communication.html).

I was inspired to make a twitter quote-bot, to share his message with others who find it meeting their needs, while becoming much more intimate with it myself, and developing my technical skills. 

During my studies, I found a [podcast with nonviolent communication trainings and interviews with Marshall Rosenberg](https://anchor.fm/nvc-archive), which directed me to [transcripts for some of them](https://github.com/cognitivetech/Marshall-Rosenberg-NVC) that made a perfect foundation for this project.

Of course, the ability to create a social media quote-bot is also marketable skill, that could be useful for a variety of purposes, both personally and professionally.

This walk-through strings together a handful of fundamental skills, so that once you've completed each step, you'll be able to apply this knowledge towards a variety of domains and have come a long way toward being a coder. 

I had to learn each skill individually just figuring things out on my own, over a couple years, so you should feel very lucky to get this all on a single page! 🤣

## How to create a social media quote-bot

This guide walks the reader through each step in my process, explaining what tools I use, and how I use them.

The process begins with gathering and preparing quotes. Then getting a developer account on twitter, reviewing each line of the Python code used to publish these quotes, and the steps necessary to automate that on GitHub.

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
    - [How are you planning to use the Twitter API?](#how-are-you-planning-to-use-the-twitter-api)
    - [Will your app use Tweet, Retweet, Like, Follow, or Direct Message functionality?](#will-your-app-use-tweet-retweet-like-follow-or-direct-message-functionality)
    - [Developer agreement](#developer-agreement)
    - [Post application e-mail exchange](#post-application-e-mail-exchange)
    - [Get your API keys](#get-your-api-keys)
    - [Generate access tokens](#generate-access-tokens)
- [Making the quote bot](#making-the-quote-bot)
  - [Python Script for Publishing Text Quotes to Twitter](#python-script-for-publishing-text-quotes-to-twitter)
  - [Dependencies](#dependencies)
    - [Install Python](#install-python)
    - [Install Git](#install-git)
    - [Set Git User\Email](#set-git-useremail)
  - [Make a copy of this project on GitHub](#make-a-copy-of-this-project-on-github)
  - [Create a local clone of your brand-new github repository](#create-a-local-clone-of-your-brand-new-github-repository)
- [Using VSCode to manage your project](#using-vscode-to-manage-your-project)
  - [Save your twitter credentials as environment variables](#save-your-twitter-credentials-as-environment-variables)
    - [On your computer](#on-your-computer)
    - [Set up workflow file for GitHub Actions](#set-up-workflow-file-for-github-actions)
    - [The Workflow file](#the-workflow-file)
    - [Customizations](#customizations)
  - [Have Fun!](#have-fun)
  
## Prep

### Gathering Source Material

There are many resources online that could be used as the raw materials for a quote-bot. You could use the lyrics of your favorite band, transcripts of podcasts, or videos online.

If you begin with audio, it's possible to auto-generate transcriptions for them. They'll still require some manual editing, but you can quickly create plenty of fodder for your quotebot. [FreeTranscriptions.com](https://www.freetranscriptions.com/) offers 300 minutes of free auto-transcriptions, each month. 

YouTube automatically transcribes its videos, but older videos may not have auto-generated transcripts. I've even uploaded content to youtube for its transcription services, before.

![](https://i.imgur.com/zcfmPvd.png)

Click Toggle Timestamps and you can copy the whole thing, and manually extract your favorite quotes.

![](https://i.imgur.com/7YWYmxX.png)

[Project Gutenberg](https://www.gutenberg.org/) has the largest collection of public domain books available online.

[![](https://i.imgur.com/4JR6BFA.png)](https://www.gutenberg.org/ebooks/bookshelf/24)

You could also take the easy route, and get quotes from [Goodreads](https://www.goodreads.com/quotes), or [other quotes sites online](http://www.quotationspage.com/collections.html).

If you're wondering how many quotes you want, just think about how often you'd like to tweet. I wanted to publish 4 tweets a day, and have enough quotes to get me through a year without needing to repeat. I don't think I got that many, maybe I only got enough for 6 months. But I wanted to gather enough tweet sized quotes so I wouldn't have to worry about getting more for a while.

### Preparing the Quotes

For this exercise, we're going to use [YAML](https://yaml.org/spec/1.2/spec.html), a simple data structure that makes it easy for a programming language to read your quotes. 

#### Introduction to VSCode

I use [VSCode](https://code.visualstudio.com/) all of the time for editing large documents, and working on projects with multiple directories of files I may need to edit. It's easy to use, but also has tons of advanced features that would take a long time to learn all of.

VSCode or Code allows you to search\replace text within files of entire directories, and there are extensions for different languages and data-types that help you avoid errors. 

VSCode also integrates very nicely with GitHub, which will save you from having to use `git` manually. If you aren't familiar with `git` already, VSCode will prove invaluable.

If you don't care for VS Code, or its too resource intensive, you may prefer [Notepad++](https://notepad-plus-plus.org), which is a little less resource intensive, but offers comparable features.

**Open VSCode and create a new file.**

![](https://i.imgur.com/UXgiYLb.png)

Go to *File > Save as...* you can save it in your Documents directory, for now, and name it `quotes.yml`. 

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

`\n` is a newline character, which is automatically turned into an actual new-line by the python interpreter. 

Be careful if there are quotation-marks in your quote. You could also surround text with a single quote if there are double quotes inside, like so:

```yaml
- 'Single Quote can "contain" double quotes.'
```

I'm not 100% this is necessary, but its less ambiguous.

#### Transforming a plain list of quotes into a YAML list of quotes

If you have a document with each quote on a new line, you can use search and replace in VSCode to make easy work of this:

![](https://i.imgur.com/07bnXPS.png)

That example uses [regular expression](https://docs.microsoft.com/en-us/visualstudio/ide/using-regular-expressions-in-visual-studio?view=vs-2019) (RegEx) which is a wildcard system allowing you to change many instances of text at the same time.

The blue asterisk activates regular expression in your search query. In the top search box `\n` is a newline character. `.*` selects every character on a line, and surrounding it with parentheses `(.*)` saves it to be used in the replace. 

As you type your RegEx query, the matching selection is automatically highlighted on the page.

Then in the replace box `$1` provides the value captured in parentheses above, and if you have multiple expressions surrounded with parentheses, in the same search query, you can reference them sequentially, left to right.

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

If you have a lot of quotes, this will especially come in handy.

For an example, I removed the closing quotation mark from a line, circled in red. You'll notice that the filename turns red on our side panel, and there is a red mark on the scrollbar, that lets me notice there is a formatting error.

A grey line goes through the middle of the scrollbar, showing where your cursor is in context with the rest of the document. 

I scrolled down the page until my scrollbar reached the red mark pointing out our YAML error. (Note the bottom right of the picture below with a red arrow)

Once I clicked the line where the error occurs, that grey line jumped to the red mark on the scrollbar.

![](https://i.imgur.com/mJgyrKk.png)

### Get a twitter developer account

Now that you've collected and prepared quote material, go ahead and apply for a twitter developer account. 

If you have any questions about this process, have any trouble or questions about using your twitter developer account more generally, you can check out the [Twitter Developer Forums](https://twittercommunity.com/).

#### Apply 

Once I had some quotes formatted, I used this guide to get started with the code:

* [How to write a Twitter Bot with Python and Tweepy](https://dototot.com/how-to-write-a-twitter-bot-with-python-and-tweepy/)

You may want to try out that script first, to see how this works. All it will do is post a single tweet, so it's easier to proceed with the later parts of this guide if you've tried that first.

It instructs us, login to [dev.twitter.com](https://dev.twitter.com/) with the account I want to auto-tweet from.

![](https://i.imgur.com/ql3VyhW.png)

Click the Apply button to begin. On the next page you'll click "Apply for a twitter developers account". 

![](https://i.imgur.com/GTZnKMw.png)

On the next page, click "Hobbyist", "Making a Bot" and then "Get Started".

#### How are you planning to use the Twitter API?

After you fill out basic info, you will be asked "**How are you planning to use the Twitter API or Twitter Data**". It suggests your application will be easier to approve the more descriptive you are, so try to make a few sentences about how you are using the API.

I filled out something like this:

> I want to schedule post from a collection of quotes and images with quotes printed onto them.
> 
> I'm making this bot as a learning experience with python and writing each step of the process.

Next it asks if you are analyzing twitter data, in this case we are not.

#### Will your app use Tweet, Retweet, Like, Follow, or Direct Message functionality?

Yes it will, and so the next form we go into a little more detail what the app will do.

> This script will tweet 4-6 times a day a different quote from a list of prepared material. When users re-tweet, my script will like their re-tweet. 
> 
> I will not need use of re-tweet, follow, or direct messaging via API

the rest of the questions I click "No" and go to the next page which reviews the information already submitted, so we can click next again.

#### Developer agreement

Yes, we agree to the developer agreement. Like I said, for a simple tweet bot, this is not a concern. When you develop more complex bots for twitter, it's important to really read the developers agreement and understand how you are expected to use it. Especially if you want to automate some of the use of your personal twitter account, not just a new account to try out some code.

Be sure to verify your e-mail address, and you will see this message:

![](https://i.imgur.com/x5cabsn.png)

#### Post application e-mail exchange

After I filled out my application I got this message, the next day.

> Before we can finish our review of your developer account application, we need some more details about your use case. 
>
> The types of information that are valuable for our review include:
> 
> - The core use case, intent, or business purpose for your use of the Twitter APIs.
> - If you intend to analyze Tweets, Twitter users, or their content, share details about the analyses you plan to conduct, and the methods or techniques.
> - **If your use involves Tweeting, Retweeting, or liking content, share how you’ll interact with Twitter accounts, or their content.**
> - If you’ll display Twitter content off of Twitter, explain how, and where, Tweets and Twitter content will be displayed with your product or service, including whether Tweets and Twitter content will be displayed at row level, or aggregated.
> 
> Just reply to this email with these details. Once we’ve received your response, we’ll continue our review. We appreciate your help! 

I revised my answer a couple times before they accepted it. Each time, they patiently pointed me to the [Developer Agreement and Policy](https://developer.twitter.com/en/developer-terms/agreement-and-policy.html), [Automation Rules](https://help.twitter.com/en/rules-and-policies/twitter-automation) & [Twitter Rules](https://help.twitter.com/en/rules-and-policies/twitter-rules), requesting me to re-write your use-case in accordance with the policies.

Notice the section in bold, above, about tweeting, retweeting, and liking content. I had to read the various policy links and figure out exactly what the rules are for each.

After a few tries, back and forth with the developer support, here's my final response that they accepted:

> _My core use-case for the Twitter API is to develop my tech skills, share valuable knowledge, and support community growth. I do not intent to make money with this project, but in the future I may apply these skills to profitable ventures._
> 
> _This account is made to share quotes that help people understand whiteness as a social construct, and its role in shaping society throughout the history of the world, particularly the united states._
> 
> _I do not intend to analyze tweets, twitter users, or their content. The greatest extent of analyzing twitter data would include keyword\hashtag search, to find other content related to this project._
> 
> _The script I'm working on is only for tweeting quotes from a pre-defined list of content._
> 
> _I may sometimes browse and manually like responses to my tweets or retweets. I will manually follow accounts I believe are sympathetic to the type of content I'm creating based on their tweets and who they follow. When I come across educational or otherwise informative content that that aligns with the quotes I'm sharing, I may like and retweet those using tweet-deck. I won't message users unless they message me first._
> 
> _If I display content outside of twitter, I would use publish.twitter.com_

#### Get your API keys

> Your Twitter developer account application has been approved!
>
> Thanks for applying for access. We’ve completed our review of your application, and are excited to share that your request has been approved.
>
> Sign in to your [developer account](https://developer.twitter.com/en/portal/register/welcome) to get started.

Follow that link, name your app and get your **API Key**, **API Secret Key**, and **Bearer Token**.

![](https://i.imgur.com/acbeWKo.png)

I'd recommend using a password manager such as [keepassxc](https://keepassxc.org/) or [lastpass](https://www.lastpass.com/) for storing these keys (and all of your passwords).

However you decide to manage this, try to keep them safe.

Now you can click "skip to dashboard".

It shows you your keys one last time, its a good time to double check the values you copied are correct.

I double check that the first and last 4 digits of each string matches, then proceed to the dashboard.

#### Generate access tokens

First thing to do on the dashboard, is scroll down below your "App Details" to "App Permissions"

![](https://i.imgur.com/mKu7kkk.png)

Be sure to give your app both read and write permissions.

Next, click the "Keys and Tokens" tab of your settings, and generate your **Access Token** & **Access Secret**. 

![](https://i.imgur.com/y66FjpI.png)

The first keys we got let the Twitter API know who you are, these access keys determine what permissions the app has, according to the setting where you just enabled read and write permissions.

> Click the “Create” button in the "Access token & access token secret" section. – [Twitter Developers Documentation](https://developer.twitter.com/en/docs/twitter-api/enterprise/account-activity-api/guides/authenticating-users#:~:text=User%20owns%20the%20app%20%2F%20Single,%26%20access%20token%20secret"%20section.)

Once you've saved the keys, the page you'll be brought back to should indicate that your access keys have both read and write permissions. Otherwise, you can go back to the Settings page, adjust the permissions, and then regenerate the access tokens.

![](https://i.imgur.com/HamxUjP.png)

## Making the quote bot

### Python Script for Publishing Text Quotes to Twitter

Without any further ado, lets check out the script!

There it is, 23 lines of python (minus comments). I've tried to explain each line as thoroughly as possible. You should be able to copy this script, set a few variables, and be ready to roll. 

First read the script and its comments. All lines starting with `#` are comment lines that explain what each line does. After that, we'll walk through the steps to test it out locally, and then deploy to github.

```python
# Tweet Quotes from Yaml

### This next bit is python telling your computer how to interpret your code
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Import necessary packages
import tweepy, yaml, random, os
## Tweepy helps talk to twitter. 
## Yaml helps process our YAML structured quotes. 
## Random is a "random" number generator. 
## OS lets the script access your shell environment where we'll be storing our keys, both for local deployment and on GitHub.

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

Git is a tough customer, and takes time to get used to and begin to understand, but since we're using VSCode, we shouldn't have to worry too much about that, once set up. 

The way Git works is it keeps a copy of the entire project directory, for every change you "commit" to the repository. (you'll see how to do that momentarily). It gives people a system to manage changes across multiple versions of a project, in a distributed fashion. Whether that's multiple people working on it or a single person working on it.

It's basically like a blockchain where nobody *has* to agree, but it helps them agree when they choose to. :-P

#### Set Git User\Email

If you don't set your Git user name and e-mail, then VSCode won't know how to talk to GitHub account you're about to open (with the same email address you set here). The user name is not as critical but the email should be the same one you open your github account with.

`git config --local user.name "GitHub Actions Tester"`\
`git config --local user.email github-testing@danforth-restorative.com`

### Make a copy of this project on GitHub

This project is stored as a GitHub Template Repository, making it easy for anyone make a copy of this project.

Creating a GitHub account is straight-forward. Use the e-mail you just configured with git config and, once you are logged in, go to this address:

[https://github.com/digitalskills-info/GitHub-Actions-Quote-Bot](https://github.com/digitalskills-info/GitHub-Actions-Quote-Bot)

**Click 'Use this Template'**
![](https://i.imgur.com/HHjg49n.png)

**Choose a descriptive name for your repository**
![](https://i.imgur.com/heZogsx.png)

### Create a local clone of your brand-new github repository

![](https://i.imgur.com/QiHUljm.png)

Click the copy button in the smaller red circle. Notice you are getting the HTTPS version of the link.

Now you need to decide where to work on this project. Navigate to whatever folder you have quotes saved in or create a new space to work.

**On Windows**:

`cd C:\%HOMEPATH%\Documents` 

**On Mac\Linux**:

`cd ~/Documents`

**Type 'git clone' plus the URL you just copied to clone (download) your copy of this Git repository**

`git clone https://github.com/trying-github-actions/quote-bot.git`

`cd quote-bot`

Now you can install two packages this script depends upon:

`pip3 install pyyaml` (or `pip install pyyaml`)\
`pip3 install tweepy` (or `pip install tweepy`)

I'm pretty sure you have to install these python packages on a project by project basis, that's why we navigated to our working directory before installing.

Now you can also move the quotes file, that you made earlier, to this directory.

## Using VSCode to manage your project

If you don't care for VS Code, you might like to try [GitHub Desktop](https://desktop.github.com). It isn't as powerful as VS Code, but it's more user-friendly. You may also choose to [sync the changes with Git manually](https://dont-be-afraid-to-commit.readthedocs.io/en/latest/git/commandlinegit.html).

Otherwise, go ahead and open VSCode, then select "open folder"

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

**The Extension 'GitHub' wants to sign in using GitHub**\
Click allow, to sign into your GitHub account.

![](https://i.imgur.com/aToRcJM.png)

**Make sure you are using the same browser where you are logged into github**\
![](https://i.imgur.com/6YOf6iT.png)

Click continue:

![](https://i.imgur.com/v1R6Pt1.png)

**If you see a message like this, you've succeeded in linking VSCode to GitHub**

![](https://i.imgur.com/pb33RQt.png)

Otherwise, close code, then re-open your folder and try again. 

If you've succeeded in authenticating with GitHub, your sync button shouldn't have any numbers next to it, and you will have added the quotes file you made earlier to your copy of this project on GitHub. Go see if your project has been updated, and if not, try again. :)

![](https://i.imgur.com/w8WwUu2.png)

### Save your twitter credentials as environment variables

An environment variable allows a program to access a value (in this case your developers credentials), without storing those values directly in the code.

We're going to set environment variables on your computer first, then on github, so the code can be run either place without a security risk or the trouble of making two versions of the same code, one for local testing and one for GitHub.

#### On your computer

**[Windows Instructions](https://superuser.com/questions/79612/setting-and-getting-windows-environment-variables-from-the-command-prompt)**

> To make the environment variable accessible globally you need to set it in the registry. As you've realised by just using:
>
>```
>    set NEWVAR=SOMETHING
>``` 
> 
> you are just setting it in the current process space.
> 
> According to this page you can use the [setx](https://ss64.com/nt/) command:
>
>```
>    setx NEWVAR SOMETHING
>```
>
> setx is built into Windows 7, but for older versions may only be available if you install the Windows Resource Kit

To be clear, that means `set` saves your variable only for this session, `setx` saves it 'permanently'.

**Mac \ Linux**

`export KEY=value` is the formula we're following.

You can use the following commands to save your keys in the shell environment.

```
export CONSUMER_KEY=XXXYourAPIKeyXXX
export CONSUMER_SECRET=XXXYourAPISecretXXX
export ACCESS_KEY=XXXYourAccessKeyXXX
export ACCESS_SECRET=XXXYourAccessSecretXXX
````

To save them in a lasting fashion, add those lines to your `.bashrc` or `.zshrc` file (which may be a hidden file) in your home directory.

Now type `source ~/.bashrc` to reload your shell with those settings, and then `echo $CONSUMER_KEY` to be sure you've succeeded.

If you're going to create multiple apps that have different keys, then your variables need to have different names. Or you could use a [`.env`](https://pypi.org/project/python-dotenv/) file, but then you would need a different copy of the script (with hardcoded keys) for local use, which defeats the purpose.

Now you should be able to run `python quotes.py` and see a quote published to twitter.

#### Set same environment variables on GitHub

Now we're getting ready to tie this experience together with a bow. 

Head over to your copy of this project on github, that you made earlier in this guide. Click settings, then click `secrets` on the side-bar, and then click `New Repository Secret` on the right.

![](https://i.imgur.com/lerflZW.png)

```
CONSUMER_KEY=XXXYourAPIKeyXXX
CONSUMER_SECRET=XXXYourAPISecretXXX
ACCESS_KEY=XXXYourAccessKeyXXX
ACCESS_SECRET=XXXYourAccessSecretXXX
````

![](https://i.imgur.com/X7c97HY.png)

You set these repository secrets for each of the above keys, just like we saved in our local environment earlier. 

![](https://i.imgur.com/1wbKhQa.png)

#### Set up workflow file for GitHub Actions

Open your local version of this project in VS Code.

The workflow files are in `.github/workflows.disabled`. If you don't see those files, they may be hidden, so check the settings of your directory. 

![](https://i.imgur.com/wP3VpCZ.png)

#### The Workflow file

```yaml
{% raw %}
# Name it whatever you like
name: quotebot

# Triggers the workflow on push or pull request
# https://crontab.guru/ helps to figure out this scheduling syntax works.
# I scheduled two tweets a day for this workflow.
on:
  schedule:
    - cron:  '11 11 * * *'
    - cron:  '22 19 * * *'
  # It also runs whenever the script or the workflow files are changed.
  push:
    paths:
    - 'quotes.py'
    - '.github/workflows/quote.yml'

# A workflow run is made of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of operating system the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed
    steps:
      # Checkout makes a local copy for the script
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.8.5' # Range or exact, using SemVer syntax
        # Installs dependencies listed in requirements.txt
      - uses: py-actions/py-dependency-install@v2   
        with:
          path: requirements.txt
      - shell: bash
        env: # Saves our secrets where the script can access them
          CONSUMER_KEY: ${{ secrets.CONSUMER_KEY }}
          CONSUMER_SECRET: ${{ secrets.CONSUMER_SECRET }}
          ACCESS_KEY: ${{ secrets.ACCESS_KEY }}
          ACCESS_SECRET: ${{ secrets.ACCESS_SECRET }}
        # Post random quote to twitter
        run: python quotes.py
        # remove quote from yaml as posted.
      - name: Save updated quotes.yml
        run: |
          git remote add gh-token "https://github.com/danforth-restorative/MBR_Quotes.git"
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git pull --ff-only
          git commit -a -m "de-dupe"
          git push gh-token main
{% endraw %}
```

Now you can move quote.yml to the workflows directory.

![](https://i.imgur.com/4cqqkwP.png)

Go ahead and commit that change, then push that change to GitHub. If all goes well, it will run the action and post a quote when you do.

#### Customizations

GitHub cron syntax is slightly different from [crontab.guru](https://crontab.guru), if you're having any trouble, go to the workflows folder of your project on GitHub, and edit that file there.

In that editor, if you hover over the cron expression, a pop-up tells you exactly how its interpreted.

![](https://i.imgur.com/CtICkLy.png)

Notice that I've made a backup of the `quotes.yaml` file called `quotes.bak`. To avoid duplicate tweets, we're removing the quote from `quotes.yaml` at the time its posted. Be sure to make a backup of your quotes file, although the nature of Git ensures that even if you don't, you can still retrieve the original.

### Have Fun!

You may notice [this project's repository on GitHub](https://github.com/danforth-restorative/MBR_Quotes/) contains scripts for posting image tweets, as well as facebook text\image tweets.

It also has scripts for bulk cropping images, printing text onto images, and other associated tools.

After a while, I'll write about using the rest of it.