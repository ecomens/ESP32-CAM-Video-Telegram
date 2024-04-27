# ESP32-CAM-Video-Telegram
Record avi video on ESP32-CAM and send to Telegram on event or request

Apr 26, 2024
<h3>Click below for a new version</h3>

- will work without internet
 
- will save to sd card
 
- radu made for 4 second limelapse in very low light

- can access from local internet from http://192.168.4.1/ or with router http://192.168.1.99/
- local access to change ssids, or capture image, clip to telegram/sd, or local image to motioneye, browser, etc 
 
https://jameszah.github.io/ESP32-CAM-Video-Telegram/installer/README11

<h3> Previous Version</h3>
Oct 16, 2022

<h5>One Click Installer</h5>
https://jameszah.github.io/ESP32-CAM-Video-Telegram/installer/
<h5>Source Code</h5>
https://github.com/jameszah/ESP32-CAM-Video-Telegram/tree/main/v9.6/
<h5>Free coffee</h5> 
https://ko-fi.com/jameszah/
<br/>
<br/>
                    
<h4>Updates:</h4>

1.  Arduino 1.8.19 / esp32-arduino 2.0.4
2.  One-Click-Installer - you dont have to compile
3.  WiFiManager - set your ssid, password, Telegram id and BOT id during your first boot
4.  Timed Photos - set a timer in minutes 0->1440 to send photos to your telegram BOT
5.  Very Slow Video - runs at 2 sec per frame to get a 5 minute ? timelapse video to your telegram
6.  EPPROM - all your parameters are saved in EPROM, so it should boot back to the same configuration
7.  PSRAM - now checks your available PSRAM, so 2MB and 8MB modules should now work
8.  changes to wifiman timeouts - 5 minutes to connect to wifi, 5 minutes to resolve AP setup
9.  changes to timed picture reset - reset timer with /entim or /nn 
10.  add wifimanager option to select HD or VGA

--------------
<br/>
Jan 17, 2022  
<br/>  
ESP32-CAM-Video-Telegram_8.8x.ino --- Arduino 1.8.13 / esp32-arduino 1.6.0    <br/>   
ESP32-CAM-Video-Telegram_8.9x.ino --- Arduino 1.8.19 / esp32-arduino 2.0.2    <br/>     


<h2> Description </h2>

This program runs on a simple ESP32-CAM module without a SD card installed, and will respond to Telegram requests to take a photo or video, and then send it to the Telegram server to arrive at your phone or computer.  If you set up an active-high PIR or radar sesnsor to pull up Pin 13, then it will take a photo and a video of the event that set off the PIR, and then a video afterwards.  The video can be "fast" at 25 frames-per-second, "med" at 8 fps, or "slow" at 2 fps.  The video is recorded in the psram of the ESP32-CAM module, so there is only about 3MB of space available, which means about 60 bright outdoor frames, or 150 dull bland frames.  So the videos will be about 3 seconds, 10 seconds, and 40 seconds long.  As well, it is set to play the fast video at half speed, the medium video at realtime, and the slow video at 5x speed, so your 40 seconds video will play in timelapse over 10 seconds or so.  

You could adjust the quality parameters of the framesize to get more frames for longer videos.  It is currently set up for VGA (640x480) and quality 10 (good, not great), so you could switch to QVGA (320x240) and quality 20 to get many more frames with the 3MB buffer.  The ov2640 camera and drivers also double in speed below CIF (400x296) size.  (I think that still works?)

<h2> Demo </h2>

As seen on Telegram screen: <br>

<img src="./magpie.jpg">

Click below to see DoorCam....avi as seen on Youtube - the picture here is the first frame of the avi, which is one frame after the jpeg sent to Telegram or about 40ms.  It does not go to Youtube automatically - I just did that as I cannot play a video from github site.  But you can download it below.  Telegram only handles mp4 files inline, so the avi just appears as a file.

