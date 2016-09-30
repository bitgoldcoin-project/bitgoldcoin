# QT 설치

* Daemon

> miniupnpc-1.9.20140911 => minupnpc rename

> mingw-64 설치시 버전을 4.9.1로 설치

> makefile.mingw 수정

  line:66 DEFS += -DMINIUPNP_STATICLIB -DUSE_UPNP=$(USE_UPNP)

> Qt 4.8.6

- configureapp.cpp
```
2180, 2262
endsWith => contains
```

make -f makefile.mingw
strip bitgoldcoind.exe

* QT 
> bitgoldcoin-qt_windows.pro 수정
  line88: DEFINES += USE_UPNP=$$USE_UPNP MINIUPNP_STATICLIB

set PATH=%PATH%;D:\Qt\4.8.6\bin
qmake "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" bitgoldcoin-qt_windows.pro
mingw32-make -f Makefile.Release
