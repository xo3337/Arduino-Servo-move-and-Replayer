
# 🦾 Arduino Servo move and Replayer

This project controls a servo motor using push buttons and records its position history. Once the user presses the "Replay" button, the servo replays the recorded movement.

## 📷 Overview

- Image of the circuit:  
<img width="637" height="719" alt="Screenshot 2025-07-19 050213" src="https://github.com/user-attachments/assets/8209148f-60f7-4e43-b40c-58be3bb49535" />


- Demonstration video:
- https://github.com/user-attachments/assets/a1e755a4-ac09-46df-8241-e1ea5bb23133




## 🧰 Components Used

- Arduino UNO
- 1x Servo motor (connected to pin 11)
- 3x Push buttons (Move Forward, Move Backward, Replay)
- 3x 10kΩ pull-down resistors
- Breadboard & jumper wires

> The setup shown also allows extension to control more servos using similar logic (4 are shown in the image, but only one is coded).

## 📌 Pin Configuration

| Pin | Function         |
|-----|------------------|
| 11  | Servo signal     |
| 7   | Replay button    |
| 8   | Move forward     |
| 9   | Move backward    |

## 🧠 Code Functionality

### Basic Behavior
- The servo starts at the middle position (90°).
- **Button on Pin 8** moves the servo forward by 30 steps (up to 180°).
- **Button on Pin 9** moves it backward by 30 steps (down to 0°).
- Every new position is logged into an array.
- **Button on Pin 7** triggers a replay of the logged motion history.

### Logging
- Up to 900 positions are logged.
- If the array fills, it wraps around like a circular buffer.

## 💾 `logPosition()` Function
Each time the servo moves, the new position is saved:
```cpp
void logPosition(int pos) {
  positionHistory[historyIndex] = pos;
  historyIndex++;
  if (historyIndex >= historySize) {
    historyIndex = 0; // Wrap around
  }
}
```

## ▶️ How to Use

1. Upload the code to the Arduino UNO.
2. Connect USB power.
3. Press:
   - **Move Forward** to rotate the servo toward 180°.
   - **Move Backward** to rotate it toward 0°.
   - **Replay** to move through all previously recorded positions in order.

## 🛠️ Future Enhancements
- Add support for multiple servos.
- Implement EEPROM storage to retain history after power-off.
- Visual display for position feedback.