[![Click to see on Youtube](http://img.youtube.com/vi/x4JaLOefIWA/hqdefault.jpg)](http://www.youtube.com/watch?v=x4JaLOefIWA "Magpie from Youtube")
[![gif](https://media.giphy.com/media/Cm9930ondKHQihs1Zi/giphy.gif)]

The file as served by Telegram:<br>

https://github.com/jameszah/ESP32-CAM-Video-Recorder-samples/blob/master/ESP32-Cam-Telegram/DoorCam%202021-06-03%2014.47.52.avi
  
The file uploaded to Youtube - now converted to Youtube's formats:  
https://youtu.be/x4JaLOefIWA

Click to see the 25 fps in slow motion.<br>

[![Click to see on Youtube](http://img.youtube.com/vi/KBXaL-kRRFI/hqdefault.jpg)](http://www.youtube.com/watch?v=KBXaL-kRRFI "Robin from Youtube")
[![gif](https://media.giphy.com/media/xbRQQ1TvP1jWQHC2JV/giphy.gif)]

Click to see the 2 fps in 5x timeslapse motion.  This is very dull light, meaning the frames are small and you get over 1 minute at 2 fps played over 15 seconds, and even then it hit the 150 frames limit, rather than the 3MB limit.<br>

[![Click to see on Youtube](http://img.youtube.com/vi/Y0s_3u5maDU/hqdefault.jpg)](http://www.youtube.com/watch?v=Y0s_3u5maDU "Dull Timelapse")


<h2> Controls </h2>


If you put in your correct Telegram chat_id, you will get a message when the ESP32-CAM boots, and then the /start command will produce the following.  
You can ask for a photo or a clip (photo plus avi), or reconfigure the ESP32 for different clip lengths, and disable the PIR, or disable the "avi", which means it will just send the photo and skip the avi.

The 3MB clip can take a minute to send to the Telegram server, so you may want to shut if off at times.  I'm not sure how many 3MB avi's Telegram will store for you, or if they get annoyed if you send too many.  They also pile up on my Android phone, so you need to empty your Telegram history from time to time.

<img src="./telegram.jpg">

It defaults to PIR enabled, and avi enabled amd a "fast" 25fps 3 second video.

<h2> Compile Time Parameters </h2>

```
char ssid[] = "yourssid";         
char password[] = "yourssidpass"; 
String TIMEZONE = "GMT0BST,M3.5.0/01,M10.5.0/02";  
String chat_id = "1234567890";
#define BOTtoken "XXXXXXXXX:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

// see here for information about getting free telegram credentials
// https://github.com/witnessmenow/Universal-Arduino-Telegram-Bot
// https://randomnerdtutorials.com/telegram-esp32-motion-detection-arduino/
// find your timezone here 
// https://sites.google.com/a/usapiens.com/opnode/time-zones  
```

<h2> Hardware </h2>

Hardware looks similar to the photos, samples over here.  
Just remember it is gpio 13 with a big resistor (10 Kohm), active high PIR, and it makes sense to put a big capacitor between power and ground to cover currennt surges from the WiFi.

https://github.com/jameszah/ESP32-CAM-Video-Recorder

---

Jan 24, 2022 - question about bots

In response to an email query:  

A telegram bot is like a person receiving and replying to messages.  The bot is a simple receive/reply system.  When it boots it sends one message to the hard-coded owner of the bot (10 digit number in chat_id), and thereafter it responds to anyone in the telegram universe that sends a message.  Obviously a little esp32 cannot respond to millions of messages, but anyone can send it a message.  This line below grabs the identity of the person sending a message, and uses that identity to send a message back.  You could delete this line, and it would always send messages back your hard-coded identiy.  Or check if the caller is you or one of your friends, and then reply to them, or ignore them.

https://github.com/jameszah/ESP32-CAM-Video-Telegram/blob/87c9ffa764ef58648e143ca038c49ba51b8964bf/ESP32-CAM-Video-Telegram_8.9x.ino#L302


