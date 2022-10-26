# Send a signal from your NodeMCU to a different NodeMCU (Through the internet)

- Gemaakt door Adam el Ghareib | 500849066
- 26 october 2022

# Introduction

In this tutorial you will learn to send a signal from your NodeMCU to a different NodeMCU.

# Required hardware components

- 1x Arduino Board (ESP8266 Development Board)
- 1x Arduino Button

# Step 1: Creating and sharing a feed

In order to create a feed you have to use Adafruit.io.
Once you've created an account on Adafruit, you have to navigate to the "Feeds" tab at the top.
After clicking the tab, you should see a pop up like in the image shown below:

<img src="/imagesiot/create_feed.png">

In the pop up, give your feed a name and select "create"

In order to share your feed, you first have to go over to the share tab as shown in the image below:

<img src="/imagesiot/feed_sharing.png">

Once you've clicked this option, a pop up should appear prompting you to enter an email or username of the person you'd like
to share the feed with.

<img src="/imagesiot/feed_sharing_2.png">

Once you've filled in the email or username of the person you want to share your feed with you can also determine their
privileges within the feed by clicking the dropdown menu.

<img src="/imagesiot/feed_sharing_3.png">

# Step 2: Install the button

Installing the button goes as follows. First open Arduino. Once you've done that, navigate to examples > Adafruit IO > Adafruitio_06_digital.in
Once you have the example file open, fill in your adafruit io username and key (you can aquire these by clicking on the yellow key when in adafruit io)
Afterwards, fill in your wifi SSID and your wifi password. Normally this should work fine however, in our case we were unable to succesfully
establish a connection as shown in the image below:

<img src="/imagesiot/connection_error.png">

# Step 3: Upload & Verify the code

Now you can upload and verify the code! Check the serial monitor to see if you're connecting to the WiFi.

# Step 4: Check the feed

If your code uploaded succesfully you should now see feedback in the feed you created earlier

# Errors:

Problem:
<img src="/imagesiot/connection_error.png">



# Sources:

https://randomnerdtutorials.com/esp8266-nodemcu-date-time-ntp-client-server-arduino/
