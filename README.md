
# 🦾 Arduino Servo move and Replayer

This project controls a servo motor using push buttons and records its position history. Once the user presses the "Replay" button, the servo replays the recorded movement.


## 👷‍♂️ Tinkercad link
- https://www.tinkercad.com/things/2P2sipGfPus-bodacious-crift-kup/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard

## 📷 Image of the circuit 
<img width="637" height="719" alt="Screenshot 2025-07-19 050213" src="https://github.com/user-attachments/assets/8209148f-60f7-4e43-b40c-58be3bb49535" />


 ## 🎞️ Demonstration video
 https://github.com/user-attachments/assets/a1e755a4-ac09-46df-8241-e1ea5bb23133



## 🧰 Components Used

- Arduino UNO
- 4x Servo motor (connected to pin 11)
- 3x Push buttons (Move Forward, Move Backward, Replay)
- 3x 10kΩ pull-down resistors
- Breadboard & jumper wires



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

## 👨‍💻 Code
```cpp
#include <Servo.h>

Servo servo_9;
int currentPos = 90;  // Start at center

const int minPos = 0;
const int maxPos = 180;

int ifgood1 = 0;
int ifgood2 = 0;
int TIME = 0;

 int historySize = 900;        // Max number of positions to store
int positionHistory[900];  // Array to store positions
int historyIndex = 0;              // Tracks the current index

void setup() {
  servo_9.attach(11, 500, 2500);
  servo_9.write(currentPos);
  logPosition(currentPos);

  pinMode(7, INPUT);  // Replay button
  pinMode(8, INPUT);  // Move forward
  pinMode(9, INPUT);  // Move backward

  
  
  // Optional debug
  // Serial.begin(9600);
}

void loop() {
  ifgood1 = digitalRead(8);
  ifgood2 = digitalRead(9);
  TIME = digitalRead(7);

  if (ifgood1 == LOW) {
    for (int i = 0; i < 30; i++) {
      if (currentPos < maxPos) {
        currentPos++;
        servo_9.write(currentPos);
        logPosition(currentPos);
        delay(15);
      }
    }
  }

  if (ifgood2 == LOW) {
    for (int i = 0; i < 30; i++) {
      if (currentPos > minPos) {
        currentPos--;
        servo_9.write(currentPos);
        logPosition(currentPos);
        delay(15);
      }
    }
  }

  // Replay movement when TIME button is pressed
  if (TIME == LOW) {
    for (int i = 0; i < historyIndex; i++) {
      servo_9.write(positionHistory[i]);
      delay(15);  // Same delay used during logging
    }
  }
}

// Store position in history array, rolling over if needed
void logPosition(int pos) {
  positionHistory[historyIndex] = pos;
  historyIndex++;
  if (historyIndex >= historySize) {
    historyIndex = 0;  // Wrap around (circular buffer)
  }

  // Optional:
  // Serial.print("Logged: ");
  // Serial.println(pos);
}
```
