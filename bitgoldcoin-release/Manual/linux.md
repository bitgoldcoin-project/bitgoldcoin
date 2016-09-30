# [Compiling an bitgoldcoin daemon in Ubuntu Linux](https://dev.cryptolife.net/compiling-the-daemon-for-your-altcoin-in-ubuntu-linux/)


This guide will assist you in compiling a headless client (the daemon) in Ubuntu Linux. This is a necessary step if you wish to set up a node of your bitgoldcoin. It is assumed that you are starting on a fresh Ubuntu (14.04 x64) installation. If you require a VPS for hosting, I highly recommended Digital Ocean.

The $5/month plan is all you need. Use sudo in front of all commands if you’re not logged in as root.

Set up a swapfile if your system has less than 1.5GB of memory:

```
sudo fallocate -l 2G /swapfile
sudo chown root:root /swapfile
sudo chmod 0600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

If fallocate doesn’t work, you can use dd if=/dev/zero of=/swapfile bs=1024 count=1024288 instead.

Initialize swapfile automatically on boot

```
nano /etc/fstab
Add this at the bottom: /swapfile none swap sw 0 0
```

Install all required dependencies:

```
sudo apt-get update && apt-get upgrade
sudo apt-get install ntp unzip git build-essential libssl-dev libdb-dev
sudo apt-get install libdb++-dev libboost-all-dev libqrencode-dev
sudo aptitude install miniupnpc libminiupnpc-dev
```

Pull the source code from github, or upload it yourself:

```
git clone https://github.com/bitgoldcoin-project/bitgoldcoin.git
```

If your coin uses leveldb, compile leveldb:

```
cd /bitgoldcoin/src/leveldb
make libleveldb.a libmemenv.a
```

Return to source directory, and compile the daemon:

```
cd /bitgoldcoin/src
make -f makefile.unix USE_UPNP=1 USE_QRCODE=1 USE_UPNP=1
```

Strip the file to make it smaller, then relocate it:

```
strip bitgoldcoind
cp bitgoldcoind /usr/bin
```

Now run the daemon:

```
bitgoldcoind
```

It will return an error, telling you to set up config file in a directory. Now we’ll set up the config file. Note that this is case sensitive.

```
nano ~/.bitgoldcoin/bitgoldcoin.conf

Add the following, save and exit:

daemon=1
server=1
rpcuser=(username)
rpcpassword=(strong password)
```

Run bitgoldcoind once more and if you did everything correctly, your daemon is now online! Type bitgoldcoind help for a full list of commands available.
