# 🤖 Creating your Facebook Messenger bot with hello_bot

hello_bot is a python skeleton for a facebook messenger bot based on [Flask Microframework](http://flask.pocoo.org/).<br>

We have tutorials and specific code for Openshift and Google App Engine. Just check out the `openshift` or `appengine` branches
to see the respective tutorials and how to set everything up.

To run your bot you have a couple of choices (obviously there are countless more, but these are the ones we suggest)
Choose one, and come back here when you've set up how you want to run your bot :)
* [Openshift](https://github.com/moccadroid/hello_bot/tree/openshift)
* [Google Appengine](https://github.com/moccadroid/hello_bot/tree/appengine)
* [Heroku](https://github.com/moccadroid/hello_bot/tree/heroku)


***Welcome Back!!***

## Setup the Facebook App

1. We're halfway through, so now we need to set up Facebook, to work with our hello_bot.
If you haven't already, go and create a Facebook Developer Account [here](https://developers.facebook.com).
2. While you are there, create a Facebook Page that you will use with and for your bot.
<br>
<br>I will wait :)

Done?<br>
Nice!

3. Log in to your Developer Account and create a new App.

![Create a new FB App](/demo/fb-add-app.png)

If Facebook asks you what type you want, just click "basic setup" at the bottom, fill out the fields and choose "Apps for Pages" as your category.

4. In your new App you see the menu point "+Add Product" on the left side. I suggest you click on it to see what happens :)
<br>
The next couple of steps might seem a little confusing at first, but bear with me, we're almost done.

5. Click on "Messenger", select your page and copy the Page Access Token.

![Copy the FB Page Access Token](/demo/fb-page-access-token.png)

<br> Remember your hello_bot's sourcecode you checked out before?

## Setup the Bot

1. Go to your editor, open "main.py" and at the very top you see a variable "access_token". Add your Facebook Access Token here and save the file.
2. On your command line, go to your bot directory (where the sourcecode is) and enter the following command to commit and push the changes back to the Openshift Git Repository:

```
$ git commit -a -m "access_token changed" && git push
```

Openshift does everything for you now and deploys your code directly to your cartridge.

<br> Like before, go to your bot URL and look for "Hello World".

Everything cool?

Great!!

## Let's tell Facebook where our bot is!

On your Facebook developer page we left off, when you created the Page Access Token. <br>
The next step is to setup the Webhook.<br>

Copy the Access Token again, click on "setup webhook" (underneath) and paste it into the field that says "Verify Token".<br>
In callback URL you enter `https://yourbot-yourdomain.rhcloud.com/webhook`.<br>

It's the same URL that you used to see the "Hello World", but this time with "/webhook" added at the end. <br>
Facebook will POST all its communication to this endpoint, which in turn becomes the starting point for your bot.

Check all the Subscription Fields... just to be sure ;)

Click on "verify and save".

You should now see a green checkmark saying "Complete". Select your Facebook page underneath to subscribe your webhook to it. People can now chat with your bot by messaging your Facebook page.

![Facebook Webhook Complete](/demo/fb-webhook-complete.png)

One more step and we're done (for real this time ;))

## Trigger the Facebook App to send messages

Open Postman and send a POST request to the following URL:<br>
`https://graph.facebook.com/v2.6/me/subscribed_apps?access_token=PAGE_ACCESS_TOKEN`<br>
PAGE_ACCESS_TOKEN needs to be the same token that you've used twice now. Just replace it and POST the link.

On OSX you can copy & paste the command below into the Terminal to trigger the Facebook app to send messages. Use the PAGE_ACCESS_TOKEN mentioned above.

```
curl -X POST "https://graph.facebook.com/v2.6/me/subscribed_apps?access_token=<PAGE_ACCESS_TOKEN>"
```

The result in Postman or in the Terminal should look like this:
```json
{
  "success": true
}
```

## You did it!!!

If you've made it this far (and everything worked out) you can finally go to your Facebook page and click on "message".
<br>
<br>
Type something... Raise your arms, and yell "Bots bots bots bots bots!!!" :D

![Message your Bot](/demo/fb-message-bot.png)

## What's next??

If you've never programmed, you now sit in front of your bot and have no idea what to do next.
<br> I have some suggestions you could try:

Find the following line in your main.py:
```python
reply(sender_id, message_text)
```
and change it to:
```python
rules(sender_id, message_text)
```

Now post the following code at the end of your main.py:
```python
def rules(recipient_id, message_text):
    rules = {
        "Hello": "World",
        "Foo": "Bar"
    }

    if message_text in rules:
        reply(recipient_id, rules[message_text])

    else:
        reply(recipient_id, "You have to write something I understand ;)")
```

## What have we done??

We have added a new function that allows us to reploy to specific words that the user sends our bot.<br>
```python
rules = {
    "Hello": "World",
    "Foo": "Bar"
}
```
This is called a dictionary. You can add as many variables as you like. The left side of each entry is what the user has to enter, to get the right side.<br>
You could for example send the user a link to a cat picture if they send your bot the word `cat`.
Your dictionary could then look like this:
```python
rules = {
    "cat": "https://lazycuriokitty.files.wordpress.com/2013/06/36345108.jpg"
}
```
Now in your terminal (in the folder where your code is) just type this again to push all the code to openshift:
```
$ git commit -a -m "dictionary" && git push
```

## Welcome back, we've udpated some stuff!!

If you create a new project in Openshift and import the current code from here, you'll see a couple of new functions.
<br> I'll now briefly explain what they mean and what they do :)

### Replies
```python
def reply_with_text(recipient_id, message_text):
    message = {
        "text": message_text
    }
    reply_to_facebook(recipient_id, message)
```
Since Facebook allows you to reply not only with text but also pictures etc. we renamed the old `reply(recipient_id, message_text)` to `reply_with_text(recipient_id, message_text)`
<br> It still works the same though.
<br> If you want to reply with some fancy pictures and text you can now use this:
```python
title = "Hello"
image = "http://cdn.shopify.com/s/files/1/0080/8372/products/tattly_jen_mussari_hello_script_web_design_01_grande.jpg"
message_text = "My important message_text"

element = create_generic_template_element(title, image, message_text)

reply_with_generic_template(sender_id, element)
```
`create_generic_template_element(title, image, message_text)` converts your message into a form that facebook understands.
<br>Instead of just `message_text` you now send back this element.
<br>You can also send a list of elements if you like. That could look like this:
```python
element1 = create_generic_template_element("title1", "image1_url", "message1_text")
element2 = create_generic_template_element("title2", "image2_url", "message2_text")
reply_with_generic_template(sender_id, [element1, element2])
```
Facebook has even more possibilities for you to reply with structured messages. If you interested head over to the [Facebook Developers Page](https://developers.facebook.com/docs/messenger-platform/send-api-reference)
and adapt the code to your likings :)

### GET Urls
Another small change is the function `get_url(url)`
<br>With this method you can get data from other webservices. Let's take a look at an example!!
<br> Open Postman and enter this url `http://data.wien.gv.at/daten/geo?service=WFS&request=GetFeature&version=1.1.0&srsName=EPSG:4326&outputFormat=json&typeName=ogdwien:WLANWRLOGD`
<br> If you have selected `GET` as the HTTP method and hit `Send` you'll see something like this:
```json
{
  "type": "FeatureCollection",
  "totalFeatures": 10,
  "features": [
    {
      "type": "Feature",
      "id": "WLANWRLOGD.fid--5e42d738_1565b782478_-7862",
      "geometry": {
        "type": "Point",
        "coordinates": [
          16.414651641971762,
          48.18974868077848
        ]
      },
      "geometry_name": "SHAPE",
      "properties": {
        "OBJECTID": 1029782,
        "NAME": "Wiener Linien WLAN",
        "ADRESSE": "3., Kundenzentrum Erdberg, Erdbergstraße 202 ",
        "WEITERE_INFORMATIONEN": "http://blog.wienerlinien.at/gratis-wlan/",
        "ANBIETER": "http://www.wienerlinien.at/",
        "SE_ANNO_CAD_DATA": null
      }
    }, ...
```

This is the JSON reply of the Open Government Data Platform of Vienna, if you ask them about all the WLAN hotspots of the Wienerlinien.
<br> There is a TON more data if you check [here](https://open.wien.gv.at/site/datenkatalog/?search-term=&formatTopFilter_JSON=on&formatFilter_JSON=on&connection=and#showresults)
<br> Make sure you select `JSON` under Filters.

#### So what do we do with this?
Well first of all we call this URL in Python. It would look like this:
```python
result = get_url("http://data.wien.gv.at/daten/geo?service=WFS&request=GetFeature&version=1.1.0&srsName=EPSG:4326&outputFormat=json&typeName=ogdwien:WLANWRLOGD")
```
Second we need to do something with the result.
<br>JSON is built like a tree. That means if you want to know the name of a particular WLAN hotspot, you have to go through the result in the same way, the JSON result is structured.
An important thing to note is that JSON has objects and lists. If you see `{}` then it's an object and you can directly access it like this:
```
result["type"]
```
This would result in `FeatureCollection`.

But if you see `[]` somewhere that means you can't just access it straight away but you have to *loop* over the included elements.
So if we want to print all the names of the hotspots we need to do this:
```python
for features in result["features"]:
    print features["properties"]["NAME"]
```
The result here would be `Wiener Linien WLAN`... 10 times... because that's how they roll! They named all their WLANs the same way :D

You can take it a step further and create a carousel with the 10 `Wiener Linien WLAN` results:

```python
if "message" in messaging_event:

    sender_id = messaging_event['sender']['id']

    if 'text' in messaging_event['message']:
        message_text = messaging_event['message']['text']

        if message_text == "wifi":
            # Fetch the data from Wiener Linien's API
            result = get_url("http://data.wien.gv.at/daten/geo?service=WFS&request=GetFeature&version=1.1.0&srsName=EPSG:4326&outputFormat=json&typeName=ogdwien:WLANWRLOGD")

            # Create a list which we'll use for collecting the wifi router results
            entries = []

            # Iterate through each entry in the results
            for entry in result["features"]:
                entry = create_generic_template_element(entry["properties"]["NAME"], "http://blog.wienerlinien.at/wp-content/uploads/2016/04/header_wifi.jpg", entry["properties"]["ADRESSE"])

                # Add each wifi router to the list we've created above
                entries.append(entry)

            # Add each wifi router to the list we've created above
            reply_with_generic_template(sender_id, entries)

            # After we've sent the message with the generic template we stop the code
            return "ok", 200
```

Here's how it looks:

![Carousel with Wiener Linien WLAN Hotspots](/demo/example-carousel-wiener-linien.png)

### Working with the Current Location

In the next code example you can extract the latitude and longitude values when receiving the current location and further process it according to your needs. In our example below we return the address via making a call to the Google Maps Public API with our latitude and longitude values.

```python
if "message" in messaging_event:

    sender_id = messaging_event['sender']['id']

    # Check the message input for an attachment
    if 'attachments' in messaging_event['message']:
        # Extract the latitue and longitude values
        lat = messaging_event['message']['attachments'][0]['payload']['coordinates']['lat']
        lng = messaging_event['message']['attachments'][0]['payload']['coordinates']['long']

        # Make a Google Maps Public API call with the latitude and longitude values
        google_maps_results = get_url('http://maps.googleapis.com/maps/api/geocode/json?latlng=%s,%s&sensor=false' % (lat, lng))

        # Reply with the address of the location via text message (as unicode with u'' when using strings)
        reply_with_text(sender_id, u'I found this address for your location: 📍' + google_maps_results['results'][0]['formatted_address'])
```

![Address of the current location](/demo/example-current-location.png)

## How to create a simple JSON Database

In this example we'll save the `sender_id` and the `created_at` date of a Facebook User writing with your Messenger bot in a JSON file that's stored in your project directory.

Here we go:

Create a file `data.json` in your directory with the content `{}`.

Import datetime to be able to save the date of creation:
```python
from datetime import datetime
```

Define the database json file in a global constant variable:
`DB_FILE = "data.json"`

Load the JSON file and save it as `user_db` variable:
```python
with open('data.json') as data_file:
    user_db = json.load(data_file)
```

Define a function to save the `json` file:
```python
def save_db():
    with open('data.json', 'w') as data_file:
        json.dump(user_db, data_file)
```

Here's how you save the `sender_id` and the `created_at` timestamp to the JSON:

```python
if "message" in messaging_event:

    sender_id = messaging_event['sender']['id']

    # writing and actually persisting the db
    if sender_id not in user_db:
        userdata = {
            "sender_id": sender_id,
            "created_at": str(datetime.now())
        }
        user_db[sender_id] = userdata
        save_db()
    else:
        userdata = user_db[sender_id]
```

That's how the entry in the `data.json` would look like this (there would be an actual ID instead of USER_SESSION_ID)
```json
{
    "USER_SESSION_ID": {
        "sender_id": "USER_SESSION_ID",
        "created_at": "CREATED_AT_DATE"
    }
}
```

Here's the example [gist](https://gist.github.com/sido378/4347a34260f3def303c37f0b4769ba67) by [sido378](https://github.com/sido378).

---

If all of this makes absolutely ***zero*** sense to you, then I'd recommend you check out this tutorial by [CodeAcademy](https://www.codecademy.com/learn/python)
<br> They have great tutorials for free that should teach you the basics of Python in a couple of hours.
