# Getting Started with PlatformIO

## To Go Further

For more information and detailed guides, visit the [PlatformIO Documentation](https://platformio.org/).

## Why Choose PlatformIO?

PlatformIO is a powerful development environment for embedded systems. It’s an excellent alternative to the Arduino IDE, offering several key advantages:

1. **Project Organization:** PlatformIO uses a clear, modular folder structure, making it easier to manage code, libraries, and configurations.
2. **Customizable Configuration:** The `platformio.ini` file lets you customize build options, specify boards, and manage dependencies easily.
3. **Library Management:** Local library management prevents version conflicts, unlike the global approach used by the Arduino IDE.
4. **Testing and Debugging:** Built-in tools for unit testing and debugging help you find issues directly on your hardware.
5. **Cross-Platform and IDE Support:** PlatformIO works on Windows, macOS, and Linux and integrates with popular editors like VS Code.
6. **Automation and CI Integration:** Seamlessly integrates with CI tools (e.g., GitHub Actions) for automated builds and testing.

If you need more control, better organization, and advanced features for your embedded projects, PlatformIO is the way to go.

## How PlatformIO Works

PlatformIO uses a flexible and powerful setup to help you streamline your development process. Here’s a quick overview of its core components:

### 1. `platformio.ini` - The Project Configuration File

The `platformio.ini` file is the heart of your PlatformIO project. It lets you define your environment, specify the board, set compiler options, and list library dependencies.

**Example `platformio.ini`:**

```ini
[env:esp8266]
platform = espressif8266
board = esp12e
framework = arduino
lib_deps =
    ESP Async WebServer
    ESP8266WiFi
```

### Project Structure

A typical PlatformIO project is organized into a clear folder structure:

```
.
├── include       # Header files
├── lib           # Custom libraries
├── src           # Main source code (e.g., main.cpp)
├── test          # Unit tests
└── platformio.ini # Project configuration file
```

- **`src` folder:** Contains your main code files (e.g., `main.cpp`).
- **`lib` folder:** Holds custom libraries specific to the project.
- **`include` folder:** Stores header files for shared code.

```cpp
/**
 * Blink
 *
 * Turns on an LED on for one second,
 * then off for one second, repeatedly.
 */
#include "Arduino.h"

#ifndef LED_BUILTIN
#define LED_BUILTIN 2
#endif

void setup()
{
  // initialize LED digital pin as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop()
{
  // turn the LED on (HIGH is the voltage level)
  digitalWrite(LED_BUILTIN, HIGH);

  // wait for a second
  delay(1000);

  // turn the LED off by making the voltage LOW
  digitalWrite(LED_BUILTIN, LOW);

   // wait for a second
  delay(1000);
}
```

### Serial Monitor

PlatformIO includes a built-in serial monitor for reading data from your board. Launch it with:

```bash
pio device monitor
```

You can configure the monitor speed in the `platformio.ini` file:

```ini
monitor_speed = 115200
```

### Building and Uploading Code

Compile and upload your code with these commands:

```bash
pio run                   # Build the project
pio run --target upload   # Upload the code to your board
```

PlatformIO automatically detects the connected board, making the upload process seamless.

### Conclusion

PlatformIO is a versatile development environment, ideal for both simple projects and complex embedded systems. For example, using **ESP8266 (ESP82)** and the **AsyncWebServer** library, you can easily build scalable, non-blocking web servers. PlatformIO offers:

- Organized project structure
- Flexible configuration with `platformio.ini`
- Local library management to prevent conflicts
- Integrated tools for testing and debugging
- Seamless support across multiple platforms and IDEs
- CI integration for automated workflows

For more details on setting up a web server with ESP8266 and AsyncWebServer, check out this [tutorial on setting up a web server](./webserver.md)
