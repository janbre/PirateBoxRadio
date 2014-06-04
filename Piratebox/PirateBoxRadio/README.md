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

There's no way all this is going to fit on rootfs, but luckily we have access to external storage on the USB-drive! Let's install these packages there with opkg install -d ext PACKAGES

You'll probably get the following error message at the end of the opkg-output, but just ignore it: 
```
 *pkg_run_script: package "kmod-sound-core" postinst script returned status 127 
 *opkg_configure: kmod-sound-core.postint returned 127
```

Now that we've got everything installed, we can start configuring it. But first, let's add a user and group for the icecast server since it refuses to run as root
Execute the following two commands to add a user and group "icecast" 
```
echo "icecast:x:200:" >> /etc/group
```
```
echo "icecast:*:200:200:nobody:/var:/bin/ash" >> /etc/passwd
```

At this point you should take a look at the supplied ices.xml and icecast.xml config files. If you want to keep the default settings you don't need to do anything, but if you don't want to store the logs in /mnt/usb/log you need to change the log path in both files. Also, you might want to change the password for the icecast server, but that's up to you.

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

Now we're almost done. Last thing to do is add some music! 
