## Class 7: Simple kitchen timer program
### 1. Load in the analog slider program from Class 6 (file 5)

### 2. Attach the push button and debouncing code from Class 4
- Copied code lines:
  - `int buttonSwitchState = 0;`
  - `long timerStart = 0;`
  - `bool buttonState = true;`
  - `bool prevButtonState = true;`
  - `pinMode(3, INPUT_PULLUP);`

### 3. Copy the state machine function for debouncing from Class 4 and remove all references to 'counter'
- Uncomment the "successfull press" serial print message:
  - `//Serial.println("successful press");`
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
### 4. Make variables for switching between "set time mode" and "countdown mode"
- New code lines:
  - `bool setTimerMode = true;`
  - `setTimerMode = !setTimerMode;`
  - `Serial.println(setTimerMode);`
- Comment out the other serial print statement
  - `Serial.println("successful press");`
- (Upload, click button, watch serial monitor)
### 5. Make if statements for the 2 modes
- Add this if and else statement over the lines pertaining to turning the LEDs on like this:
  - `if(setTimerMode == true){`
  - `if(bitRead(mapPotentiometer, 0) == 1){digitalWrite(5, HIGH);}`
  - `else{digitalWrite(5, LOW);}`
  - `if(bitRead(mapPotentiometer, 1) == 1){digitalWrite(8, HIGH);}`
  - `else{digitalWrite(8, LOW);}`
  - `if(bitRead(mapPotentiometer, 2) == 1){digitalWrite(11, HIGH);}`
  - `else{digitalWrite(11, LOW);}}`
  - `else{}`
- (Upload, move potentiometer, click button, watch LEDs)
- Notice how you can click the button once to stop the potentiometer from changing the LEDs, then click again to have them change again
### 6. Make variables for counting down the timer when not in "set timer mode"
- New code lines:
  - `int timerCountDown = 0;`
  - `long countDownTimerStart = 0;`
  - `if((millis()-countDownTimerStart)>500)`
  - `{Serial.println(timerCountDown);`
  - `countDownTimerStart = millis();`
  - `timerCountDown = timerCountDown - 1;}`
  - `timerCountDown = mapPotentiometer;`
- Comment out:
  - `Serial.println(setTimerMode);`
- (Upload, press button, move slider, watch LEDs and serial monitor)
- Notice how the serial monitor counts down the number the LEDs were on once entering the "countdown mode"
