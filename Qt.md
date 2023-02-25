## Compile QT Source for another platform
```sh
cd QT_SRC
./configure -opensource -confirm-license -xplatform win32-g++ -device-option CROSS_COMPILE=/usr/bin/x86_64-w64-mingw32 -prefix /usr/Qt/5.13.2/x86_64-w64-mingw32 -nomake examples
```

<hr>

[!] if **Limit Error** happens

```sh
cd QT_SRC/qtbase/corelib/global/
vim qglobal.h
```

Add `#include <limits>` to this first part:

Result should be like below:
```cpp
#ifdef __cplusplus
#  include <type_traits>
#  include <cstddef>
#  include <utility>
#  include <limits>
#endif
```


<hr>

## Compile QT for ARCH in Windows
```sh
configure.bat  -xplatform linux-arm-gnueabi-g++ -v -developer-build -opensource -prefix  C:\Qt\Qt5.14.2\5.14.2\arm  -release -qml -sql-sqlite  -dbus -no-opengl
```

## Run Application to ARCH
```sh
export DISPLAY=:0
amixer -c 0 sset 'Headphone',0 100% unmute
QT_QPA_PLATFORM=linuxfb:fb=/dev/fb0
```
## Run Application in another ARCH Device

So once x11vnc is installed, you can use it like this:

1) Start your Qt5 application:

``./myCuteQt551App -platform linuxfb -geometry WxH &``

Where W and H are respectively the Width and the Height of your graphical view (in integer, for instance 240x320).

2) Start your x11vnc server

``x11vnc -noipv6 -rawfb /dev/fb0 -clip WxH+0+0``