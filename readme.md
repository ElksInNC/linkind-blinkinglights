Situation:
-3 Linkind switches in a devgroup.  All three controlling the kitchen can lights
-Kitchen cans (and other lights) are in a time-of-day based motion detection routine.
-There are times that I want all the lights on and for them to stay on for some period of time even if occupancy goes to vacant in the room.
- Would like visual confirmation on the switch that the action requested has been recorded and is in force.

Tools:
-LEDs 1 & 2 decoupled from power state.  LEDs controlled with rules
-Devgroup for 3 switches controlling power state - AFAIK devgroup doesn't control LEDs... (standing by to be corrected).
-Custom compile to support MQTT subscribe on the switches.
-HA Automation to manage timing and override of occupancy motion detection routine (yes, yes ... NR to the rescue! ;-)

Solution:
-Rules manage LED state
-Rules do looping mechanism and a "bit flip" to flash lights
-Subscribe used to remotely set the LED value from HA
-Publish on switch used to send switch multi-press info to HA

Working scenarios
1) Power on - LED Green
   Power off - LED Ren

2) HOLD all lights on (scene) for 30 min - signaled by top button double press
   Add another 30 min by additional double press.  Keep adding up to 4 segments (2 hours)
   Acknowledge that the request was made by blinking LEDs One blink sequence.
   Set LED RED & GREEN (Yellow) on to show that there is an override in place.

3) HOLD all lights on "indefinitely" (actually until midnight) - triple press top button
   Acknowledge with two blinking LED sequences
   Yellow LED showing override state

4) Cancel override - revert to normal light state for time of day - double press bottom button.
   Single flash loop then revert LED to current state.

5) Override all lights off until midnight - Triple press bottom button.
   Double flash loop then Yellow LED

