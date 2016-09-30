# Daemon

## Compiling Windows Altcoin Client 32 Bit  

### Windows 32 bit bitgoldcoind and bitgoldcoin Compiling Guide.

Compiling  your own altcoin client can be tricky. Just make sure to follow all steps word for word. If you want to cheat you can download the pre-built dependencies [here](http://www.mediafire.com/file/y89546s65sf8de5/deps.zip), though it is recommended to build your own.

### 1. Pre-installers.

#### 1a.File Compressor/Extractor

Download and install Winrar or an [alternate file compression tool](http://www.rarlab.com/download.htm).

#### 1b. Text Editor.

Download and install a text editor such as [Sublime Text](http://www.sublimetext.com/). You need a text editor that can easily search and replace case sensitive text.

### 2. Download and install MingW

[download](http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download)
Double click to install, keep the checkbox for the GUI checked and make sure to install in C:\MinGW. Press continue. From the MinGW GUI interface, go to all packages -> MYSYS

Right click on the following installations and mark for installation.

```
msys-base-bin (may highlight other checkboxes which is fine)
msys-autoconf-bin
msys-automake-bin
msys-libtool-bin
```

Click on Installation -> Apply changes. MinGW will now download the remaining packages. Make sure to remove any previous installs of MinGW before starting. 
Once complete, navigate to C:\MinGW\bin and you should only have a mingw-get application. If you have msys-gcc and msys-w32api you need to delete MinGW and check the correct install packages are selected above.

#### Download and extract mingw32 to C:\mingw32

[download](http://sourceforge.net/projects/mingw-w64/files/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/4.9.1/theads-posix/dwarf/i686-4.9.1-release-posix-dwarf-rt_v3-rev1.7z/download)

mingw-64 설치시 버전을 4.9.1로 설치

You now need to change the path variables. Go to control panel->system and security->system. Click on advanced system properties->environmental variables. In the top box navigate to PATH and change to 

```
C:\mingw32\bin;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\
```

Press ok. You may need to restart your computer to ensure the previous build variables are not cached.
Checking your MingW install. To start the MingW app navigate to C:\MinGW\msys\1.0\msys.bat, create a desktop shortcut as you will use the msys command as well as the windows command prompts. Double click to start and enter the following to display the version and correct paths.

```
gcc -v
```

Your msys output should look like the following code.

```
$ gcc -v

Using built-in specs.
COLLECT_GCC=c:\mingw32\bin\gcc.exe
COLLECT_LTO_WRAPPER=c:/mingw32/bin/../libexec/gcc/i686-w64-mingw32/4.9.1/lto-wrapper.exe
Target: i686-w64-mingw32
Configured with: ../../../src/gcc-4.9.1/configure --host=i686-w64-mingw32 --build=i686-w64-mingw32 --target=i686-w64-mingw32 --prefix=/
mingw32 --with-sysroot=/c/mingw491/i686-491-posix-dwarf-rt_v3-rev1/mingw32 --with-gxx-include-dir=/mingw32/i686-w64-mingw32/include/c++
 --enable-shared --enable-static --disable-multilib --enable-languages=ada,c,c++,fortran,objc,obj-c++,lto --enable-libstdcxx-time=yes -
-enable-threads=posix --enable-libgomp --enable-libatomic --enable-lto --enable-graphite --enable-checking=release --enable-fully-dynam
ic-string --enable-version-specific-runtime-libs --disable-sjlj-exceptions --with-dwarf2 --disable-isl-version-check --disable-cloog-ve
rsion-check --disable-libstdcxx-pch --disable-libstdcxx-debug --enable-bootstrap --disable-rpath --disable-win32-registry --disable-nls
 --disable-werror --disable-symvers --with-gnu-as --with-gnu-ld --with-arch=i686 --with-tune=generic --with-libiconv --with-system-zlib
 --with-gmp=/c/mingw491/prerequisites/i686-w64-mingw32-static --with-mpfr=/c/mingw491/prerequisites/i686-w64-mingw32-static --with-mpc=
/c/mingw491/prerequisites/i686-w64-mingw32-static --with-isl=/c/mingw491/prerequisites/i686-w64-mingw32-static --with-cloog=/c/mingw491
/prerequisites/i686-w64-mingw32-static --enable-cloog-backend=isl --with-pkgversion='i686-posix-dwarf-rev1, Built by MinGW-W64 project'
 --with-bugurl=http://sourceforge.net/projects/mingw-w64 CFLAGS='-O2 -pipe -I/c/mingw491/i686-491-posix-dwarf-rt_v3-rev1/mingw32/opt/in
clude -I/c/mingw491/prerequisites/i686-zlib-static/include -I/c/mingw491/prerequisites/i686-w64-mingw32-static/include' CXXFLAGS='-O2 -
pipe -I/c/mingw491/i686-491-posix-dwarf-rt_v3-rev1/mingw32/opt/include -I/c/mingw491/prerequisites/i686-zlib-static/include -I/c/mingw4
91/prerequisites/i686-w64-mingw32-static/include' CPPFLAGS= LDFLAGS='-pipe -L/c/mingw491/i686-491-posix-dwarf-rt_v3-rev1/mingw32/opt/li
b -L/c/mingw491/prerequisites/i686-zlib-static/lib -L/c/mingw491/prerequisites/i686-w64-mingw32-static/lib -Wl,--large-address-aware'
Thread model: posix
gcc version 4.9.1 (i686-posix-dwarf-rev1, Built by MinGW-W64 project)
```

If you are having issues I have uploaded a MinGW install here 

### 3.Download and Install Dependencies.

Create a deps folder at C:\deps

#### 3a.OpenSLL- Install OpenSSL dependencies on Windows.

Download the latest version of [OpenSSL](https://www.openssl.org/source/openssl-1.0.1j.tar.gz) to your deps folder.
Open the MinGW shell at C:\MinGW\msys\1.0\msys.bat 

```
cd /c/deps/
tar xvfz openssl-1.0.1j.tar.gz
cd openssl-1.0.1j
./Configure no-zlib no-shared no-dso no-krb5 no-camellia no-capieng no-cast no-cms no-dtls1 no-gost no-gmp no-heartbeats no-idea no-jpake no-md2 no-mdc2 no-rc5 no-rdrand no-rfc3779 no-rsax no-sctp no-seed no-sha0 no-static_engine no-whirlpool no-rc2 no-rc4 no-ssl2 no-ssl3 mingw
make
```

#### 3b.Berkeley DB

[Download](http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz) and place in your deps folder.
In the MinGW shell use the following code.

```
cd /c/deps/
tar xvfz db-4.8.30.NC.tar.gz
cd db-4.8.30.NC/build_unix
../dist/configure --enable-mingw --enable-cxx --disable-shared --disable-replication
make
```

#### 3c.Boost

[Download](http://sourceforge.net/projects/boost/files/boost/1.55.0/boost_1_55_0.zip/download) Boost to your deps folder. 
Make sure to download either the 7z or zip versions. Double click on the folder to extract boost_1_55_0 in your deps folder. This may take several minutes depending on your PC's speed. Using the Windows command prompt, bootstrap and compile boost. To bring up the windows command prompt just type cmd in the windows search bar.


```
cd C:\deps\boost_1_55_0\
bootstrap.bat mingw
b2 --build-type=complete --with-chrono --with-filesystem --with-program_options --with-system --with-thread toolset=gcc variant=release link=static threading=multi runtime-link=static stage
```

#### 3d.Mini UPNP

[Download](http://miniupnp.free.fr/files/download.php?file=miniupnpc-1.9.20140911.tar.gz) and extract MiniUPNP to your deps folder. 
Rename  folder from "miniupnpc-1.9.20140911" to "miniupnpc" then from a Windows command prompt:

```
cd C:\
ren miniupnpc-1.9.20140911 minupnpc 
cd C:\deps\miniupnpc
mingw32-make -f Makefile.mingw init upnpc-static
```

#### 3e.Protoc and Libprotobuf:

[Download](http://protobuf.googlecode.com/files/protobuf-2.5.0.zip) and extract  to your deps folder.
In the msys shell run

```
cd /c/deps/protobuf-2.5.0
configure --disable-shared
make
```

#### 3f.libpng

[Download](http://prdownloads.sourceforge.net/libpng/libpng-1.6.14.tar.gz?download) and extract to your deps folder. Extract.
In msys shell run 

```
cd /c/deps/libpng-1.6.14
configure --disable-shared
make
cp .libs/libpng16.a .libs/libpng.a
```

#### 3g.qrencode

[Download](http://fukuchi.org/works/qrencode/qrencode-3.4.4.tar.gz) and extract to your deps folder.
In msys shell run 

```
cd /c/deps/qrencode-3.4.4

LIBS="../libpng-1.6.14/.libs/libpng.a ../../mingw32/i686-w64-mingw32/lib/libz.a" \
png_CFLAGS="-I../libpng-1.6.14" \
png_LIBS="-L../libpng-1.6.14/.libs" \
configure --enable-static --disable-shared --without-tools

make
```

### 4. Download and Compile QT.

[Download](http://download.qt-project.org/official_releases/qt/4.8/4.8.6/qt-everywhere-opensource-src-4.8.6.zip) and uncompress to C:\Qt\4.8.6. Once again check it resides in C:\Qt\4.8.6 not C:\Qt\4.8.6\4.8.6. Due to a bug in 4.8.6 you will need to apply the patch available here. For those who can't find or work it out, you need to change the following lines in C:\Qt\4.8.6\tools\configure\configureapp.cpp or download the patched file here and replace it in C:\Qt\4.8.6\tools\configure\configureapp.cpp

```
2180  |  -  const QString mingwPath = dictionary["QMAKESPEC"].endsWith("-g++") ?
2180  |  +  const QString mingwPath = dictionary["QMAKESPEC"].contains("-g++") ?
2252  |   -  if (dictionary["QMAKESPEC"].endsWith("-g++")+
2252  |  +  if (dictionary["QMAKESPEC"].contains("-g++")+
```

From your Windows command prompt run 

```
cd C:\Qt\4.8.6
configure -release -opensource -confirm-license -static -no-sql-sqlite -no-qt3support -no-opengl -qt-zlib -no-gif -qt-libpng -qt-libmng -no-libtiff -qt-libjpeg -no-dsp -no-vcproj -no-openssl -no-dbus -no-phonon -no-phonon-backend -no-multimedia -no-audio-backend -no-webkit -no-script -no-scripttools -no-declarative -no-declarative-debug -qt-style-windows -qt-style-windowsxp -qt-style-windowsvista -no-style-plastique -no-style-cleanlooks -no-style-motif -no-style-cde -nomake demos -nomake examples
mingw32-make
```

## Compiling bitgoldcoind.

### 5. Download and extract the bitgoldcoin source code to C:\bitgoldcoin-0.8. Navigate to  C:\bitgoldcoin-0.8\src\makefile.mingw and open with your text editor.

[download](https://github.com/bitgoldcoin-project/bitgoldcoin/archive/master.zip)

```
USE_UPNP:=-
USE_IPV6:=1

DEPSDIR?=/usr/local
BOOST_SUFFIX?=-mgw46-mt-sd-1_52

INCLUDEPATHS= \
 -I"$(CURDIR)" \
 -I"$(DEPSDIR)/include"

LIBPATHS= \
 -L"$(CURDIR)/leveldb" \
 -L"$(DEPSDIR)/lib"

LIBS= \
 -l leveldb \
 -l memenv \
 -l boost_system$(BOOST_SUFFIX) \
 -l boost_filesystem$(BOOST_SUFFIX) \
 -l boost_program_options$(BOOST_SUFFIX) \
 -l boost_thread$(BOOST_SUFFIX) \
 -l boost_chrono$(BOOST_SUFFIX) \
 -l db_cxx \
 -l ssl \
 -l crypto

DEFS=-D_MT -DWIN32 -D_WINDOWS -DBOOST_THREAD_USE_LIB -DBOOST_SPIRIT_THREADSAFE
DEBUGFLAGS=-g
CFLAGS=-mthreads -O2 -w -Wall -Wextra -Wformat -Wformat-security -Wno-unused-parameter $(DEBUGFLAGS) $(DEFS) $(INCLUDEPATHS)
# enable: ASLR, DEP and large address aware
LDFLAGS=-Wl,--dynamicbase -Wl,--nxcompat -Wl,--large-address-aware
```

to 

```
USE_UPNP:=1
USE_IPV6:=1

DEPSDIR?=/usr/local
BOOST_SUFFIX?=-mgw49-mt-s-1_55

INCLUDEPATHS= \
 -I"$(CURDIR)" \
 -I"/c/deps/boost_1_55_0" \
 -I"/c/deps" \
 -I"/c/deps/db-4.8.30.NC/build_unix" \
 -I"/c/deps/openssl-1.0.1j/include"
 
LIBPATHS= \
 -L"$(CURDIR)/leveldb" \
 -L"/c/deps/boost_1_55_0/stage/lib" \
 -L"/c/deps/miniupnpc" \
 -L"/c/deps/db-4.8.30.NC/build_unix" \
 -L"/c/deps/openssl-1.0.1j"

LIBS= \
 -l leveldb \
 -l memenv \
 -l boost_system$(BOOST_SUFFIX) \
 -l boost_filesystem$(BOOST_SUFFIX) \
 -l boost_program_options$(BOOST_SUFFIX) \
 -l boost_thread$(BOOST_SUFFIX) \
 -l boost_chrono$(BOOST_SUFFIX) \
 -l db_cxx \
 -l ssl \
 -l crypto

DEFS=-D_MT -DWIN32 -D_WINDOWS -DBOOST_THREAD_USE_LIB -DBOOST_SPIRIT_THREADSAFE
DEBUGFLAGS=-g
CFLAGS=-mthreads -O2 -w -Wall -Wextra -Wformat -Wformat-security -Wno-unused-parameter $(DEBUGFLAGS) $(DEFS) $(INCLUDEPATHS)
# enable: ASLR, DEP and large address aware
LDFLAGS=-Wl,--dynamicbase -Wl,--nxcompat -Wl,--large-address-aware -static
```

We change the deps to point to the deps we created earlier. If you chose to place your deps in a different folder, change the code to point to your folders. We also  -static to LDFLAGS=-Wl,--dynamicbase -Wl,--nxcompat -Wl,--large-address-aware -static for static linked exe,  updated UPNP Includes and Lib paths to enable UPNP.

In the Msys shell, you can now compile bitgoldcoind.

```
 makefile.mingw 수정
 line:66 DEFS += -DMINIUPNP_STATICLIB -DUSE_UPNP=$(USE_UPNP)
```

```
cd /c/bitgoldcoin/src
make -f makefile.mingw
strip bitgoldcoind.exe
```

Your bitgoldcoind should now be in your C:\bitgoldcoin-0.8.6\src folder.

## Compiling bitgoldcoin Windows QT.

Create libleveldb.a and libmemenv.a.
Using Msys shell, run

```
cd /C/bitgoldcoin-0.8.6/src/leveldb
TARGET_OS=NATIVE_WINDOWS make libleveldb.a libmemenv.a
```

If the file already exists, Msys will inform you so.

Open your bitcoin-qt.pro file.
 We now need to change the dependency directory locations 

```
# Dependency library locations can be customized with:
#    BOOST_INCLUDE_PATH, BOOST_LIB_PATH, BDB_INCLUDE_PATH,
#    BDB_LIB_PATH, OPENSSL_INCLUDE_PATH and OPENSSL_LIB_PATH respectively

OBJECTS_DIR = build
MOC_DIR = build
UI_DIR = build
```

to

```
# Dependency library locations can be customized with:
#    BOOST_INCLUDE_PATH, BOOST_LIB_PATH, BDB_INCLUDE_PATH,
#    BDB_LIB_PATH, OPENSSL_INCLUDE_PATH and OPENSSL_LIB_PATH respectively
BOOST_LIB_SUFFIX=-mgw49-mt-s-1_55
BOOST_INCLUDE_PATH=C:/deps/boost_1_55_0
BOOST_LIB_PATH=C:/deps/boost_1_55_0/stage/lib
BDB_INCLUDE_PATH=C:/deps/db-4.8.30.NC/build_unix
BDB_LIB_PATH=C:/deps/db-4.8.30.NC/build_unix
OPENSSL_INCLUDE_PATH=C:/deps/openssl-1.0.1j/include
OPENSSL_LIB_PATH=C:/deps/openssl-1.0.1j
MINIUPNPC_INCLUDE_PATH=C:/deps/
MINIUPNPC_LIB_PATH=C:/deps/miniupnpc
QRENCODE_INCLUDE_PATH=C:/deps/qrencode-3.4.4
QRENCODE_LIB_PATH=C:/deps/qrencode-3.4.4/.libs
OBJECTS_DIR = build
MOC_DIR = build
UI_DIR = build
```

and comment out the genleveldb command

```
genleveldb.commands = cd $$PWD/src/leveldb && CC=$$QMAKE_CC CXX=$$QMAKE_CXX $(MAKE) OPT=\"$$QMAKE_CXXFLAGS 
```

to

```
#genleveldb.commands = cd $$PWD/src/leveldb && CC=$$QMAKE_CC CXX=$$QMAKE_CXX $(MAKE) OPT=\"$$QMAKE_CXXFLAGS 
```

and add the flags for a static build.
Add to 

```
CONFIG += static
```

to 

```
CONFIG += thread
CONFIG += static
```

and 
 
```
win32:QMAKE_LFLAGS *= -Wl,--large-address-aware
```

to

```
win32:QMAKE_LFLAGS *= -Wl,--large-address-aware  -static
```

```
 bitgoldcoin-qt_windows.pro 수정
 line88: DEFINES += USE_UPNP=$$USE_UPNP MINIUPNP_STATICLIB
```

Now from a the Windows cmd, run  

```
set PATH=%PATH%;C:\Qt\4.8.6\bin
cd C:\bitgoldcoin\
qmake "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" bitgoldcoin-qt_windows.pro
mingw32-make -f Makefile.Release
```

Your bitgoldcoin qt should now be available in your C:\bitgoldcoin\release
