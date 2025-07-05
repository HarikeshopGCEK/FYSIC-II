
---
# Overview
1. The line follower is a bot that follows a dark line that is drawn over a White surface
2. Complicated line followers can be made by using PID Control and Weighted Sum methods
3. Here we are going to use a simple Logic for Line followe

---
# Components Needed

1. IR Sensor
2. Arduino Uno
3. Jumper wires
4. Batteries etc.

---

# Logic

- If black line detected on the left side turn to left
- If on the right turn right
- If both side detects white move forward
- If both side detects white move forwards

---

# Code For The Bot
```cpp
#include <Arduino.h>

#define LM1 2

#define LM2 3

#define RM1 4

#define RM2 5

#define ENA 10

#define ENB 11

#define LIR 6

#define RIR 7

#define LSPEED 255

#define RSPEED 255

void setup()

{

// put your setup code here, to run once:

Serial.begin(9600);

pinMode(LM1, OUTPUT);

pinMode(LM2, OUTPUT);

pinMode(RM1, OUTPUT);

pinMode(RM2, OUTPUT);

pinMode(ENA, OUTPUT);

pinMode(ENB, OUTPUT);

pinMode(LIR, INPUT);

pinMode(RIR, INPUT);

}

  

void loop()

{

// put your main code here, to run repeatedly:

int l = digitalRead(LIR);

int r = digitalRead(RIR);

if (l == LOW && r == LOW)

{

forward();

}

else if (l == LOW && r == HIGH)

{

right();

}

else if (l == HIGH && r == LOW)

{

left();

}

else

{

stop();

}

}

  

void forward()

{

digitalWrite(LM1, HIGH);

digitalWrite(LM2, LOW);

digitalWrite(RM1, HIGH);

digitalWrite(RM2, LOW);

analogWrite(ENA, LSPEED);

analogWrite(ENB, RSPEED);

}

void backward()

{

digitalWrite(LM1, LOW);

digitalWrite(LM2, HIGH);

digitalWrite(RM1, LOW);

digitalWrite(RM2, HIGH);

analogWrite(ENA, LSPEED);

analogWrite(ENB, RSPEED);

}

void left()

{

digitalWrite(LM1, LOW);

digitalWrite(LM2, HIGH);

digitalWrite(RM1, HIGH);

digitalWrite(RM2, LOW);

analogWrite(ENA, LSPEED);

analogWrite(ENB, RSPEED);

}

void right()

{

digitalWrite(LM1, HIGH);

digitalWrite(LM2, LOW);

digitalWrite(RM1, LOW);

digitalWrite(RM2, HIGH);

analogWrite(ENA, LSPEED);

analogWrite(ENB, RSPEED);

}

void stop()

{

digitalWrite(LM1, LOW);

digitalWrite(LM2, LOW);

digitalWrite(RM1, LOW);

digitalWrite(RM2, LOW);

analogWrite(ENA, 0);

analogWrite(ENB, 0);

}
```

---
