# Installing Hush

## Install

    echo 'deb https://dl.bintray.com/myhush/hush/ hush main' | sudo tee --append /etc/apt/sources.list.d/hush.list
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 379CE192D401AB61
    sudo apt-get update
    sudo apt-get install hush

## Create a HUSH configuration file (*important*):

```
mkdir -p ~/.hush
echo "rpcuser=username" >> ~/.hush/hush.conf
echo "rpcpassword=`head -c 32 /dev/urandom | base64`" >>~/.hush/hush.conf
echo "addnode=node.myhush.network" >> ~/.hush/hush.conf
echo "addnode=mmc01.madbuda.me" >> ~/.hush/hush.conf
echo "addnode=zdash.suprnova.cc" >> ~/.hush/hush.conf
echo "addnode=explorer.myhush.org" >> ~/.hush/hush.conf
echo "addnode=hushipv4.matthewreichardt.com" >> ~/.hush/hush.conf
echo "addnode=stilgar.leto.net" >> ~/.hush/hush.conf
```

## Download proving key

This will download a Zcash Sprout proving key, if you already have one on this system, you only need one
and this script will just verify that it is correct.

```
./zcutil/fetch-params.sh
```

# Building Hush from source

Building Hush from source will take some time but your efforts will be rewarded :)

## Requirements

You will need at least 4GB of RAM to build hush from git source, OR
you can enable a swap file. To enable a 4GB swap file on modern Linux
distributions:

    sudo fallocate -l 4G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile

You will need to have Git and a C++ compiler and libtool and a
a few other libraries, depending on your setup.

## Building

The following build process generally applies to Ubuntu (and similar) Linux
distributions. For best results it is recommended to use Ubuntu Linux 16.04
or later.

Build HUSH along with most dependencies from source by running

#### For Linux 
Get dependencies:
```{r, engine='bash'}

sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python \
      zlib1g-dev wget bsdmainutils automake
```

Make sure to create a HUSH configuration file as described above.

#### For Windows
Get dependencies:
```{r, engine='bash'}
sudo apt-get install \
      build-essential pkg-config libc6-dev m4 g++-multilib \
      autoconf libtool ncurses-dev unzip git python \
      zlib1g-dev wget bsdmainutils automake mingw-w64
```
Additionally curl, cmake are required.

## Downloading Git source repo, building and running Hush

#### For Linux
```{r, engine='bash'}
# pull
git clone https://github.com/MyHush/hush.git
cd hush
# fetch key
./zcutil/fetch-params.sh
# Build
./zcutil/build.sh -j$(nproc)
# Run a HUSH node
./src/hushd
```

#### For Windows
```{r, engine='bash'}
# pull
git clone https://github.com/MyHush/hush.git
cd hush
# fetch key
./zcutil/fetch-params.sh
# Build
./zcutil/build-win.sh -j$(nproc)
```
The exe files will save to `src` which you can then move to a windows machine.

## Supported Platforms

Currently Linux,  Windows and Mac OS X are our supported platforms. If you
are interested in porting Hush to a new operating system, we are interested
to talk with you.

We aim to support all platforms, but there is only so much time in the day.

Currently, any ARMv7 machine will not be able to build this repo, because the
underlying tech (zcash and the zksnark library) do not support that instruction
set.

This also means that RaspberryPi devices will not work, unless they have a
newer ARMv8-based Raspberry Pi.

