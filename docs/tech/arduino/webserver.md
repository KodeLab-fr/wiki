# Create a Web Server with ESP8266 (ESP82)

## Table of Contents

1. [Setup `platformio.ini`](#setup-platformioini)
2. [Code for Web Server](#code-for-web-server)
3. [Upload and Run](#upload-and-run)
4. [Why Use Async?](#why-use-async)
5. [Benchmarking with OHA](#benchmarking-with-oha)
6. [PlatformIO Requirements](#platformio-requirements)

## Requirements

- [PlatformIO](platformio.md)
- ESP8266 Board (e.g., ESP-12E)
- AsyncWebServer Library

## 1. Setup `platformio.ini`

Create a `platformio.ini` file with the following configuration:

```ini
[env:esp8266]
platform = espressif8266
board = esp12e
framework = arduino
lib_deps =
    ESP Async WebServer
    ESP8266WiFi
```

## 2. Code for Web Server

Create a `src/main.cpp` file with the following code:

```cpp
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

ESP8266WebServer server(80); // Create a web server on port 80

void handleRoot() {
  String message = "";
  for (uint8_t i = 0; i < server.args(); i++) {
    message += server.argName(i) + ": " + server.arg(i) + "\n";
  }
  if (server.method() == HTTP_POST) {
    message = "Hello\n" + message;
  }
  server.send(200, "text/plain", message);
}

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);
  while (WiFi.status()!= WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.begin();
  Serial.println("HTTP server started");
}

void loop() {
  server.handleClient();
}

```

```cpp
#include <ESP8266WiFi.h>
#include <ESPAsyncWebServer.h>

// Replace with your network credentials
const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

AsyncWebServer server(80);

void setup() {
  // Start serial communication
  Serial.begin(115200);

  // Connect to Wi-Fi
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.println("Connected to WiFi");

  // Serve a simple webpage
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *request){
    request->send(200, "text/html", "<h1>Hello from ESP8266 Web Server!</h1>");
  });

  // Start the server
  server.begin();
}

void loop() {
  // Nothing to do here
}
```

### 3. Upload and Run

Run the following commands to build and upload your code:

```bash
pio run        # Build the project
pio run --target upload  # Upload to ESP8266
```

Once uploaded, open the Serial Monitor to find the IP address. Visit the IP address in your browser to see the web page.

This markdown guide provides the steps for setting up a simple web server with ESP8266 using Platfo

### Why Use Async?

By using the `AsyncWebServer` library, your web server can handle multiple client requests simultaneously without blocking other tasks. This significantly improves the performance and speed of the server, especially when dealing with large numbers of requests or performing other I/O operations. Async operations are non-blocking, which means they keep the microcontroller responsive and efficient, even when the server is busy serving requests.

### Benchmarking with OHA

To benchmark the performance of your web server, you can use tools like **OHA (Open Hardware Analyzer)** to measure latency, throughput, and response times of the server under different loads.

- **Step 1:** Install OHA and connect your ESP8266 to the network.
- **Step 2:** Run the benchmarking tool to send multiple requests to the server and measure the results.
- **Step 3:** Analyze the data to assess performance and identify any potential bottlenecks or areas for optimization.

Using OHA will help you evaluate your server's performance and fine-tune it for better efficiency and speed.
