#### Getting started
You can either download the packages yourself, or download an archive from my GitHub page, the ones you need are listed in the attached text file. I recommend downloading the archive, unzipping it and placing it on the USB-drive before connecting the drive to your PirateBox and powering it up... it contains all the needed packages as well as basic configuration files for icecast and ices
packages needed:
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
There's no way all this is going to fit on rootfs, but luckily we have access to external storage on the USB-drive! Let's install these packages there with opkg install -d ext PACKAGES

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
echo "icecast:*:200:200:nobody:/var:/bin/ash" >> /etc/passwd
```

At this point you should take a look at the supplied ices.xml and icecast.xml config files. If you want to keep the default settings you don't need to do anything, but if you don't want to store the logs in ```/mnt/usb/log``` you need to change the log path in both files. Also, you might want to change the password for the icecast server, but that's up to you.

In ices.xml, look for the following section:
``` xml
    <input>
      <param name="type">basic</param>
      <param name="file">/mnt/usb/playlist1.txt</param>
      <param name="random">1</param>
      <param name="once">0</param>
      <param name="restart-after-reread">1</param>
    </input>
```
See where it says playlist? Guess what that line does. Correct! It tells ices where to find the playlist you want it to stream. You can place the playlist anywhere you want, it's just a textfile with a list of absolute paths to the music you want to stream. If you don't have a specific place you want it, just keep the default setting. 

The rest of the file is basically just giving your pirate radio channel a name, description, genre etc. Nothing you need to change unless you absolutely want to. 

#### Add some tunes
Now we're almost done! But we need to add some music! If you don't have any .ogg files at hand, you can download an awesome CC-licensed song which I've converted to .ogg here https://mega.co.nz/#!8skFTBRB!1ZB7himQzPXxnHs5MXtzVg4_UPir4COQXDQNhCCohUI 
Original can be found here http://www.jamendo.com/en/artist/352184/conway-hambone and license here https://creativecommons.org/licenses/by-sa/3./0 
Place it in ```/mnt/usb/PirateBox/Shared``` since you want to share it with the world. Now run the following command which will find all .ogg files and list their locations in playlist1.txt:
```
find /mnt/usb -name "*.ogg" > /mnt/usb/playlist1.txt
```
If you've decided to place your placelist somewhere else, or named it differently, adjust accordingly (the ices.xml file... remember?)

#### Let's stream some music!
Now you've got about 10 seconds left of work to do before you can listen to some sweet, sweet locally streamed music.
First, start the icecast server and ices client with the following commands:
```
icecast -b -c icecast.xml
ices ices.xml
```
That's it! Now you can use e.g. VLC to open the network stream http://192.168.1.1:8000/one.ogg 
Now relax, lean back, and enjoy the melodies


Any problems, questions, complaints, interesting new profanities or anything else, drop me a PM on the forum, shoot me a mail at piratebox@crossingrubicon.eu or light a fire and hope that I notice your smoke signals. 


