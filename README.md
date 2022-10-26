# LED changes color after time passes

- Gemaakt door Adam el Ghareib | 500849066
- 26 october 2022

# Introduction

In this tutorial you will learn how to make your LED change color after time passes.

# Required hardware components

- 1x Arduino Board (ESP8266 Development Board)
- 1x LED

# Step 1: Install the NTP (Network Time Protocol) library

Install the NTP library. In your Arduino IDE, go to Sketch > Library > Manage Libraries.
Search for NTPClient and install the library by Fabrice Weinberg as shown in the image below.

<img src="/images/Library.webp">

# Step 2: Upload & Verify the code 

Copy and paste the code down below.

~~~
/*
  Rui Santos
  Complete project details at https://RandomNerdTutorials.com/esp8266-nodemcu-date-time-ntp-client-server-arduino/
  
  Permission is hereby granted, free of charge, to any person obtaining a copy
  of this software and associated documentation files.
  
  The above copyright notice and this permission notice shall be included in all
  copies or substantial portions of the Software.
*/

#include <ESP8266WiFi.h>
#include <NTPClient.h>
#include <WiFiUdp.h>

// Replace with your network credentials
const char *ssid     = "REPLACE_WITH_YOUR_SSID";
const char *password = "REPLACE_WITH_YOUR_PASSWORD";

// Define NTP Client to get time
WiFiUDP ntpUDP;
NTPClient timeClient(ntpUDP, "pool.ntp.org");

//Week Days
String weekDays[7]={"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"};

//Month names
String months[12]={"January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"};

void setup() {
  // Initialize Serial Monitor
  Serial.begin(115200);
  
  // Connect to Wi-Fi
  Serial.print("Connecting to ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

// Initialize a NTPClient to get time
  timeClient.begin();
  // Set offset time in seconds to adjust for your timezone, for example:
  // GMT +1 = 3600
  // GMT +8 = 28800
  // GMT -1 = -3600
  // GMT 0 = 0
  timeClient.setTimeOffset(0);
}

void loop() {
  timeClient.update();

  time_t epochTime = timeClient.getEpochTime();
  Serial.print("Epoch Time: ");
  Serial.println(epochTime);
  
  String formattedTime = timeClient.getFormattedTime();
  Serial.print("Formatted Time: ");
  Serial.println(formattedTime);  

  int currentHour = timeClient.getHours();
  Serial.print("Hour: ");
  Serial.println(currentHour);  

  int currentMinute = timeClient.getMinutes();
  Serial.print("Minutes: ");
  Serial.println(currentMinute); 
   
  int currentSecond = timeClient.getSeconds();
  Serial.print("Seconds: ");
  Serial.println(currentSecond);  

  String weekDay = weekDays[timeClient.getDay()];
  Serial.print("Week Day: ");
  Serial.println(weekDay);    

  //Get a time structure
  struct tm *ptm = gmtime ((time_t *)&epochTime); 

  int monthDay = ptm->tm_mday;
  Serial.print("Month day: ");
  Serial.println(monthDay);

  int currentMonth = ptm->tm_mon+1;
  Serial.print("Month: ");
  Serial.println(currentMonth);

  String currentMonthName = months[currentMonth-1];
  Serial.print("Month name: ");
  Serial.println(currentMonthName);

  int currentYear = ptm->tm_year+1900;
  Serial.print("Year: ");
  Serial.println(currentYear);

  //Print complete date:
  String currentDate = String(currentYear) + "-" + String(currentMonth) + "-" + String(monthDay);
  Serial.print("Current date: ");
  Serial.println(currentDate);

  Serial.println("");

  delay(2000);
} 
~~~

Fill in your SSID and password.

~~~
const char *ssid     = "REPLACE_WITH_YOUR_SSID";
const char *password = "REPLACE_WITH_YOUR_PASSWORD";
~~~

Go to line 50 and change your setTimeOffset from "0" to "7200" to match Amsterdam's timezone.

~~~
timeClient.setTimeOffset(7200);
~~~

Upload and verify the code to see if it works, in the picture down below is how it should look.
<img src="/images/Result1.png">

# Step 3: Add a loop

Add on line 105 the code below and change the "currentHour == 13" to the current time in your timezone.

~~~
if (currentHour == 15) {
    Serial.println("15:00");
} else {
    Serial.println("It's not 15:00");
}
~~~

Upload and verify the code to see if it works, in the picture down below is how it should look.
<img src="/images/Loop.png">

# Step 4: Check the feed

Now its time to make your LED shine. Paste this code below on line 16.

~~~
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> 
#endif

#define PIN D5 
#define NUMPIXELS 18 

Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

#define DELAYVAL 500 
~~~

Paste this code below on line 65 on the bottom of your "void setup".
~~~
#if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
  clock_prescale_set(clock_div_1);
#endif

  pixels.begin();
~~~

Now you gonna change your void loop from this:
~~~
if (currentHour == 15) {
    Serial.println("15:00");
} else {
    Serial.println("It's not 15:00");
}
~~~

To this:
~~~
if ( currentHour == 15) {
    Serial.println("15:00");
    for(int i=0; i<NUMPIXELS; i++) { 
    
    pixels.setPixelColor(i, pixels.Color(0, 150, 0));

    pixels.show();

    delay(DELAYVAL); 
  }
  } else {
    for(int i=0; i<NUMPIXELS; i++) {
    
    pixels.setPixelColor(i, pixels.Color(150, 150, 0));

    pixels.show();

    delay(DELAYVAL);
  }
    Serial.println("Loser");
  }
~~~

Basically what this code is saying, when its 15:00 the LEDS turn green. When it's not 15:00 so 14:00 or past 16:00 o'clock then your LED light will turn yellow. That's how you make your LED light change to an specific color each hour.

# Errors:

There is one error I encountered. It was very weird and I couldn't exactly determine where it came from. I think my ESP8266 file got corrupted but even deleting that didn't work. So I deleted and reinstalled Arduino which fixed my problem eventually.

<img src="/images/Error2.png">



# Sources:

https://randomnerdtutorials.com/esp8266-nodemcu-date-time-ntp-client-server-arduino/ <br>
https://www.arduinolibraries.info/libraries/ntp-client
