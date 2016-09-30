# [How to Build Proof of Stake Altcoins and Stake Coins on a Remote Ubuntu Server](https://steemit.com/altcoins/@complexring/how-to-build-proof-of-stake-altcoins-and-stake-coins-on-a-remote-ubuntu-server)

Introduction

Mining cryptocurrencies is a difficult endeavor for those who are not technologically inclined. Often, one needs to build a GPU rig, learn how to navigate a unix environment, install dependencies, compile code, scour forums when errors occur because a forked project changed a configuration setting (often due to dynamic versus static libraries), install heat monitoring software, provide 24 hour access to the internet, etc.

Nowadays, without lots of capital (due to the economies of scale) and access to cheap electricity, it is nearly impossible to mine anymore. As an alternative, one may do instead is to stake a favorable cryptotoken (or even several).

This can be done with relative ease, once a server is setup, say for instance with Ubuntu 15.04, and all the dependencies installed.

Pick your favorite company that sell unmanaged servers and choose the server you want; I use wholesaleinternet.net, for instance. I use their dedicated server option loaded with Ubuntu, which gives me full control and also ensures nobody else is sharing the resources of the server.

Making Your User a Sudoer

Depending on your hosting providers' setup, your user may not be a sudoer, which allows you root privileges, but without being logged in as root (which is potentially dangerous from a security point of view).

In order to do this, you need to su into the root account. With wholesaleinternet, this is fortunately the same password as your customer login password (customer being the generic username that they create for you).

Then, you type visudo to load up the sudoers file. Here, you want to add a line which is your username (in this case, customer) followed by the same format that is given for the root username line.

customer ALL=(ALL) ALL

Replace customer with your username that you want to add to the sudoers list. Type ctrl + o and enter to write the new file and then exit with ctrl + x. Then, type exit in the terminal window to leave the root login and return to your normal shell with your new sudo privileges intact.

Build Dependencies

With Ubuntu, installing packages is easy, and you rarely need to compile and install packages by hand. Most of the dependencies for bitcoin are the same for nearly any altcoin that uses that as its main codebase.

Type the following into your command line window :

sudo apt-get install -y git build-essential cmake libssl-dev autoconf autotools-dev \ 
                        doxygen libncurses5-dev libreadline-dev libtool \
                        screen libicu-dev libbz2-dev libqrencode-dev
However, depending on the configuration of these machines, some of the libraries needed for an altcoin may be installed in non-standard locations (or some other voodoo, like broken pkg-config files), and the configure scripts may not always find them. As such, I opted to install 4 of these libraries manually.

These libraries are : openssl, Berkeley DB 4, Boost, and miniUPNPC. Now, the two former packages are compiled with what's known as the autotools suite. miniUPNPC is compiled with CMake, and Boost uses its own installing package known as bjam that is pre-bundled with the source code. All these programs (the autotools and cmake) were installed with the previous command.

Installing Boost

You can install boost with the following set of commands:

cd $HOME
wget http://downloads.sourceforge.net/project/boost/boost/1.61.0/boost_1_61_0.tar.bz2
tar xjf ./boost_1_61_0.tar.bz2
cd boost_1_61_0
./bootstrap.sh --prefix=/opt/boost_1_61_0
./b2 > /dev/null
sudo ./b2 install > /dev/null
Installing Berkeley DB 4

I suggest you download Berkeley DB 4 from Oracle's site. Once you sign up, you can then download the source code and install it via the following :

cd $HOME
wget --user YourOracleUsername --password YourOraclePassword \
             http://download.oracle.com/berkeley-db/db-4.8.30.tar.gz
tar -xkvf db-4.8.30.tar.gz 
cd db-4.8.30/build_unix
../dist/configure --prefix=/opt/db-4.8.30
make
sudo make install
Installing OpenSSL

The following commands will compile and install OpenSSL

wget https://www.openssl.org/source/openssl-1.0.1t.tar.gz
tar -xkvf openssl-1.0.1t.tar.gz
cd openssl-1.0.1t
./config --prefix=/opt/openssl-1.0.1t
make depend
make
sudo make install
Installing MiniUPNPC

