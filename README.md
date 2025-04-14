# Cyber-Secure Smart Lock Home Protection

## Table of Contents

1.  [Project Overview](#project-overview)
2.  [Features](#features)
3.  [Hardware Requirements](#hardware-requirements)
4.  [Software Requirements](#software-requirements)
5.  [Wiring Diagram](#wiring-diagram)
6.  [Installation](#installation)
7.  [Usage](#usage)
8.  [Code Explanation](#code-explanation)
9.  [Future Enhancements](#future-enhancements)
10. [Contributing](#contributing)
11. [License](#license)
12. [Acknowledgments](#acknowledgments)

## 1. Project Overview <a name="project-overview"></a>

This project implements a basic keypad-based security system using an Arduino Uno. It allows users to enter a password via a 4x4 keypad. The system provides feedback through 
an OLED display, an LED, and a buzzer. An RTC (Real-Time Clock) module is included in the code for potential future time-based features.

## 2. Features <a name="features"></a>

* **Keypad Input:** Accepts password input from a 4x4 numeric keypad.
* **Password Entry Masking:** Displays asterisks (\*) on the OLED screen as the password is entered.
* **Password Submission:** Uses the '#' key to submit the entered password.
* **Backspace/Clear:** Uses the '\*' key to delete the last entered character.
* **Correct Password Feedback:**
    * Displays "Access Granted!" on the OLED.
    * Turns on an LED for 2 seconds.
* **Incorrect Password Feedback:**
    * Displays "CYBER DEPARTMENT", "MONITORING", and "NOTIFICATION SENDS TO HOUSE OWNER" on the OLED for 5 seconds.
    * Sounds a two-beep warning using the buzzer.
* **Startup Sound:** The buzzer briefly rings upon system startup.
* **RTC Integration:** Includes the `RTClib` library and initializes the RTC module (DS3231) for potential future time-related functionalities.

## 3. Hardware Requirements <a name="hardware-requirements"></a>

* Arduino Uno
* 4x4 Keypad Matrix
* 0.96 inch OLED Display (SSD1306, 128x64)
* DS3231 Real-Time Clock (RTC) Module
* LED
* 220 Ohm Resistor (for the LED)
* Buzzer
* Jumper Wires
* Breadboard (optional, but recommended for prototyping)

## 4. Software Requirements <a name="software-requirements"></a>

* Arduino IDE installed on your computer.
* The following Arduino libraries (installable via the Arduino Library Manager):
    * `Adafruit GFX Library` by Adafruit
    * `Adafruit SSD1306` by Adafruit
    * `Keypad` by Mark Stanley and Alexander Brevig
    * `RTClib` by Adafruit

## 5. Wiring Diagram <a name="wiring-diagram"></a>

**Keypad to Arduino:**

* Keypad Row 1: Arduino Digital Pin 9
* Keypad Row 2: Arduino Digital Pin 8
* Keypad Row 3: Arduino Digital Pin 7
* Keypad Row 4: Arduino Digital Pin 6
* Keypad Column 1: Arduino Digital Pin 5
* Keypad Column 2: Arduino Digital Pin 4
* Keypad Column 3: Arduino Digital Pin 3
* Keypad Column 4: Arduino Digital Pin 2

**OLED Display (SSD1306) to Arduino:**

* OLED VCC: Arduino 3.3V or 5V (check your module)
* OLED GND: Arduino GND
* OLED SDA: Arduino Analog Pin A4 (SDA)
* OLED SCL: Arduino Analog Pin A5 (SCL)

**DS3231 RTC Module to Arduino:**

* RTC VCC: Arduino 5V (or 3.3V, check your module)
* RTC GND: Arduino GND
* RTC SDA: Arduino Analog Pin A4 (SDA)
* RTC SCL: Arduino Analog Pin A5 (SCL)

**LED to Arduino:**

* LED Anode (+, longer leg): Arduino Digital Pin 10 (through a 220 Ohm resistor)
* LED Cathode (-, shorter leg): Arduino GND

**Buzzer to Arduino:**

* Buzzer Positive (+, often marked): Arduino Digital Pin 13
* Buzzer Negative (-): Arduino GND

## 6. Installation <a name="installation"></a>

1.  **Install Arduino IDE:** If you haven't already, download and install the Arduino IDE from the official website ([https://www.arduino.cc/software](https://www.arduino.cc/software)).
2.  **Install Libraries:**
    * Open the Arduino IDE.
    * Go to **Sketch** > **Include Library** > **Manage Libraries...**
    * Search for and install the following libraries:
        * `Adafruit GFX Library`
        * `Adafruit SSD1306`
        * `Keypad`
        * `RTClib`
3.  **Connect Hardware:** Wire the components to your Arduino Uno according to the [Wiring Diagram](#wiring-diagram).
4.  **Upload Code:**
    * Copy the provided Arduino code into a new sketch in the Arduino IDE.
    * Ensure your Arduino Uno is connected to your computer via USB.
    * Select the correct board ("Arduino Uno") and port in the Arduino IDE (**Tools** menu).
    * Click the **Upload** button to upload the code to your Arduino.

## 7. Usage <a name="usage"></a>

1.  Once the code is uploaded, the system will start. You should hear a brief beep from the buzzer.
2.  The OLED screen will display "Enter Password:".
3.  Use the keypad to enter the password digits. Each digit entered will be displayed as an asterisk (\*) on the OLED.
4.  If you make a mistake, press the '\*' key to delete the last entered character.
5.  After entering the full password, press the '#' key to submit it.
6.  **Correct Password (default: "2025"):**
    * The OLED will display "Access Granted!".
    * The LED will turn on for 2 seconds.
    * The system will then return to the "Enter Password:" prompt.
7.  **Incorrect Password:**
    * The OLED will display "CYBER DEPARTMENT", "MONITORING", and "NOTIFICATION SENDS TO HOUSE OWNER" for 5 seconds.
    * The buzzer will sound two short beeps.
    * The system will then return to the "Enter Password:" prompt.

## 8. Code Explanation <a name="code-explanation"></a>

* **Library Includes:** Includes necessary libraries for OLED display, keypad, and RTC.
* **Defines:** Defines constants for screen dimensions and the OLED reset pin.
* **OLED Object:** Creates an instance of the `Adafruit_SSD1306` class.
* **Keypad Configuration:** Defines the keypad layout, row pins, and column pins, and creates a `Keypad` object.
* **Password Management:** Stores the correct password and the entered password, along with an index to track the entered characters.
* **Pin Definitions:** Defines pin numbers for the LED and buzzer.
* **`setup()` Function:**
    * Initializes serial communication.
    * Sets the LED and buzzer pins as outputs and turns them off initially.
    * Initializes the RTC module.
    * Initializes the OLED display and displays the initial "Enter Password:" message.
    * Plays a short startup sound on the buzzer.
* **`loop()` Function:**
    * Continuously checks for key presses on the keypad.
    * If a key is pressed:
        * If it's '#', it calls the `checkPassword()` function.
        * If it's '\*', it simulates a backspace by decrementing the password index.
        * If it's a digit and the password length is not exceeded, it adds the key to the `enteredPassword` array and displays an asterisk on the OLED.
* **`checkPassword()` Function:**
    * Compares the `enteredPassword` with the `correctPassword`.
    * If they match, it displays "Access Granted!" and turns on the LED.
    * If they don't match, it displays the "CYBER DEPARTMENT MONITORING" messages and activates the buzzer.
* **`clearEnteredPassword()` Function:** Resets the `enteredPassword` array and the `passwordIndex`.

## 9. Future Enhancements <a name="future-enhancements"></a>

* **Persistent Password Storage:** Store the password in EEPROM so it persists even after power loss.
* **Password Change Functionality:** Implement a way for the user to change the password.
* **Multiple User Support:** Allow for multiple passwords.
* **Time-Based Features:** Utilize the RTC to log access attempts with timestamps or implement time-based security features.
* **Locking Mechanism Control:** Integrate with a physical locking mechanism (e.g., a solenoid) to control access.
* **More Sophisticated Error Handling:** Implement a limited number of incorrect attempts before locking the system.
* **User Interface Improvements:** Enhance the OLED display with more informative messages.

## 10. Contributing <a name="contributing"></a>

Contributions to this project are welcome. Feel free to fork the repository, make your changes, and submit a pull request. Please ensure your code follows good coding practices and includes comments to explain your changes.

## 11. License <a name="license"></a>

This project is licensed under the [Specify your license here, e.g., MIT License]. See the `LICENSE` file for more details.

## 12. Acknowledgments <a name="acknowledgments"></a>

* The Arduino team for the fantastic platform and IDE.
* Adafruit Industries for their excellent GFX and SSD1306 libraries, and the RTClib library.
* Mark Stanley and Alexander Brevig for the useful Keypad library.
* The open-source community for their valuable resources and contributions.

---

This README file provides a comprehensive overview of your project, making it easy for others (and your future self) to understand and use it. Remember to replace "[Specify your license here, e.g., MIT License]" with the actual license you choose for your project. Good luck with your project!
