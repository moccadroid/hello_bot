# hello_bot

hello_bot is a skeleton for a facebook messenger bot to run on Google App Engine.<br>
If you follow the tutorial below, your bot should be up and running in less than an hour :)


## Tools & SDKs
[PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/) <br>
[Python 2.7](https://www.python.org/downloads/ ) <br>
[Git](https://git-scm.com/downloads) <br>
[Google Cloud SDK](https://cloud.google.com/sdk/) <br>
[Postman](https://www.getpostman.com/) <br>

## Hello Bot
Check out Sample Code from https://github.com/moccadroid/hello_bot using git: <br>
```
git clone https://github.com/moccadroid/hello_bot.git
```

## Google App Engine
Go to [Google App Engine](https://cloud.google.com/appengine/) and register an account. You will need a credit card, but Google offers a 60 days trial for free.

After you've done this you'll be dropped into the Google Cloud Console. In the top right corner you'll see "My first Project" in the top menu bar. <br>
Clicking on it reveals "Create a project.." which let's you create a new project. Do this :)

When you're done with this, Google again drops you in the dashboard of your new project.
Click on the menu button in the top left corner and select "Development".
Since your project still doesn't contain any sourcecode click on "Get started" and choose the first option <br>```Push code from a local Git repository to your Cloud Repository```

Open the Google Cloud SDK Shell, cd to your hello_bot directory on your computer and follow the instructions.
(in step 3 remove the "\\" at the end of the first line)

You should now have your hello_bot project pushed to your Google Cloud Repository, we're halfway there.

Start the Cloud Shell (the button in the top right corner next to the present) and type the following:

```
$ mkdir src
$ cd src
$ gcloud source repos clone default --project=<your project-name>
$ cd default
$ gcloud app deploy
```

Open your browser and point it to `https://<project-name>.appspot.com`<br>
If you see "Hello World", everything works and you can finally set up Facebook ;)


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
<br> Open the Google Cloud SDK Shell again, navigate to your project folder and type the following commands:
```
$ git commit -a -m "access_token changed" && git push --all google
```
You have no committed and pushed the changes back to the Google Cloud Git Repository.<br>
Go back to your browser, open the Google Cloud Console, navigate to your code (should be in src/default) and type this:
```
$ git pull && gcloud app deploy
```
Google now deploys your bot to its services. You can check if everything worked out by pointing your browser to `https://<project-name>.appspot.com` and look for "Hello World".

Everything cool?

Great!! Let's tell Facebook where our bot is!

On your Facebook developer page we left off, when you created the Page Access Token. <br>
The next step is to setup the Webhook.<br>
Copy the Access Token again, click on "setup webhook" (underneath) and paste it into the field that says "Verify Token".<br>
In callback URL you enter `https://<project-name>.appspot.com/webhook`.<br>
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