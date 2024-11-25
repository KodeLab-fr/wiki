# Getting started ESP CAM

## How to setup platformio

Create project:

```bash
pio init --board esp32cam
```

We just need to modifi the `platformio.ini` to get access to monitor:

> [!WARNING]  
> Need to modify the document to get monitor working great

```ini
[env:esp32cam]
platform = espressif32
board = esp32cam
framework = arduino
monitor_speed = 115200
monitor_rts = 0
monitor_dtr = 0
```

## Trying our config

Now you can create the `src/main.cpp` file here is a config example

```cpp
#include <Arduino.h>

void setup() {
  Serial.begin(115200);
}

void loop() {
  delay(1000);
  Serial.println("Waiting 1s...");
}
```

Now it is time to try:

```bash
pio device monitor
# Use the following if monitor rts not configured or not in the right directory
# pio device monitor --baud 115200 --rts 0 --dtr 0
```

## Going further

Take a look at this two posts:
![Streaming from esp32](./esp32/esp-cam/stream.md)
![Capture tool from esp32](./esp32/esp-cam/capture.md)
