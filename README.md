# Patching the Tangent Quattro Reciva radio
Notes how to patch the Reciva radios (Tangent Quattro etc.) to have internet radio access now after the Reciva was shut down.

Also some own notes how to do stuff with it :)

**What we can get**

- Telnet connection to the radio
- [Sharpfin](https://www.sharpfin.org/) installation to the radio
- Possible to do change presets and control radio using web browser
- [Internet radio plugin by David at megapico.co.uk](http://www.megapico.co.uk/sharpfin/mediaserver.html)

---
## Table of contents
- TODO

--- 
## Why?
I was googling around to buy Tangent Quattro radio for myself, as my parents' already had it. It's quite old, however quite a nice way to listen to some smooth jazz during breakfast. 

However, I found out that Reciva was going to close down and all those radios will stop working.

 [The Reciva Internet radio station aggregator is closing down](https://swling.com/blog/2020/11/the-reciva-internet-radio-station-aggregator-closing-down/).

The Recive was a service that aggregates internet radio stations, there was over 17k of radio channels to select from.

I managed to install Sharpfin and some plugins to the radio. After that, it's possible to change presets (1...6) by using web browser. It's also possible to add own station lists.


## Hacking

I also tried to decompile (read: waste my spare time) and look around in the radio, as it's Linux based. I found out that there are some TEST MODE stuff with fake station list. However havent got any further yet. Some discussion here: [http://iradioforum.net/forum/index.php?topic=2968.0](http://iradioforum.net/forum/index.php?topic=2968.0)



---


## Step 1 - Installing Sharpfin (patching the radio)

The radio is configured to connect to our PC instead of Reciva server when updating firmware.

**Please download this repository as zip or clone to your PC to access required files.**

### IP configuration
1. Connect the radio to the same LAN or WLAN network that your computer is connected.

2. Find out the IP address of your PC
    * Windows: Hit Windows+R on keyboard, write cmd and write `ipconfig /all`
    *  My computer IP was 192.168.5.16 (used later)

3. Open network configuration in Radio 
    * `Menu -> Configure -> Network Config -> Edit Config`

4. Edit network settings
    * Disable DHCP
    * IP Address
      - Enter any valid IP from your network
      - In this example, PC was `192.168.5.16` and radio will be `192.168.5.25`
    * Netmask
      - Same as you can see in `ipconfig`
      - Usually `255.255.255.0`
    * Gateway 
      - Same as you can see in `ipconfig` (usually your router IP)
      - In this example `192.168.5.1`
    * DNS server (1)
      - Enter your PC IP
      - In this example `192.168.5.16`
    * Enter DNS #2?
      - NO
      

### Firmware update / patching
1. Open `files/RecivaServer/server.exe` as Administrator
    * Awesome tool by [dliw](http://iradioforum.net/forum/index.php?topic=2968.0) to patch the radio
    * Source: [http://iradioforum.net/forum/index.php?topic=2968.0](http://iradioforum.net/forum/index.php?topic=2968.0)
    * Why administrator? Using the DNS port 54 requires admin privileges

2. Enter following settings
    * Patch file
      - `RecivaServer\patches\sharpfin-base_0.3.patch``
    * DNS server
      - `8.8.8.8` (Google)
      - Note: The textbox is empty at start (it only has a placeholder 8.8.8.8)
    * Own IP
      - Your PC IP used in previous steps
      - In this example `192.168.5.16`

![image](https://user-images.githubusercontent.com/13457157/120915774-56ee5080-c6ae-11eb-9e6e-f2c467ff4534.png)


8. Press Start button > The server starts listening

```
gui: trying to determine IP.
dns: server is running...
gui: servers started...
```
    

9. In radio, navigate to 
    * `Menu -> Configure -> Upgrade Firmware`

10. The Reciva Pather should display some info in log similar to
```
dns: query for p1.h1.uk.reciva.com
dns: query for p1.h1.uk.reciva.com
dns: query for www.reciva.com
dns: query for u1.ext.h2.west.us.reciva.com
dns: redirected to local http server.
http: got request.
server: patch info requested.
dns: query for p1.h1.uk.reciva.com
dns: query for 17.5.168.192.in-addr.arpa
dns: query for 17.5.168.192.in-addr.arpa
```

10. Answer `YES` to confirmation

11. The radio should now begin to do a firmware update
    * **NOTE:** Sometimes the first firmware upgrade does nothing (the radio only reboots). Just do it again, until the following stuff happens:

TODO

*Tangent Quattro screen when (successful) patching ongoing* 

![image](https://user-images.githubusercontent.com/13457157/120915929-42f71e80-c6af-11eb-8a25-a68170f18301.png)

*Reciva Pather log when patching*

12. After update the radio will reboot automatically.

13. Change radio IP settings back to normal, so the internet connection works
    * Probably/usually just `Auto (DHCP)` to `YES`

14. Check your radios new IP address by navigating to 
    * `Menu -> Configure -> Network Config -> View Config -> IP Address`
    * In this example it's `192.168.5.13`


13. Open web browser and navigate to your radio IP. You should see Sharpfin frontpage

![image](https://user-images.githubusercontent.com/13457157/120916088-27404800-c6b0-11eb-9811-6a3bacb1f652.png)

14. Go `Admin Home -> Install addon`

15. Enter one of the following URL to the text box
    * 
