---
title: WiFi Deauthentication Attack with ESP32 Wroom
author: prasaanth
date: 2025-03-07 11:33:00 +0800
tags: [WiFi, IoT, Reverse, Engineering]
categories: [WiFi, Reverse Engineering,IoT Security]
---

#### WiFi Deauthentication [802.11]

<img class="source_images" src="/assets/img/esp32_NODEMCU_cp21-550x550w.png" alt="Screenshot of the htop process folder in Linux terminal">

How does the `WiFi deauthentication` works basically some people are confusing with jamer and the deauthentication for this wifi
deauthentication is nothing an device send deauth signal to client devices for example we will take the `esp32` deauth tool form 
will not only see just simply downloading the publicly available deauth firmware that are available but that is easy thing right 
just download and flash it and it will simple deauth but today that is not our objective we will learn about how does the deauth 
what is principal behind that and we will see about that.

#### 802.11 frame types

For this we need to understand the frames is primary part the wifi signals for authentication and connection and control data so
many things where involved in this frames for now we will see about deauth frame it is an raw 802.11 frame so every action it has it
own frames so for deauthentication also we have an frame that's why attacker uses this frame to disconnect from the wifi

Example Deauth Frames

```cpp
uint8_t deauthPacket[26] = {
    0xC0, 0x00,  
    0x3A, 0x01,  
    0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF,  // Brodcast to all devices
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // Source MAC
    0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // BSSID 
    0x00, 0x00,  
    0x01, 0x00   
};
```


#### How it works
For this we will take the `esp32 wroom` it has its own built-in wifi so we can use that to execute this first we need to put our
wifi card in monitor mode with help that we can able to sniff all the nearby access points and what are devices where
connected to that even interacting with that network `RFMON` we can sniff all things about that network by using this 
feature the attacker crafts and well structured deauthorize with thatand we will get `SSID` of the access point and device
`BSSID` (MAC ADDRESS) with two of we can construct the deauth frame and send the frames in `WPA` and `WPA2` does not have `PMF` 
Protected management frames so that is why this is possible but it does not mean that `WPA3` is secure everything has it own
weakness until we figure it out when the attacker sends deauth beacons with spoofed Access point deauth frame 
the client was not validating whether it is spoofed or not so that's why it was disconnecting this how it works

#### Libary level lock


In this blog we talk about the esp32 wroom for beginners does not when just grab some online tools and put that in in esp32 wroom 
magically it deauth the wifi access point but that is not an easy thing in that we have firmware level restrictions on that like 
for that we need to send raw 802.11 frames to the access point in esp32 wroom wifi.h have some library but it was locked at some point  become so frustrated
ever well-constructed frame the something on the low level that was blocking after much research I figured out expressif has 
blocked that feature and I tried some online patched lib and put it into my project it didn't work so I decided  let's patch our library 
at that time only my reverse engineering skills where helped me after some research on which function was blocking that and checking 
<img class="source_images" src="/assets/img/function1.png" alt="Screenshot of the htop process folder in Linux terminal">

so extracted the object file from that lib and patched the check there is a function called `ieee80211_raw_frame_sanity_check` this is 
where blocked me this function role is it will check any type malicious frame are being gone if in case any frame send it will 

<img class="source_images" src="/assets/img/afterpatchfn.png" alt="Screenshot of the htop process folder in Linux terminal">

it will return false so I simply patch the bin like just `return 0` so it will pass the sanity check so now I can send raw wifi because 
ever I want so I can now send a deauth frame without any restriction

<img class="source_images" src="/assets/img/inassembly.png" alt="Screenshot of the htop process folder in Linux terminal">