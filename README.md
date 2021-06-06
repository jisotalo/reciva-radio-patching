# Patching the Tangent Quattro Reciva radio
Notes how to patch the Reciva radios (Tangent Quattro etc.) to have internet radio access now after the Reciva was shut down.


Also some own notes how to do stuff with it :)

**What we can get**

- Telnet connection to the radio
- [Sharpfin](https://www.sharpfin.org/) installation to the radio
- Possible to do change presets and control radio using web browser
- [Internet radio plugin by David at megapico.co.uk](http://www.megapico.co.uk/sharpfin/mediaserver.html)

![image](https://user-images.githubusercontent.com/13457157/120917030-ab48fe80-c6b5-11eb-8bb4-8c1988ffc9cd.png)

---
## Table of contents
- [Patching the Tangent Quattro Reciva radio](#patching-the-tangent-quattro-reciva-radio)
  * [Table of contents](#table-of-contents)
  * [Why?](#why-)
  * [Hacking](#hacking)
  * [Step 1 - Installing Sharpfin (patching the radio)](#step-1---installing-sharpfin--patching-the-radio-)
    + [IP configuration](#ip-configuration)
    + [Firmware update / patching](#firmware-update---patching)
    + [Updating the Sharpfin to 2.0](#updating-the-sharpfin-to-20)
    + [Installing Sharpfin streaming media server by David at megapico.co.uk](#installing-sharpfin-streaming-media-server-by-david-at-megapicocouk)
  * [Managing presets](#managing-presets)
    + [Adding a new preset to empty slot](#adding-a-new-preset-to-empty-slot)
    + [Updating an existing preset](#updating-an-existing-preset)
  * [Telnet connection](#telnet-connection)
  * [Disabling radio write protect](#disabling-radio-write-protect)
  * [Editing configuration](#editing-configuration)
  * [Links](#links)

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

      ![image](https://user-images.githubusercontent.com/13457157/120916753-fd892000-c6b3-11eb-99b4-e9697855ec5f.png)

    * IP Address
      - Enter any valid IP from your network
      - In this example, PC was `192.168.5.16` and radio will be `192.168.5.25`

      ![image](https://user-images.githubusercontent.com/13457157/120916768-0c6fd280-c6b4-11eb-82c9-9cd3d51bc788.png)

    * Netmask
      - Same as you can see in `ipconfig`
      - Usually `255.255.255.0`

      ![image](https://user-images.githubusercontent.com/13457157/120916785-227d9300-c6b4-11eb-8f16-b3e10b494a3c.png)

    * Gateway 
      - Same as you can see in `ipconfig` (usually your router IP)
      - In this example `192.168.5.1`

      ![image](https://user-images.githubusercontent.com/13457157/120916800-32957280-c6b4-11eb-83e0-273bf69c5d9f.png)

    * DNS server (1)
      - Enter your PC IP
      - In this example `192.168.5.16`

      ![image](https://user-images.githubusercontent.com/13457157/120916805-3cb77100-c6b4-11eb-9cf3-3a9e9c4be157.png)

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


![image](https://user-images.githubusercontent.com/13457157/120916814-548ef500-c6b4-11eb-9f6b-d8e9d0852e75.png)

11. The radio should now begin to do a firmware update
    * **NOTE:** Sometimes the first firmware upgrade does nothing (the radio only reboots). Just do it again, until the following stuff happens:

![patching](https://user-images.githubusercontent.com/13457157/120916963-5311fc80-c6b5-11eb-8fb7-37872f63f683.gif)

*Tangent Quattro screen when (successful) patching ongoing* 

![image](https://user-images.githubusercontent.com/13457157/120915929-42f71e80-c6af-11eb-8a25-a68170f18301.png)

*Reciva Pather log when patching*

12. After update the radio will reboot automatically.

13. Change radio IP settings back to normal, so the internet connection works
    * Probably/usually just `Auto (DHCP)` to `YES`

14. Check your radios new IP address by navigating to 
    * `Menu -> Configure -> Network Config -> View Config -> IP Address`
    * In this example it's `192.168.5.13`

    ![image](https://user-images.githubusercontent.com/13457157/120916987-6fae3480-c6b5-11eb-862b-a41c15855d85.png)

13. Open web browser and navigate to your radio IP. You should see Sharpfin frontpage

![image](https://user-images.githubusercontent.com/13457157/120916088-27404800-c6b0-11eb-9811-6a3bacb1f652.png)



### Updating the Sharpfin to 2.0

1. Go `Admin Home -> Install addon` at the web browser

2. Enter one of the following URL to the text box (if first doesnt work, try to second)
* 
    * http://www.megapico.co.uk/sharpfin/sharpfin-ep2.install
    * http://github.com/jisotalo/reciva-radio-patching/raw/main/files/sharpfin-ep2.install

![image](https://user-images.githubusercontent.com/13457157/120916295-83579c00-c6b1-11eb-9f4e-b55bfa9d01c0.png)

3. Press `Download Install Script` and then `Click Here to Do the Install` at the end of the page

4. Sharpfin will be updated, status should be shown on the radio screen. The radio will reboot.

5. Refresh the web page. It should be updated to newer version.

![image](https://user-images.githubusercontent.com/13457157/120916354-d4679000-c6b1-11eb-8e0e-5d678dc0cb88.png)


### Installing Sharpfin streaming media server by David at megapico.co.uk


More info at [http://www.megapico.co.uk/sharpfin/mediaserver.html](http://www.megapico.co.uk/sharpfin/mediaserver.html)

1. Navigate to `ADMINhome -> Install Addon`

2. Enter one of the following URL to the text box (if first doesnt work, try to second)
* 
    * http://www.megapico.co.uk/sharpfin/Sharpfin-media-server-1.0.install
    * http://github.com/jisotalo/reciva-radio-patching/raw/main/files/Sharpfin-media-server-1.0.install

3. Press `Download Install Script` and then `Click Here to Do the Install` at the end of the page

4. Done. See more info about this at [http://www.megapico.co.uk/sharpfin/mediaserver.html](http://www.megapico.co.uk/sharpfin/mediaserver.html)



## Managing presets

Now that the radio has Sharpfin installed, you can listen to internet radios by editing presets using web browser.

### Adding a new preset to empty slot

1. Navigate to your radio using web browser

2. Press Store under presets

![image](https://user-images.githubusercontent.com/13457157/120916520-be0e0400-c6b2-11eb-882a-15eb3da5a51d.png)

3. Press 1-20

4. Press edit (pen) button under preset you want to modify.

5. Edit settings and press Store.

6. Reboot radio


### Updating an existing preset

1. Navigate to your radio using web browser

2. Press Manage under presets

![image](https://user-images.githubusercontent.com/13457157/120916484-9028bf80-c6b2-11eb-9530-049290a9ce3f.png)

3. Enter settings and press store
    * Swiss Jazz
    * http://stream.srg-ssr.ch/m/rsj/mp3_128
    * 1

![image](https://user-images.githubusercontent.com/13457157/120916555-f7467400-c6b2-11eb-946e-1286631c5410.png)


4. Reboot radio

5. After radio has started, press button 1 -> Swiss Jazz starts playing.


## Telnet connection

After installing Sharpfin, you can connect to the radio using telnet.
- IP: *Radio IP*
- Port: 23
- Username: admin
- Password: admin


![image](https://user-images.githubusercontent.com/13457157/120917317-48586700-c6b7-11eb-8682-fbd4d650f3ab.png)


## Disabling radio write protect

Using telnet, you can modify any files you want. However, the radio has a write protect that restores everything after reboot.

You can disable the write protect, do changes and then enable it again.

The following is from https://www.sharpfin.org/index.php/Config.txt_File

- Log into the radio using telnet (user & password = admin)
- Issue the command `mount / -orw,remount` to unprotect the partition
- **Do whatever you like**
- Write protect the drive with `sync;mount / -oro,remount`
- Reboot the radio

## Editing configuration


Use with your own risk. See https://www.sharpfin.org/index.php/Config.txt_File

- Log into the radio using telnet (user & password = admin)
- Look in(`chdir /root/hwconfig/`) /root or /root/hwconfig, depending on the current firmware version of your radio
- Find your configxxxx.txt file (The xxxx is the Hardware Id, which you can find in `Configure / Version`)
- Look in it and others in the same directory to work out what modifications you wish to make (such as enabling alarms)
- Issue the command `mount / -orw,remount` to unprotect the partition
- Edit the configxxxx.txt file with `vi configxxxx.txt`
- `i` to enter Insert mode
- Double Check your edits
- Now treble check them!
- Hit <ESC> to leave Insert Mode and enter Command mode
- `:w<ENTER>` To write current file (if you are sure and wish to save it)
- `:q!<ENTER>` To exit `vi`
- Write protect the drive with `sync;mount / -oro,remount`
- Reboot the radio


## Links

Just some links I have saved

- https://www.sharpfin.org/index.php/Main_Page
- https://www.sharpfin.org/index.php/Config.txt_File
- http://iradioforum.net/forum/index.php?topic=193.0
- http://www.g3gg0.de/wordpress/reversing/reciva-encryption-on-reciva-barracuda/
- https://web.archive.org/web/20110808021251/http://copper.reciva.com/sources/
- http://internetradiohack.blogspot.com/2006/
- http://internetradiohack.blogspot.com/2007/
- https://www.sharpfin.org/index.php?title=Enabling_Login
- http://www.megapico.co.uk/sharpfin/mediaserver.html
- http://iradioforum.net/sharpfin/