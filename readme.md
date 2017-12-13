# Project New World (PNW) #

## Setting up your machine ##

You must be running a 64-bit Linux distribution (Google recommends using 
[Ubuntu](http://www.ubuntu.com/download/desktop) for this).
You also must have installed some packages to build PNW.

### To build PNW, you’ll need: ###

```bash
sudo apt-get update && sudo apt-get install bc bison build-essential curl flex g++-multilib
gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev
libesd0-dev liblz4-tool libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-dev libxml2
libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```

#### For Ubuntu versions older than 16.04 (xenial), substitute: ####

```bash
libwxgtk3.0-dev → libwxgtk2.8-dev
```

### Java ###

PNW requires OpenJDK 1.8 [How do I install OpenJDK 1.8?](https://wiki.ubuntuusers.de/Java/Installation/OpenJDK/)

* (Note: Some linux distribution already have this java version included.)

## Getting the source ##

[Repo](http://source.android.com/source/developing.html) is a tool provided by Google that
simplifies using [Git](http://git-scm.com/book) in the context of the Android source.

### Installing Repo ###

```bash
# Make a directory where Repo will be stored and add it to the path
$ mkdir -p ~/.bin
$ PATH=~/.bin:$PATH

# Download Repo itself
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo

# Make Repo executable
$ chmod a+x ~/.bin/repo
```

### Initializing Repo ###

```bash
# Create a directory where the source will be stored.
# Note: You can name the folder to whatever you want, just replace *PNW*
# with the name you intended  to use
$ mkdir -p ~/PNW
$ cd ~/PNW

# Install the PNW Repo in the created directory
$ repo init -u git://github.com/ProjectNewWorld/android.git -b oreo
```

### Download the source code ###

The [repo sync](https://source.android.com/source/downloading) command is used to update
the latest source code from PNW and Google. Remember it, as you may want to do it 
every few days to keep your code base fresh and up-to-date.

```bash
# This may take a while, depending on your internet speed.
#
# The -j# option specifies the number of concurrent download threads to run.
# 4 threads is a good number for most internet connections.
# You may need to adjust this value if you have a particularly slow connection.
$ repo sync --force-sync -j4
```

## Building ##

### Device-specific code ###

To build PNW you’ll need an existing device tree, kernel and proprietary blobs. 
If your device already receive official LineageOS builds you can grab the device tree 
and kernel here [LineageOS Github](https://github.com/LineageOS) and the proprietary 
blobs here [TheMuppets Github](https://github.com/themuppets).

### Speed up the build ###

Make use of [ccache](https://ccache.samba.org/) if you want to speed up subsequent
builds by running:

```bash
$ echo "export USE_CCACHE=1" >> ~/.bashrc
$ ~/PNW/prebuilts/misc/linux-x86/ccache/ccache -M 50G
```

* Where 50G corresponds to 50GB of cache. This needs to be run once. Anywhere from
25GB-100GB will result in very noticeably increased build speeds (for instance, a typical 
1hr build time can be reduced to 20min). If you’re only building for one device,
25GB-50GB is fine. If you plan to build for several devices that do not share the 
same kernel source, aim for 75GB-100GB. This space will be permanently occupied 
on your drive, so take this into consideration.See more information about ccache 
on Google’s [Android build environment initialization page](https://source.android.com/source/initializing.html#setting-up-ccache).

### Last step ###

```bash
$ cd ~/PNW
$ . build/envsetup.sh && brunch lineage_*my-device*-userdebug
```

## Success! What’s next? ##

You’ve done it! Now you can share your builds wherever you want.
(Note: If you have done more then 5 stable builds for your device you have the
chance to become an official maintainer from PNW for your device.
Join our PNW Support group on [Telegram](https://t.me/NucleaRomSupport)
for questions related to this or other topics.)
