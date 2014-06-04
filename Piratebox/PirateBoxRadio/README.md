---BEGIN IMPORTANT---

This repo contains everything needed to get ices and icecast working on your TP-Link MR3020 or WR703N PirateBox, turning it into a PirateBoxRadio.
You should consider this to be a pre-alpha, since I've just followed some tutorials for getting ices and icecast to work together and have basically no idea what they actually do.
I haven't done any testing over prolonged time periods to see if it's stable enough to keep on running for hours on end. I have only tested with two (2) connected clients so I don't know how many clients can connect and listen at the same time.
As of now, only .ogg files is playable... that means NO MP3s!!! I might try and get MP3 working, but I have no plans for it at the moment. I support open formats and standards, so I want my PirateBox to stream music that is free in every way.

---END---

For those of you who don't feel all that comfortable on the command line or when manually editing configuration files, or you don't want to spend the time setting it all up manually, there's a simple script that will set it all up for you. Just download the archive, extract it and place it on the USB-stick you use with your PirateBox.
The archive file contains all the needed packages, a setup script, a folder for storing logs and configuration files for ices and icecast. If you don't feel comfortable at the command line or by manually editing configuration files, just run the included setup script and you'll be good to go. This tutorial is meant for those who want to do the setup manually.

#### Getting started
First you need to install some packages. You can download them from this repo or directly from OpenWrt at http://downloads.openwrt.org/snapshots/trunk/ar71xx/packages/
You'll need the following packages (not listing complete names, only what's needed to locate them with CTRL+F):
- libshout
- ices
- libtheora
- libspeex
- alsa-lib
- kmod-sound-core
- kmod-input-core
- kernel_3.10
- icecast
- libxslt
- libxml2_2.9
- libcurl
- libpolarssl
- libvorbisidec
- librt

#### Installing needed packages
There's no way all this is going to fit on rootfs, but luckily we have access to external storage on the USB-drive. Assuming you've placed the packages in a folder "packages" Let's install them with ```opkg install -d ext packages/*```

You'll probably get the following error message at the end of the opkg-output, but just ignore it: 
```
 *pkg_run_script: package "kmod-sound-core" postinst script returned status 127 
 *opkg_configure: kmod-sound-core.postint returned 127
```

#### Configuring icecast and ices
Now that we've got everything installed, we can start configuring it. But first, let's add a user and group for the icecast server since it refuses to run as root
Execute the following two commands to add a user and group "icecast" 
```
echo "icecast:x:200:" >> /etc/group
```
```
echo "icecast:*:200:200:icecast:/mnt/usb:/bin/ash" >> /etc/passwd
```

At this point you should take a look at the supplied ices.xml and icecast.xml config files. If you want to keep the default settings you don't need to do anything, but if you don't want to store the logs in ```/mnt/usb/PirateBoxRadio/log``` you need to change the log path in both files. Also, you might want to change the password for the icecast server, but that's up to you.

In ices.xml, look for the following section:
``` xml
    <input>
      <param name="type">basic</param>
      <param name="file">/mnt/usb/PirateBoxRadio/playlist1.txt</param>
      <param name="random">1</param>
      <param name="once">0</param>
      <param name="restart-after-reread">1</param>
    </input>
```
See where it says playlist? Guess what that line does. Correct! It tells ices where to find the playlist you want it to stream. You can place the playlist anywhere you want, it's just a textfile with a list of full paths to the music you want to stream. If you don't have a specific place you want it, just keep the default setting. 

The rest of the file is basically just giving your pirate radio channel a name, description, genre etc. Nothing you need to change unless you absolutely want to. 

#### Add some tunes
Now we're almost done! But we need to add some music! If you don't have any .ogg files at hand, you can download an awesome CC-licensed song which I've converted to .ogg here https://mega.co.nz/#!8skFTBRB!1ZB7himQzPXxnHs5MXtzVg4_UPir4COQXDQNhCCohUI 
Original can be found here http://www.jamendo.com/en/artist/352184/conway-hambone and license here https://creativecommons.org/licenses/by-sa/3./0 
Place it in ```/mnt/usb/PirateBox/Shared``` since you want to share it with the world. Now run the following command which will find all .ogg files and list their locations in playlist1.txt:
```
find /mnt/usb -name "*.ogg" > /mnt/usb/playlist1.txt
```
If you've decided to place your placelist somewhere else, or named it differently, adjust accordingly (the ices.xml file... remember?)
Please note that if you only have one track in your playlist, ices will exit after finishing it with the default configuration. That's because it is configured to shuffle the playlist, and refuses to play the same track twice in a row. If you have two or more tracks, it will keep on playing them until you shut it down.

#### Let's stream some music!
Now you've got about 10 seconds left of work to do before you can listen to some sweet, sweet locally streamed music.
First, start the icecast server and ices client with the following commands (assumes you're in the folder where the configuration files are located. If not, adjust accordingly):
```
icecast -b -c icecast.xml
ices ices.xml
```
That's it! Now you can use e.g. VLC to open the network stream http://192.168.1.1:8000/one.ogg 
Now relax, lean back, and enjoy the melodies.


Any problems, questions, complaints, interesting new profanities or anything else, drop me a PM on the forum, shoot me a mail at piratebox@crossingrubicon.eu or light a fire and hope that I notice your smoke signals. 


