* Dependancy / QT ��ġ*

�� �÷����� ���� ����

1. ��������
��ġ�� ���α׷��� ���� ���� ���̺귯�� �浹 ������
������ ��Ÿ��(MinGW)�� ���̺귯���� ��ġ�� ���� �ʴ� ��찡 �߻��ϱ� ������
Ŭ�� ��ġ�� �����쿡�� �����ϴ� ���� ����
(Dependancy�� ��ġ �� Virtual PC ���)

2. ������ / ��
�Ŵ��� ����


* DAEMON ���� ���

1. WINDOWS (�ڼ��� ������ ÷�ε� �������� ����.)

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
QT5 Creator (GUI) ���� (sudo qtcreator)
bitgoldcoin-qt_linux.pro ����
Build

3. Mac

* Daemon
make -f makefile.osx
strip bitgoldcoind

* QT 
QT5 Creator (GUI) ���� 
bitgoldcoin-qt_linux.pro ����
Build

