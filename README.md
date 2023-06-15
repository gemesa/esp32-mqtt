# esp32-mqtt

esp32-mqtt is a sandbox to explore the MQTT capabilities of the ESP32, for example:
- connect to AWS IoT broker using ssl transport with client certificate
- connect to local broker using ssl transport with PSK

## Toolchain installation, firmware building and flashing

### Prerequisites

- [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/)

Note: ESP-IDF is added as a submodule mainly for reference (to browse the ESP32 libraries if necessary). You can use it to install the toolchain or clone the [original repo](https://github.com/espressif/esp-idf). If you use the submodule you need to initialize the submodules recursively first:

```
$ git submodule update --init --recursive
```

TLDR installation steps (Fedora 38):

```
$ sudo dnf upgrade --refresh
$ sudo dnf install git wget flex bison gperf python3 cmake ninja-build ccache dfu-util libusbx
$ mkdir -p ~/esp
$ cd ~/esp
$ git clone --recursive https://github.com/espressif/esp-idf.git
$ cd ~/esp/esp-idf
$ ./install.sh esp32
$ . $HOME/esp/esp-idf/export.sh

```

### How to build

```
$ cd <example>
$ idf.py menuconfig
# configure Wi-Fi under "Example Connection Configuration" menu (WiFi SSID and WiFi Password), save and quit
$ idf.py build
```

### How to flash

```
$ sudo usermod -a -G dialout $USER
# restart your terminal
$ cd <example>
$ idf.py flash
```

If you encounter the following problem:

```
$ idf.py flash
Executing action: flash
Serial port /dev/ttyUSB0
/dev/ttyUSB0 failed to connect: Could not open /dev/ttyUSB0, the port doesn't exist
No serial ports found. Connect a device, or use '-p PORT' option to set a specific port.
```

Unplug and replug your ESP32 board, then:

```
$ sudo chmod a+rw /dev/ttyUSB0
$ idf.py flash
```

### How to monitor ESP32 logs

```
$ sudo screen /dev/ttyUSB0 115200
```

If you want to quit press `Ctlr+A` then `D`.

## Examples

### SSL with certification

```
```

### SSL with PSK

```
```
