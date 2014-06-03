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


Execute the following two commands to add a user and group "icecast" since icecast refuses to run as root
```
echo "icecast:x:200:" >> /etc/group
```

```
echo "icecast:*:200:200:nobody:/var:/bin/ash" >> /etc/passwd
```
