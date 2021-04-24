---
name: SpotyStation
tools: [HomeAssistant, NodeRed, ESP32, cpp]
image: https://github.com/wesleygas/NetworkDisplay32/raw/main/display_layout.jpg
description: A way to display what media is being played anywhere in your house 
---

# SpotyStation


First of all, I apologise for my terrible creativity for names 😁. But the inspiration is
still there! It's a station to show whatever is currently playing on your spotify (or
any other media player on HomeAssistant)

--------------

To start, the actual code for displaying image data on the ESP32 is based on my [Networkdisplay32](https://github.com/wesleygas/NetworkDisplay32). You can start by cloning that repository, filling your
wifi credentials on the `platformio.ini` and uploading like any other platformIO project

After that, the ESP will connect to your network and start listening on the two routes listed on the
repository readme and the only thing needed is a way to get what is being listened to, extracting the
album cover and sending it to the display. I will be using NodeRed for that.

## Getting the image to display

 First of, I really wanted to know WTH was that song that Spotify just started playing. For that, 
 I needed to connect my Spotfy account to HomeHssistant. Luckly, there is already an integration
 that does just that right [here](https://www.home-assistant.io/integrations/spotify/)

 After that is done and configured, I created a NodeRed flow that would trigger every time the media
 being played changed, sending it to two subflows that will be in charge of converting the image, downscaling it to the 128x128 screen size limit and downsampling it to decrease the filesize.
 
![nodered-flow](https://github.com/wesleygas/NetworkDisplay32/blob/main/nodered_flow.jpg?raw=true)
 
  You can get the export of that flow [here](https://raw.githubusercontent.com/wesleygas/home-assistant-config/main/spotify_chromecast_display.json).

Let's finish with a short [demo video](https://youtu.be/3ns0LNHMiB0).