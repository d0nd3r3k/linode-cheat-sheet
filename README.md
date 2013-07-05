# Linode Ubuntu 12.04 LTS Configuration
---
##Install NodeJs

```
sudo apt-get update
sudo apt-get install python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
```
[source](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)

## Intstall ffmpeg

Prepare your environment:

```
sudo apt-get update
sudo apt-get -y install build-essential checkinstall git libfaac-dev libgpac-dev \
  libjack-jackd2-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev \
  libsdl1.2-dev libtheora-dev libva-dev libvdpau-dev libvorbis-dev libx11-dev \
  libxfixes-dev texi2html yasm zlib1g-dev
```

Install x264:
```
cd ~/tmp/
git clone git://git.videolan.org/x264
cd x264
./configure --enable-static --disable-asm
make
sudo checkinstall --pkgname=x264 --pkgversion="3:$(./version.sh | \
  awk -F'[" ]' '/POINT/{print $4"+git"$5}')" --backup=no --deldoc=yes \
  --fstrans=no --default
```

Install ffmpeg:
````
cd ~/tmp/
git clone --depth 1 git://source.ffmpeg.org/ffmpeg
cd ffmpeg
./configure --enable-gpl --enable-libfaac --enable-libmp3lame --enable-libopencore-amrnb \
  --enable-libopencore-amrwb --enable-libtheora --enable-libvorbis \
  --enable-libx264 --enable-nonfree --enable-version3 --enable-x11grab
make
sudo checkinstall --pkgname=ffmpeg --pkgversion="5:$(date +%Y%m%d%H%M)-git" --backup=no \
  --deldoc=yes --fstrans=no --default
hash x264 ffmpeg ffplay ffprobe
````

[Source](https://ffmpeg.org/trac/ffmpeg/wiki/UbuntuCompilationGuide)

## Open UDP Ports via UFW

Start by installing uwf:  

```
sudo apt-get install ufw
```

Then you should enable ufw:

```
sudo ufw enable
```

Don't do my mistake and lock yourself out of ssh:

```
sudo ufw allow ssh
```

To allow ports and protocols:

```
sudo ufw allow 30010/udp
```


To check the ufw status simply:

```
sudo ufw status

Firewall loaded

To                         Action  From
--                         ------  ----
22:tcp                     DENY    192.168.0.1
22:udp                     DENY    192.168.0.1
22:tcp                     DENY    192.168.0.7
22:udp                     DENY    192.168.0.7
22:tcp                     ALLOW   192.168.0.0/24
22:udp                     ALLOW   192.168.0.0/24
```

[Source](https://help.ubuntu.com/community/UFW)

#Linode Debian GNU/Linux 6.0 Configuration
---

##Install Node JS (Latest)

###1. Remove previous installations

Find where Node is installed
```
which node
```
Returns
```
/path/bin/node
```
To remove everything related to your previous installation
```
rm -r bin/node bin/node-waf include/node lib/node lib/pkgconfig/nodejs.pc share/man/man1/node.1
```

[source](http://stackoverflow.com/questions/5650169/uninstall-node-js-using-linux-command-line)

###2. Install Latest NodeJs

Compile node as root:
```
apt-get install python g++ make
mkdir ~/nodejs && cd $_
wget -N http://nodejs.org/dist/node-latest.tar.gz
tar xzvf node-latest.tar.gz && cd `ls -rd node-v*`
./configure
make install
```

[source](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)

##Install MongoDB (Latest)

Configure Package Management System (APT)
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
```

Create 10gen.list
```
sudo nano /etc/apt/sources.list.d/10gen.list
```
and add the following
```
deb http://downloads-distro.mongodb.org/repo/debian-sysvinit dist 10gen
```
Save and Exit

Now issue the following command to reload your repository
```
sudo apt-get update
```

Install MongoDB
```
sudo apt-get install mongodb-10gen
```

### Controlling MongoDB

Start mongod:
```
sudo /etc/init.d/mongodb start
```

Stop mongod:
```
sudo /etc/init.d/mongodb stop
```

Restart mongod:
```
sudo /etc/init.d/mongodb restart
```

[source](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-debian/)




















