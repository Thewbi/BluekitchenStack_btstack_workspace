# BluekitchenStack_btstack_workspace
Experiments and documentation for the Bluekitchen BTStack: https://github.com/bluekitchen/btstack

# Prerequisits

## ASUS BT400 (Broadcom Chip) BT Classic and BLE USB device.

Connect the BT400 USB Bluetooth dongle.
In Zadig, load the WinUSB (v6.1.7600.16385) driver for the windows-winusb port to work.

## CMake-GUI

On windows, install CMake-GUI (https://cmake.org/cmake/help/latest/manual/cmake-gui.1.html).

## Zadig

https://zadig.akeo.ie/

## Cygwin

This step might not be necessary for the windows-winusb port.

It seems that the easiest way to get libusb to work under Windows is to use Cygwin.
Use setup-x86_64.exe to install libusb in a recent version.

## portaudio library

Download pa_stable_v190700_20210406.tgz from https://files.portaudio.com/download.html.
Unzip that file and open cmake-gui.
In cmake-gui open the folders: Browse Source: C:/Users/<USER>/Downloads/pa_stable_v190700_20210406/portaudio
Where to build the binaries: C:/Users/<USER>/Downloads/pa_stable_v190700_20210406/portaudio

in cmake-gui:
1. Click the "Configure" button
2. Click the "Generate" button
3. Click the "Open Project" button

In Visual Studio, build the project portaudio_static.
The result is placed here: C:\Users\<USER>\Downloads\pa_stable_v190700_20210406\portaudio\Debug\portaudio_static_x64.lib.

# Compile the btstack Windows port

Open cmake gui on the folder C:\Users\<USER>\dev\btstack\port\windows-winusb 
(Not C:/Users/<USER>/dev/btstack/port/libusb because it seems that the libusb port is tightly connected to Linux or OSX)

in cmake-gui click configure, click generate
A file called BTstack-libusb.sln is generated.
Open BTstack-libusb.sln in visual studio by clicking the button "Open Project" in cmake-gui.

Once the Solution is opened in Visual Studio, there are a lot of samples to try.

A good starting point for Bluetooth Classic is the spp_counter.
A good starting point for Bluetooth Low Energy is the gatt_counter

You can make one of the examples the startup project via the context menu.

## Fixing the Samples

For every sample project in the solution, there are adjustments necessary to get the project to compile

### portaudio
Inside the file C:\Users\<User>\dev\btstack\platform\posix\btstack_audio_portaudio.c replace

```
#include <portaudio.h>
```

by

```
#include "C:\Users\<User>\Downloads\pa_stable_v190700_20210406\portaudio\include\portaudio.h"
```

### Build Configuration (C/C++ Directories)

* Edit the Include Directories and add C:\Users\<User>\Downloads\pa_stable_v190700_20210406\portaudio\include.
* Edit the Library Directories and add C:\Users\wolfg\Downloads\pa_stable_v190700_20210406\portaudio\Debug
* Edit the Linker Inputs and change it to:

```
portaudio_static_x64.lib
Debug\btstack.lib
setupapi.lib
winusb.lib
kernel32.lib
user32.lib
gdi32.lib
winspool.lib
shell32.lib
ole32.lib
oleaut32.lib
uuid.lib
comdlg32.lib
advapi32.lib
```