MiniUPNPC is not entirely necessary for a headless daemon and often some of the older altcoins have not updated their calls to functions in the later libraries. As such, if an altcoin gives you trouble with miniupnpc, you can always turn off using these features.

Regardless, you can still compile and install the libraries with the following :

cd $HOME
wget http://miniupnp.free.fr/files/miniupnpc-2.0.tar.gz
tar -xkvf miniupnpc-2.0.tar.gz
cd miniupnpc-2.0
cmake .
make
sudo make install
Choose Your Altcoin

Finally, the last thing to do is decide which altcoin you want to stake. Currently, I am staking the following (non-exclusive) list : PeerCoin, NovaCoin, Synergy, SaluS, Diamond, and Rubies.

Since Peercoin was the first coin to use Proof of Stake, I will use that for demonstrative purposes.

Installing PeerCoin

The first step you need to do is to download PeerCoin's source code from the git repository. Type

cd $HOME; git clone https://github.com/ppcoin/ppcoin.git; cd ppcoin

Now, you need to add the location of the libraries and the header files to the bitcoin-qt.pro file (peercoin never updated the name of their *.pro file after they forked).

In the very beginning of the file, there is a commented section that says

# Dependency library locations can be customized with BOOST_INCLUDE_PATH, 
#    BOOST_LIB_PATH, BDB_INCLUDE_PATH, BDB_LIB_PATH
#    OPENSSL_INCLUDE_PATH and OPENSSL_LIB_PATH respectively
Following these, you can paste the following text into the file, using your favorite text editor (vi, emacs, etc.):

BOOST_INCLUDE_PATH=/opt/boost_1_57_0/include
BOOST_LIB_PATH=/opt/boost_1_57_0/lib
BDB_LIB_PATH=/opt/db-4.8.30.NC/lib
BDB_INCLUDE_PATH=/opt/db-4.8.30.NC
OPENSSL_LIB_PATH=/opt/openssl-1.0.1t/lib
OPENSSL_INCLUDE_PATH=/opt/openssl-1.0.1t/include
I prefer emacs, so you can do the following to open the file

emacs -nw bitcoin-qt.pro

and then use the arrow keys to move a few lines down and then paste with ctrl + v from the clipboard. You can save typing ctrl + x and then ctrl + s. Afterwards, you can quit by typing ctrl + x and then ctrl + c.

Now, all you need to do is use the qt's software's making tool called qmake. Type qmake into the terminal and then cd src. Type make -f makefile.unix to build the peercoin daemon.

Assuming all is well in the world, this should compile and you should have no compilation errors and a new file called ppcoind is in the src directory. You can test for this by typing ls $HOME/ppcoin/src and seeing if that file is listed there.

If there was a failure, it is most likely due to miniupnpc and you can turn these features off in the qmake step by typing

qmake UPNP=-

Starting ppcoind and Setting up ppcoin.conf

If you managed to make it this far, then you have successfully built the headless daemon and can begin staking as soon as you finish a few more small tasks.

You first need to run the daemon. This can be accomplished by typing :

$HOME/ppcoin/src/ppcoind &

