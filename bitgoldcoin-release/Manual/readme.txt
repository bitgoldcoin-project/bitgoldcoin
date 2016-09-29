* Dependancy / QT 설치*

각 플랫폼별 문서 참고

1. 윈도우즈
설치된 프로그램에 따라 공유 라이브러리 충돌 때문에
리눅스 런타임(MinGW)용 라이브러리가 설치가 되지 않는 경우가 발생하기 때문에
클린 설치된 윈도우에서 빌드하는 것을 권장
(Dependancy가 설치 된 Virtual PC 사용)

2. 리눅스 / 맥
매뉴얼 참조


* DAEMON 빌드 방법

1. WINDOWS (자세한 내용은 첨부된 웹문서를 참고.)

> 
> miniupnpc-1.9.20140911 => minupnpc

> mingw-64 설치시 버전을 4.9.1로 설치

> makefile.mingw

65: DEFS += -DMINIUPNP_STATICLIB -DUSE_UPNP=$(USE_UPNP)

> Qt 4.8.6

- configureapp.cpp
```

2180, 2262
endsWith => contains

```

- qcoloroutput_p.h
```c++
    enum
        {
            ForegroundShift = 10,
            BackgroundShift = 20,
            SpecialShift    = 20,
//            ForegroundMask  = ((1 << ForegroundShift) - 1) << ForegroundShift,
//            BackgroundMask  = ((1 << BackgroundShift) - 1) << BackgroundShift
            ForegroundMask  = 0x1f << ForegroundShift,
            BackgroundMask  = 0x07 << BackgroundShift
        };
```




* Daemon
make -f makefile.mingw
strip bitgoldcoind.exe

* QT 
qmake "USE_QRCODE=1" "USE_UPNP=1" "USE_IPV6=1" bitgoldcoin-qt_windows.pro
mingw32-make -f Makefile.Release

2. UNIX 

* Daemon
make -f makefile.unix
strip bitgoldcoind

* QT (Devian / Ubuntu)
QT5 Creator (GUI) 실행 (sudo qtcreator)
bitgoldcoin-qt_linux.pro 열기
Build

3. Mac

* Daemon
make -f makefile.osx
strip bitgoldcoind

* QT 
QT5 Creator (GUI) 실행 
bitgoldcoin-qt_linux.pro 열기
Build

