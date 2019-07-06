## Class 7: Simple kitchen timer program
### 1. Load in the analog slider program from Class 6

### 2. Attach the push button and debouncing code from Class 4
- Copied code lines:
  - `int buttonSwitchState = 0;`
  - `long timerStart = 0;`
  - `bool buttonState = true;`
  - `bool prevButtonState = true;`
  - `pinMode(3, INPUT_PULLUP);`

### 3. Copy the state machine function for debouncing from Class 4 and remove all references to 'counter'
- Uncomment the "successfull press" serial print message:
  -`Serial.println("successful press");`
- Comment out the print statement for the analogRead:
  - `Serial.print("mapPotentiometer: ");`
  - `Serial.println(mapPotentiometer, BIN);`
- Attach the state machine by inserting this line of code into the main loop
  - `buttonStateMachine();`
- Copy the state machine from Class 4:
  - `void buttonStateMachine(){`
  - `switch(buttonSwitchState){`
  - `case 0:`
  - `prevButtonState = buttonState;`
  - `buttonState = digitalRead(3);`
  - `if(prevButtonState==1 && buttonState==0){buttonSwitchState=1;}`
  - `break;`
  - `case 1:`
  - `timerStart = millis();`
  - `buttonSwitchState = 2;`
  - `break;`
  - `case 2:`
  - `prevButtonState = buttonState;`
  - `buttonState = digitalRead(3);`
  - `if (prevButtonState==0 && buttonState==1){buttonSwitchState=0;}`
  - `if (millis() - timerStart > 5){buttonSwitchState = 3;}`
  - `break;`
  - `case 3:`
  - `Serial.println("successful press");`
  - `buttonSwitchState = 0;`
  - `break;}}`
- (Upload, move potentiometer, click button, watch LEDs and Serial monitor)
- Notice how we can do 2 things at once: Turning on LEDs with the potentiometer and clicking a button to send a message to the serial monitor
- That's the power of using state machines and not including long delay() statements






