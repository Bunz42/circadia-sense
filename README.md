# The Circadia Sense

> An intelligent circadian lighting system that automatically adjusts LED color temperature throughout the day to support your natural sleep-wake cycle.

## Overview

The Circadia Sense is an Arduino-based circadian lighting controller that synchronizes your indoor lighting with the natural progression of daylight. By automatically transitioning LED colors from energizing blue tones in the morning, to neutral white during the afternoon, and warm amber in the evening, this system helps regulate your circadian rhythm and improve sleep quality.

The system features gesture-based brightness control using an ultrasonic sensor, real-time clock synchronization, and an LCD display for time and mode information.

## Features

- **Automatic Circadian Lighting**: Smoothly transitions between three lighting modes throughout the day
- **Daytime (7:00 AM - 5:00 PM)**: Calm blue light to promote alertness and energy
- **Afternoon (5:00 PM - 9:00 PM)**: Bright white light for optimal visibility
- **Night (9:00 PM - 7:00 AM)**: Warm amber/orange light to encourage melatonin production

- **👋 Gesture-Based Brightness Control**: Adjust LED brightness by moving your hand over the ultrasonic sensor (5-25cm range)

- **⏰ Real-Time Clock Integration**: Automatic time tracking using Arduino R4 Minima's built-in RTC

- **📺 LCD Display**: Shows current time (HH:MM format) and active lighting mode

- **🌊 Smooth Color Transitions**: Gradual 30-minute fades between lighting modes prevent abrupt changes

## Hardware Requirements

### Core Components
- **Arduino R4 Minima** (with built-in RTC)
- **WS2812B NeoPixel LED Strip** (60 LEDs)
- **16x2 LCD Display** (parallel interface)
- **HC-SR04 Ultrasonic Distance Sensor**
- **10kΩ Potentiometer** (for LCD contrast adjustment)
- **5V External Power Supply** (for LED strip)

### Wiring Specifications

| Component | Pin | Arduino Pin |
|-----------|-----|-------------|
| LCD | RS | 7 |
| LCD | E | 8 |
| LCD | D4 | 9 |
| LCD | D5 | 10 |
| LCD | D6 | 11 |
| LCD | D7 | 12 |
| LED Strip | Data | 6 |
| Ultrasonic Sensor | TRIG | 5 |
| Ultrasonic Sensor | ECHO | 3 |

**Important**: The LED strip must be powered by an external 5V power supply with its ground connected to the Arduino's ground.

## Software Dependencies

Install the following libraries via the Arduino IDE Library Manager:

1. **Adafruit NeoPixel** (by Adafruit) - For controlling WS2812B LED strips
2. **LiquidCrystal** (built-in) - For LCD display control
3. **RTC** (built-in for Arduino R4 Minima) - For real-time clock functionality

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/Bunz42/The-Circadia-Sense.git
cd The-Circadia-Sense
```

### 2. Hardware Setup
1. Wire all components according to the pin configuration table above
2. Connect the external 5V power supply to the LED strip
3. Adjust the LCD contrast potentiometer until text is clearly visible

### 3. Upload the Main Program
1. Open `198_Automatic_Lighting_Pod/198_Automatic_Lighting_Pod.ino` in Arduino IDE
2. **Set the RTC time** (critical first-time setup):
   ```cpp
   // Uncomment these lines in the code:
   RTCTime startTime(21, Month::NOVEMBER, 2025, 16, 00, 00, DayOfWeek::FRIDAY, SaveLight::SAVING_TIME_ACTIVE);
   RTC.setTime(startTime);
   ```
3. Edit the date and time to match your current time
4. Upload to Arduino
5. **Re-comment the RTC setup lines and upload again** (prevents time reset on every reboot)

## Usage

### Normal Operation
Once programmed and powered on, the system operates autonomously:
- LEDs automatically transition between modes based on time of day
- Move your hand 5-25cm above the ultrasonic sensor to adjust brightness
- LCD displays current time and active mode

### Demo Mode
For testing and demonstrations, use the `198_Circadian_Light_Controller_Demo` sketch:
- Cycles through all three lighting modes every 10 seconds
- Features faster 3-second color transitions
- Displays simulated time on LCD

### Testing Individual Components
- **LED Strip Test**: Run `LED_test/LED_test.ino` to verify LED strip wiring and power
- **LCD Test**: Run `lcd_test/lcd_test.ino` to verify LCD wiring and contrast

## Project Structure

```
The-Circadia-Sense/
├── 198_Automatic_Lighting_Pod/          # Main circadian lighting controller
│   └── 198_Automatic_Lighting_Pod.ino
├── 198_Circadian_Light_Controller_Demo/ # Demo version with fast mode cycling
│   └── 198_Circadian_Light_Controller_Demo.ino
├── LED_test/                             # LED strip hardware test
│   └── LED_test.ino
├── lcd_test/                             # LCD display hardware test
│   └── lcd_test.ino
├── circadia_sense.mp4                    # Demo video
└── README.md                             # This file
```

## Customization

### Adjust Lighting Schedule
Modify the timing constants in the main sketch:
```cpp
#define HOUR_DAY_START 7        // Start of daytime mode
#define HOUR_AFTERNOON_START 17 // Start of afternoon mode
#define HOUR_NIGHT_START 21     // Start of night mode
```

### Change Color Schemes
Customize the RGB color values:
```cpp
const RGBColor COLOR_DAYTIME = {0, 10, 50};       // Calm Blue
const RGBColor COLOR_AFTERNOON = {255, 255, 255}; // Bright White
const RGBColor COLOR_NIGHT = {255, 140, 0};       // Warm Amber
```

### Adjust Fade Duration
Control the transition speed between modes:
```cpp
#define FADE_DURATION_MS 1800000UL // 30 minutes (default)
// #define FADE_DURATION_MS 10000UL  // 10 seconds (for testing)
```

## How It Works

1. **Time Tracking**: The Arduino R4 Minima's built-in RTC maintains accurate time
2. **Mode Detection**: Every minute, the system checks the current hour and determines the appropriate lighting mode
3. **Color Interpolation**: When a mode change occurs, a linear interpolation algorithm gradually fades from the current color to the target color over 30 minutes
4. **Brightness Control**: The ultrasonic sensor continuously monitors distance and smoothly adjusts LED brightness using a fade algorithm
5. **Display Update**: The LCD refreshes every second to show current time and mode

## Authors

- **Raymond Hao**
- **Alex Li**
- **Ethan Sun**
- **Sidney Ruan**

## Acknowledgments

- Linear interpolation algorithm: Google Gemini Pro
- Brightness mapping and smoothing: Google Gemini
- Adafruit NeoPixel library: Adafruit Industries

## Demo

Check out `circadia_sense.mp4` for a demonstration of the system in action!

## License

This project is open source and available for educational and personal use.

---

**Note**: This project was developed as a Human Interface Device (HID) demonstration for circadian lighting control. The system is designed to support healthy sleep patterns through intelligent automated lighting.
