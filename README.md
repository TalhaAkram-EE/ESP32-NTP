**ESP32 NTP Clock**	

Welcome to the ESP32 NTP Clock project! This project demonstrates how to create a network-connected clock using an ESP32 microcontroller and Network Time Protocol (NTP) for accurate timekeeping. It's a perfect project for those looking to explore IoT and microcontroller programming.
**Table of Contents**
•	Overview
•	Components Required
•	Circuit Diagram
•	Code
•	Setup and Installation
•	How It Works
•	Video Tutorial
**Overview**
This project uses the ESP32 to fetch the current time from an NTP server and display it on an LCD screen. The ESP32 connects to a Wi-Fi network to access the NTP server, ensuring precise timekeeping.
Components Required
•	ESP32 Development Board
•	LCD Display with I2C interface (16x2)
•	Breadboard and Jumper Wires
**Circuit Diagram**
The circuit is simple to set up:
1	Connect the VCC and GND of the LCD to the 5V and GND pins of the ESP32.
2	Connect the SDA and SCL pins of the LCD to the corresponding pins on the ESP32 (usually GPIO 21 for SDA and GPIO 22 for SCL).
**Code**
Here's the code you need to upload to your ESP32:
#include <WiFi.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C LCD = LiquidCrystal_I2C(0x27, 16, 2);

#define NTP_SERVER     "pool.ntp.org"
#define UTC_OFFSET     0
#define UTC_OFFSET_DST 0

void spinner() {
  static int8_t counter = 0;
  const char* glyphs = "\xa1\xa5\xdb";
  LCD.setCursor(15, 1);
  LCD.print(glyphs[counter++]);
  if (counter == strlen(glyphs)) {
    counter = 0;
  }
}

void printLocalTime() {
  struct tm timeinfo;
  if (!getLocalTime(&timeinfo)) {
    LCD.setCursor(0, 1);
    LCD.println("Connection Err");
    return;
  }

  LCD.setCursor(8, 0);
  LCD.println(&timeinfo, "%H:%M:%S");

  LCD.setCursor(0, 1);
  LCD.println(&timeinfo, "%d/%m/%Y   %Z");
}

void setup() {
  Serial.begin(115200);

  LCD.init();
  LCD.backlight();
  LCD.setCursor(0, 0);
  LCD.print("Connecting to ");
  LCD.setCursor(0, 1);
  LCD.print("WiFi ");

  WiFi.begin("Wokwi-GUEST", "", 6);
  while (WiFi.status() != WL_CONNECTED) {
    delay(250);
    spinner();
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());

  LCD.clear();
  LCD.setCursor(0, 0);
  LCD.println("Online");
  LCD.setCursor(0, 1);
  LCD.println("Updating time...");

  configTime(UTC_OFFSET, UTC_OFFSET_DST, NTP_SERVER);
}

void loop() {
  printLocalTime();
  delay(250);
}

**Setup and Installation**
1	Assemble the Circuit: Follow the circuit diagram to connect all components.
2	Upload the Code: Connect your ESP32 to your computer and upload the provided code using the Arduino IDE.
3	Configure Wi-Fi Credentials: The code is set to connect to the "Wokwi-GUEST" Wi-Fi network. Update the WiFi.begin() function if you are using a different network.
4	Test the System: Once the ESP32 connects to Wi-Fi, it will fetch the current time from the NTP server and display it on the LCD screen.
**How It Works**
The ESP32 connects to a specified Wi-Fi network and retrieves the current time from an NTP server. The time is then displayed on the LCD screen. This ensures that the clock remains accurate as it continuously synchronizes with the NTP server.
**Video Tutorial**
Watch our YouTube tutorial:https://youtu.be/Ou7evYw241w to see a step-by-step guide on setting up and testing the ESP32 NTP clock.
For more information, visit Polonium Technologies:https://poloniumtechnologies.com/.