The first time that this file is run, it will test for a configuration file in the default location. If no configuration file exists (and it shouldn't!), the ppcoind program will give a suggestion for a username and password to be placed in the config file (in this case $HOME/.ppcoin/ppcoin.conf). Copy the username and password and paste these into a new file with this name in this location, using, for example emacs -nw $HOME/.ppcoin/ppcoin.conf and then pasting into the window.

Now, you can rerun the daemon program again using
$HOME/ppcoin/src/ppcoind &

At this point, ppcoind should be downloading the blockchain and verifying blocks, which should take a day or so. This can be checked by looking at the log file. You can see if everything is going smoothly by typing tail -100 $HOME/.ppcoin/debug.log.

Examine the contents of the file. If it appears as if blocks are being downloaded (they'll have a timestamp date), and the blockheight is increasing, then you are all set.

Go to work, get some coffee, eat, jog, sleep, etc.

Encrypting Your Wallet

Now, after you're rested and have checked that the last blocks you are downloading for the blockchain are the ones that are being signed in real time, it is now the moment to learn some of the commands that you can use to communicate with the wallet.

For a list of everything that you can do, type
$HOME/ppcoin/src/ppcoind help

The most important commands will be getnewaddress, sendtoaddress, encryptwallet, walletpassphrase, and walletlock.

Encrypt your wallet with the following :
$HOME/ppcoin/src/ppcoind encryptwallet yourwalletpassword

You will then need to restart the daemon again with
$HOME/ppcoin/src/ppcoind &

Now you can get a new address (with a label, if you so desire) using

$HOME/ppcoin/src/ppcoind getnewaddress "Label 1"

The output of this will be your new address which will be associated (internally) with the identifier "Label 1". You can now send your peercoins to this new address.

In order to stake the new coins (after waiting the required time for the coins to mature), you can type

$HOME/ppcoin/src/ppcoind walletpassphrase yourpassphrasehere 999999 true

The 999999 is the number of seconds you want the wallet to remain unlocked and true indicates you want to set it up for staking (which doesn't allow you to send coins out of it).

That's it, you're all done!

You can unlock your wallet and set it up for staking at any time (even before you have coins in your wallet).

Unfortunately, peercoin does not have the fairly useful command getstakinginfo, which is helpful to verify you are staking. On (nearly) any other cryptotoken, you can test if you are staking after the coins mature by typing

$HOME/coin/src/coind getstakinginfo
(with the appropriate locations and names editted) and verifying the output of the staking field is true.

Congratulations! You are now staking!




# [Compiling an altcoin daemon in Ubuntu Linux](https://dev.cryptolife.net/compiling-the-daemon-for-your-altcoin-in-ubuntu-linux/)


This guide will assist you in compiling a headless client (the daemon) in Ubuntu Linux. 
This is a necessary step if you wish to set up a node of your altcoin. 
It is assumed that you are starting on a fresh Ubuntu (14.04 x64) installation. 
If you require a VPS for hosting, I highly recommended Digital Ocean.


The $5/month plan is all you need. Use sudo in front of all commands if you’re not logged in as root. Replace YOURCOIN with the name of your coin (case sensitive).
Set up a swapfile if your system has less than 1.5GB of memory:
fallocate -l 2G /swapfile
chown root:root /swapfile
chmod 0600 /swapfile
mkswap /swapfile
swapon /swapfile
If fallocate doesn’t work, you can use dd if=/dev/zero of=/swapfile bs=1024 count=1024288 instead.
Initialize swapfile automatically on boot
nano /etc/fstab
Add this at the bottom: /swapfile none swap sw 0 0
Install all required dependencies:
apt-get update && apt-get upgrade
apt-get install ntp unzip git build-essential libssl-dev libdb-dev
apt-get install libdb++-dev libboost-all-dev libqrencode-dev
aptitude install miniupnpc libminiupnpc-dev
Pull the source code from github, or upload it yourself:
git clone https://github.com/YOURCOIN
If your coin uses leveldb, compile leveldb:
cd /YOURCOIN/src/leveldb
make libleveldb.a libmemenv.a
Return to source directory, and compile the daemon:
cd /YOURCOIN/src
make -f makefile.unix USE_UPNP=1 USE_QRCODE=1 USE_UPNP=1
Strip the file to make it smaller, then relocate it:
strip YOURCOINd
cp YOURCOINd /usr/bin
Now run the daemon:
YOURCOINd
It will return an error, telling you to set up config file in a directory. Now we’ll set up the config file. Note that this is case sensitive.
nano ~/.YOURCOIN/YOURCOIN.conf
Add the following, save and exit:
daemon=1
server=1
rpcuser=(username)
rpcpassword=(strong password)
Run YOURCOINd once more and if you did everything correctly, your daemon is now online! Type YOURCOINd help for a full list of commands available.

