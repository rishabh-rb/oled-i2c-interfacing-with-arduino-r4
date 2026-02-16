# OLED I2C Interfacing with Arduino UNO R4 (PlatformIO)

This repository demonstrates driving an SSD1306-based I2C OLED display from an Arduino UNO R4 (PlatformIO project).

Checklist
- [x] Framework: Arduino (PlatformIO)
- [x] Target board: Arduino UNO R4 WiFi (PlatformIO board id: `uno_r4_wifi`)
- [x] IDE used: CLion with PlatformIO (see notes about version)
- [x] Libraries used (with versions specified in `platformio.ini`)
- [x] Code walk-through for `void setup()` and `void loop()`
- [x] Wiring / pin mapping for the OLED (I2C)
- [x] Author metadata

Project summary

- Framework: Arduino (via PlatformIO)
- PlatformIO environment: `[env:uno_r4_wifi]` (see `platformio.ini`)
- Platform (PlatformIO): `renesas-ra` (Renesas RA family MCU)
- Board: `uno_r4_wifi` (Arduino UNO R4 WiFi)

IDE and tools

- IDE used: CLion (JetBrains CLion) with PlatformIO project support.
  - NOTE: No CLion version is recorded in the repository. Please replace the placeholder below with your local CLion version if you want an exact version recorded.
  - Suggested entry (edit this README): CLion version: <your-CLion-version-here>
- PlatformIO Core (CLI) and the PlatformIO extension in CLion are used to build and upload the firmware. PlatformIO configuration is in `platformio.ini`.

PlatformIO configuration (key values from `platformio.ini`)
- environment: `uno_r4_wifi`
- platform: `renesas-ra`
- board: `uno_r4_wifi`
- framework: `arduino`
- monitor_speed: `9600`
- monitor_port / upload_port: `COM15` (Windows example in this project file — change to your OS port, e.g. `/dev/ttyACM0` or `/dev/tty.usbserial-XXXXX` on macOS/Linux)

Libraries (as used by the sketch)
- Wire (built-in Arduino I2C library)
- Adafruit GFX Library (platformio.ini pinpoints `adafruit/Adafruit GFX Library@^1.12.4`)
- Adafruit SSD1306 (platformio.ini: `adafruit/Adafruit SSD1306@^2.5.16`)

Files of interest
- `src/main.cpp` — main sketch. This file contains the OLED initialization and example text output.

Quick wiring for a typical I2C SSD1306 OLED module

Most SSD1306 I2C modules expose 4 pins (sometimes 5 if RESET is present). Typical connections to the UNO R4 WiFi are:
- VCC  -> 3.3V or 5V (check your OLED module voltage requirements; many modules accept 3.3V–5V)
- GND  -> GND
- SDA  -> SDA (board SDA pin; on many Arduino boards this is shared with analog pin A4, but on UNO R4 use the labeled SDA pin on the board or consult the board pinout)
- SCL  -> SCL (board SCL pin; on UNO R4 use the labeled SCL pin)
- (optional) RST -> any free digital pin, or leave connected to the module's onboard pull-up if present

Notes about I2C pins
- The sketch uses the Wire library, so the MCU's default I2C hardware SDA/SCL pins are used. On the UNO R4 family these are available on the dedicated SDA / SCL pins on the board header—consult the official UNO R4 pinout if you need exact header locations.

What `src/main.cpp` does (high level)

- Libraries included:
  - `Arduino.h` — core Arduino API
  - `Wire.h` — I2C master library
  - `Adafruit_GFX.h` — graphics primitives (text, shapes)
  - `Adafruit_SSD1306.h` — SSD1306 OLED driver
- Constants defined in the sketch:
  - `SCREEN_WIDTH` = 128
  - `SCREEN_HEIGHT` = 64
  - `OLED_ADDR` = 0x3C (I2C address of the OLED module; change if your module uses a different address such as 0x3D)
- `Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT);` constructs the display object.

void setup() — what it does (step-by-step)
1. `Serial.begin(9600);` — opens the serial monitor at 9600 bps (matching `monitor_speed` in `platformio.ini`). This is useful for debug output.
2. `display.begin(SSD1306_SWITCHCAPVCC, OLED_ADDR)` — initializes the SSD1306 display over I2C using the given address. If initialization fails the sketch prints "OLED not found" to the serial monitor and halts in an infinite loop.
3. `display.clearDisplay();` — clears the internal framebuffer.
4. `display.setTextSize(1);` — sets text scale to 1 (default size).
5. `display.setTextColor(SSD1306_WHITE);` — sets the text color (monochrome display uses WHITE for lit pixels).
6. `display.setCursor(0, 0);` — positions the cursor to the top-left corner.
7. `display.println("Arduino UNO R4");` etc. — writes three lines of example text into the display buffer.
8. `display.display();` — pushes the buffer to the OLED so the text becomes visible.

void loop() — what it does
- Currently the function is empty (contains only a comment `// write your code here`). This means after the initial display update in `setup()` the device remains idle. You can extend `loop()` to update the display periodically, read sensors, respond to inputs, etc.

Pin usage summary (from the current sketch)
- I2C: uses the MCU's I2C hardware via the Wire library — SDA / SCL pins on the UNO R4 board.
- Serial: uses the USB serial (Serial) at 9600 bps for debug prints.

How to build and upload (PlatformIO)
1. From CLion with PlatformIO plugin: open the project, select the `[env:uno_r4_wifi]` environment, Build and Upload using the CLion/PlatformIO UI.
2. From the command line (PlatformIO CLI):

   pio run -e uno_r4_wifi
   pio run -e uno_r4_wifi -t upload

(Adjust `upload_port` in `platformio.ini` or pass `-P /dev/ttyXXX` if your serial port differs.)

Assumptions and notes
- Assumed IDE: CLion (project file path suggests CLionProjects). If you use a different editor (VS Code, PlatformIO IDE), update the README accordingly.
- The `platformio.ini` in this project contains `monitor_port = COM15` and `upload_port = COM15` which is a Windows-style port. On macOS or Linux change those to your device port (e.g. `/dev/tty.usbmodemXXXX`).

Author / file metadata
- @author: rishabhbarnwal
- date: 2026-02-16
- time: 12:00 (local)  -- replace with your exact local time if desired
- file: `src/main.cpp`
- framework: Arduino (PlatformIO)
- purpose: Demonstrate basic I2C interfacing of an SSD1306 OLED display with Arduino UNO R4 and show simple text on the screen.
- brief: Example sketch that initializes an SSD1306 128x64 OLED display over I2C and prints three text lines.

Next steps / suggestions
- Update `loop()` to show dynamic content (e.g., sensor readings, time, animations).
- Add a README section with exact CLion and PlatformIO versions if you need reproducible environment details.
- If your OLED has address 0x3D or requires a reset pin, update `OLED_ADDR` and wiring accordingly.

If you'd like, I can:
- add example code to `loop()` that shows a counter or sensor readings,
- or detect and record your CLion / PlatformIO versions automatically (I can run the relevant commands if you want me to).

