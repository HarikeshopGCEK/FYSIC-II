
---
# Overview

- **A robot that can be controlled wirelessly using a web browser interface over Wi-Fi
- **NodeMCU Acts as both Wi-Fi sever and the control for the motors
- **No need for any external apps just use a browser on phone/laptop

---

# Core Concept

Use NodeMCU to **host a web page** (or connect to your router), then map **button presses** to **motor commands**.

**[[NodeMCU PinOut L298N Bot block diagram|Basic Flow]]

---
# Hardware and Software Requirements

- 🧠 **NodeMCU (ESP8266)**
- ⚙️ **L298N Motor Driver Module**
- 🔋 **2x Geared DC Motors**
- 🚗 **Robot Chassis + Wheels**
- 🔋 **Battery Pack (7.4V Li-ion recommended)**
- 🧵 Jumper Wires and Breadboard (optional)

---
# Code For The Bot

```cpp
#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>

#define LSPEED 255
#define RSPEED 255
#define LM1 D1  // GPIO5
#define LM2 D2  // GPIO4
#define RM1 D3  // GPIO0
#define RM2 D4  // GPIO2
#define ENA D5  // GPIO14
#define ENB D6  // GPIO12

ESP8266WebServer server(80);
const char* ssid = "your_ssid";     // Replace with your WiFi SSID
const char* password = "your_pass"; // Replace with your WiFi password

void forward() {
  digitalWrite(LM1, HIGH);
  digitalWrite(LM2, LOW);
  digitalWrite(RM1, HIGH);
  digitalWrite(RM2, LOW);
  analogWrite(ENA, LSPEED);
  analogWrite(ENB, RSPEED);
  server.send(200, "text/plain", "Moving Forward");
}

void backward() {
  digitalWrite(LM1, LOW);
  digitalWrite(LM2, HIGH);
  digitalWrite(RM1, LOW);
  digitalWrite(RM2, HIGH);
  analogWrite(ENA, LSPEED);
  analogWrite(ENB, RSPEED);
  server.send(200, "text/plain", "Moving Backward");
}

void left() {
  digitalWrite(LM1, LOW);
  digitalWrite(LM2, HIGH);
  digitalWrite(RM1, HIGH);
  digitalWrite(RM2, LOW);
  analogWrite(ENA, LSPEED);
  analogWrite(ENB, RSPEED);
  server.send(200, "text/plain", "Turning Left");
}

void right() {
  digitalWrite(LM1, HIGH);
  digitalWrite(LM2, LOW);
  digitalWrite(RM1, LOW);
  digitalWrite(RM2, HIGH);
  analogWrite(ENA, LSPEED);
  analogWrite(ENB, RSPEED);
  server.send(200, "text/plain", "Turning Right");
}

void stop() {
  digitalWrite(LM1, LOW);
  digitalWrite(LM2, LOW);
  digitalWrite(RM1, LOW);
  digitalWrite(RM2, LOW);
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
  server.send(200, "text/plain", "Stopped");
}

void handleRoot() {
  String html = "<html><head><style>";
  html += "button{padding:15px 30px;margin:10px;font-size:20px;}";
  html += "body{text-align:center;font-family:sans-serif;}";
  html += "</style></head><body>";
  html += "<h1>Web Controlled Car</h1>";
  html += "<button onclick=\"location.href='/forward'\">Forward</button><br>";
  html += "<button onclick=\"location.href='/left'\">Left</button>";
  html += "<button onclick=\"location.href='/right'\">Right</button><br>";
  html += "<button onclick=\"location.href='/backward'\">Backward</button><br>";
  html += "<button onclick=\"location.href='/stop'\">Stop</button>";
  html += "</body></html>";
  server.send(200, "text/html", html);
}

void setup() {
  Serial.begin(115200);
  pinMode(LM1, OUTPUT);
  pinMode(LM2, OUTPUT);
  pinMode(RM1, OUTPUT);
  pinMode(RM2, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi...");
  }
  Serial.print("Connected. IP: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/forward", forward);
  server.on("/backward", backward);
  server.on("/left", left);
  server.on("/right", right);
  server.on("/stop", stop);

  server.begin();
}

void loop() {
  server.handleClient();
}

```
