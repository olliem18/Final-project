## Description
My final project is a backup sensor that beeps faster the closer you are to it. I used HC - SRO4 ultrasonic distance sensor, and a key studio buzzer connected to my raspberry Pi with a standard breadboard and T cobbler. 

## Materials 
- Raspberry pi 4 
- T cobbler
- Three 20 row breadboards
- HC -SRO4 ultrasonic distance sensor
- Key studio buzzer 
- 9 Male to Male jumper wires
- 4 female to male wires
- 
## WIRING


![IMG_1050](https://github.com/olliem18/Final-project/assets/143106944/43094b4b-16e2-4c1c-8181-de643c6da180)


## Code
```python

import RPi.GPIO as GPIO
import time
GPIO.setmode(GPIO.BCM)

TRIG = 4
ECHO = 17
GPIO_BUZZER = 13

print ("Distance Measurement In Progress")

GPIO.setup(TRIG,GPIO.OUT)
GPIO.setup(ECHO,GPIO.IN)
GPIO.setup(GPIO_BUZZER, GPIO.OUT)
try:
        while True:

                GPIO.output(TRIG, False)
                print ("waiting for sensor to settle")
                time.sleep(0.1)

                GPIO.output(TRIG, True)
                time.sleep(0.00001)
                GPIO.output(TRIG, False)

                while GPIO.input(ECHO)==0:
                        pulse_start = time.time()

                while GPIO.input(ECHO)==1:
                        pulse_end = time.time()
                        pulse_duration = pulse_end - pulse_start

                        distance = pulse_duration * 17150

                        distance = round(distance, 2)
                        dist = distance
                if dist <= 40 and dist >= 20:
                        GPIO.output(GPIO_BUZZER,GPIO.HIGH)
                        time.sleep(0.5)
                        GPIO.output(GPIO_BUZZER,GPIO.LOW)
                        time.sleep(0.5)
                elif dist <= 19 and dist >= 10:
                        GPIO.output(GPIO_BUZZER,GPIO.HIGH)
                        time.sleep(0.2)
                        GPIO.output(GPIO_BUZZER,GPIO.LOW)
                        time.sleep(0.2)
                elif dist < 10:
                        GPIO.output(GPIO_BUZZER,GPIO.HIGH)
                        time.sleep(0.01)
                elif dist > 40:
                        GPIO.output(GPIO_BUZZER,GPIO.LOW)
                else:
                        GPIO.output(GPIO_BUZZER,GPIO.LOW)
                # Reset by pressing CTRL + C

                print ("Distance:",dist,"cm")

except KeyboardInterrupt: # If there is a KeyboardInterrupt (when you press ctrl+c), exit the program and cleanup
        print("Cleaning up!")
        gpio.cleanup()

```

