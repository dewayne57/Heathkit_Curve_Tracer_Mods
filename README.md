# Heathkit_Curve_Tracer_Mods
Several key modifications to the Heathkit IT-x121 series semiconductor curve tracers (IT-1121 and IT-3121).  These 
semiconductor curve tracers essentially are the same with minor differences.  The schematic is virtually identical 
between the two. 

The schematic for the IT-1121 [![IT-1121](schematic.png)](schematic.png) is included for reference. 

## Sweep Generator Modification
The sweep generator as designed used a 120Hz pulse derived from the secondary power supply to clock a 7490 decade 
counter.  The output from the counter was a 4-bit BCD value that was used to turn on or off 4 different current 
sources.  The result was a maximum of 9 discrete curves.  To set the number of curves desired to any value between 
0 and 9, the circuit picked some of the current off the energized sources and used that to determine when to reset
the counter.  This reset point was determined using a 7.5Mohm potentiometer.  Very high resistance values were used
in this circuit to reduce loading of the current generators.  

The 7.5MOhm potentiometer is no longer available and is not a standard size.  Getting a replacement would require
a custom part which would be very expensive.  Also, the use of a potentiometer to set the reset point makes setting 
the number of curves somewhat inaccurate and requires a bit of guesswork/fiddling with the setting. 

The 120Hz sweep clock works, but is relatively slow and can result in some flickering.  

This modification replaces the current sense/feedback solution with a digital one. This eliminates the current 
sense, the need for extremely high value resistors, and the potentiometer.  It replaces the potentiometer with 
a 10 position rotary switch which corresponds to the 0-9 desired curves.  The rotary switch provides discrete 
positive detents that provide for accurate setting of the desired number of curves.

The solution also replaces the 120Hz clock source with a 400Hz clock to drive the counter.  The entire circuit 
simply replaces the 7490 on the circuit board and can be built as a plug-in daughter card. 

The 7490 BCD count is applied to a 74LS42 BCD-to-decimal decoder that outputs 1 of 10 signals.  These are then 
routed to the rotary switch for selection.  The selected signal (1 of 10) is then inverted and used to reset 
the 74LS90 counter when that count has been reached.

To install this modification,
1. Remove the 7490 (IC6) from the board, leave the socket.  The updated daughter card will plug into this 
   socket. 
2. Remove the current feedback components (R46, R47, R48, R49, R51, and R52).
3. Remove the reset circuit (R43, R44, R45, Q17, and Q18). 
4. Remove the pulse generator components (R41, R42, Q5, and Q6).
5. Build the daughter card and use a 14-pin jumper from J1 to the IC6 socket on the main board. 

## Logic Supply Regulation 
The original circuit only provided a simple, low-current +5V supply that was not terribly well regulated because
there was only the single 7490 counter chip.  With the modification above, the demand for the +5V supply 
increases, and the existing circuit is no longer sufficient. 

This modification removes the voltage drop resistor and replaces it with a LM7805 linear regulator.  The +15V 
side of the resistor is tied to the input of the LM7805.  The output of the LM7805 replaces the +5V side of 
the dropping resistor (the resistor is removed).  The ground leg of the LM7805 is tied to an available ground 
connection on the circuit. 

To install this modification,
1. Remove the dropping resistor R37. 
2. Wire the input pin of an LM7805 to the +15.5V side of R37.  This is best done by placing the LM7805 
   off-board and using wires to connect it to the board.  I recommend mounting the LM7805 to the aluminum 
   chassis somewhere (insulated) for heat dissipation. 
3. Wire the output of the LM7805 to the +5 side of R37.  Note, leave capacitor C7 in circuit. 
4. Wire the ground lead of the LM7805 to the ground side of C7. 