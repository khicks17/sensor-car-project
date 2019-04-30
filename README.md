#include "mbed.h"
#include "ContinuousServo.h"
#include "TCS3472_I2C.h"
#include "Tach.h"


ContinuousServo left(p23);
ContinuousServo right(p26);
Tach tLeft(p17,64);
Tach tRight(p13,64);
TCS3472_I2C rgb_sensor(p9, p10);
float l_rpm;
float r_rpm;
float r_speed;
float l_speed;
float l_expected;
float r_expected;
char go;  
char stop;  
float d;
float conversion=.0064;
AnalogIn sonar(p19); //ultrasonic sensor @ 6.4 mV/inch
                


int main() {
    
    l_speed=(0.15);
    r_speed=(-0.15);
    
    wait(4);
    
    while(1) {
        
        
        d = sonar.read()*(1/conversion); //d is distance in inches
        printf("Distance (inches) is %f\n\r", d);
        wait(0.5);

        l_rpm = tLeft.getSpeed();
        r_rpm = tRight.getSpeed();
        
        l_expected = (0.3533)*(l_speed * l_speed) + (-0.1725)*(l_speed) + (0.0526);
        r_expected = (0.2609)*(r_speed * r_speed) + (-0.0793)*(r_speed) + (0.0327);
        
        
        l_speed = (0.0005)*(l_speed - l_expected) + l_speed;
        r_speed = (0.0005)*(r_speed - r_expected) + r_speed;
        
        left.speed(l_speed);
        right.speed(r_speed); 
        
        printf("Left duty cycle is %f and Left RPM is %f\n",l_speed, l_rpm);
        
        printf("Right duty cycle is %f and Right RPM is %f\n",r_speed, r_rpm);
        
        if (d<=9){
          break;
}
}
 left.stop();
   right.stop();
}
