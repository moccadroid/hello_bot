# hello_bot

hello_bot is a skeleton for a facebook messenger bot to run on Openshift.<br>
If you follow the tutorial below, your bot should be up and running in less than an hour... or at least a couple of hours... at most a day :D

If you find any errors in this tutorial, don't keep them! Send them to me, so we can make this tutorial even better. 
The same obviously goes for any suggestions or ideas ;)

## Tools & SDK
* [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/) (optional) you can use any editor you are comfortable with<br>
* [Python 2.7](https://www.python.org/downloads/ ) <br>
* [Git](https://git-scm.com/downloads) <br>
* [Ruby](https://www.ruby-lang.org/en/downloads/) If you're on windows, make sure to install version 2.0.0!!
* [RHC Tools](https://developers.openshift.com/managing-your-applications/client-tools.html)
* [Postman](https://www.getpostman.com/) <br>

## Hello Bot

We have setup a very simple hello_bot that just throws everything you write back at you.
It is based on the awesome [Flask Framework](http://flask.pocoo.org/) and should allow you to be up and coding in no time.

If you have no idea about Git, maybe check out [this](http://rogerdudler.github.io/git-guide/) tutorial.
In case you have a little more time check out [this](https://try.github.io/levels/1/challenges/1) too ;)

## Openshift

###***DISCLAIMER***
***Openshift just recently changed their free plan to something else. If you already have an Openshift Account you can still use it like described below. For all
those of you, that want to now register an account, the below tutorial sadly doesn't work anymore. I'll update this section as soon as I find the time.
In the meantime, just come and ask! We'll figure it out together :)***

Go to [Openshift](https://www.openshift.com/) and sign up for their free plan. The only restriction is that if you bot doesn't receive any request for 24 hours 
your free Openshift gears will go into idle mode. That means the next request will take a little longer (up to 30 seconds) until the gear is booted up again.

On the Openshift website create your first application and choose the Python 2.7 Cartrdige.
<br>Set your application name and the domain under which it will be hosted.
<br>Further down you see the point `Source Code`. Here add `https://github.com/moccadroid/hello_bot.git` and the branch `openshift`.<br>

Openshift will now pull the `hello_bot` from our github account and deploy it.
<br>After this you can go to the URL you created for your Openshift application (something like https://yourbot-yourdomain.rhcloud.com) and should see "Hello World". 

Now some nerdy stuff ;)
I hope you've installed Ruby at that point, because now we'll need it.
Open up a terminal... ***whaaaat?***
On Windows you press the Windows Key and just type `cmd`. A black window should open which is a terminal. Windows calls it "Command Prompt"<br>
On Mac OSX you press Command + Space and type `terminal`. A white window should open which is a terminal. OSX calls it "terminal" ;) <br>

Enter your first command to install the RHC Tools:
```
$ gem install rhc
```
This takes a bit... when you're done you have to setup your openshift account with the following command:
```
$ rhc setup
```
You can answer any questions with yes, if you're unsure ;)


In your Openshift Cartridge you see a link to the Source Code that looks something like this:
<br>`ssh://xxxxxxxxxxxxxxxxxxxxxxxxxx@yourbot-yourdomain.rhcloud.com/~/git/yourbot.git/`

Copy that and on your command line go to a folder where you want your code to reside and type the following:
```
$ git clone ssh://xxxxxxxxxxxxxxxxxxxxxxxxxx@yourbot-yourdomain.rhcloud.com/~/git/yourbot.git/
```

Open this folder with your editor and you're set up to develop your first bot!! :)
<br> If you don't know where your sourcecode is, it should be in your home directory in the folder named like your bot ;)

You can now continue with the rest of the Tutorial [here](https://github.com/moccadroid/hello_bot)