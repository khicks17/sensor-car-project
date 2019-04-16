#include "mbed.h"
#include "ContinuousServo.h"
#include "TCS3472_I2C.h"

Serial pc(USBTX,USBRX);
int d,conversion;
conversion = .0064; //conversion factor (V/in)
AnalogIn sonar(p19); //ultrasonic sensor @ 6.4 mV/inch

ContinuousServo left (p23);
ContinuousServo right (p26); //servos

int main() {
while (1){
d = sonar*(1/conversion); //d is distance in inches

if (d<=5){
break;
}

left = 0;
right = 0;
}
}





