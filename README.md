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

## Facebook

We're halfway through, so now we need to set up Facebook, to work with our hello_bot.
If you haven't already, go and create a Facebook Developer Account [here](https://developers.facebook.com).
<br>While you are there, create a Facebook Page that you will use with and for your bot.
<br>I will wait :)

Done?<br>
Nice!

Log in to your Developer Account and create a new App. If Facebook asks you what type you want, just click "basic setup" at the bottom, fill out the fields and choose "Apps for Pages" as your category.

In your new App you see the menu point "+Add Product" on the left side. I suggest you click on it to see what happens :)
<br>
The next couple of steps might seem a little confusing at first, but bear with me, we're almost done.

Click on "Messenger", select your page and copy the Page Access Token.

Remember your hello_bot's sourcecode you checked out before?
<br> Go to your editor, open "main.py" and at the very top you see a variable "access_token". Add your Facebook Access Token here and save the file.
<br> On your command line, go to your bot directory (where the sourcecode is) and enter the following command:
```
$ git commit -a -m "access_token changed" && git push
```
You have now committed and pushed the changes back to the Openshift Git Repository.<br>
Openshift does everything for you now and deploys your code directly to your cartridge. 
<br> Like before, go to your bot URL and look for "Hello World".

Everything cool?

Great!! Let's tell Facebook where our bot is!

On your Facebook developer page we left off, when you created the Page Access Token. <br>
The next step is to setup the Webhook.<br>
Copy the Access Token again, click on "setup webhook" (underneath) and paste it into the field that says "Verify Token".<br>
In callback URL you enter `https://yourbot-yourdomain.rhcloud.com/webhook`.<br>
It's the same URL that you used to see the "Hello World", but this time with "/webhook" added at the end. <br>
Facebook will POST all its communication to this endpoint, which in turn becomes the starting point for your bot.

Check all the Subscription Fields... just to be sure ;)

Click on "verify and save".

You should now see a green checkmark saying "Complete". Select your Facebook page underneath to subscribe your webhook to it. People can now chat with your bot by messaging your Facebook page.


One more step and we're done (for real this time ;))

Open Postman and send a POST request to the following URL:<br>
`https://graph.facebook.com/v2.6/me/subscribed_apps?access_token=PAGE_ACCESS_TOKEN`<br>
PAGE_ACCESS_TOKEN needs to be the same token that you've used twice now. Just replace it and POST the link.

The result in Postman should look like this:
```
{
  "success": true
}
```

## You did it!!!

If you've made it this far (and everything worked out) you can finally go to your Facebook page and click on "message".
<br>
<br>
Type something... Raise your arms, and yell "Bots bots bots bots bots!!!" :D