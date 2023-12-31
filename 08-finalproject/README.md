## 01 Project Description
### Floral Whispers
- Chinese Version
  
Floral Whispers是一个模拟花朵与传粉者间生态系统的交互装置。在自然界中，许多花朵短暂释放花香（VOCs）以吸引传粉者，并与周围花朵交流，以提高自身的授粉率。在该装置中，观者将扮演传粉者的   角色，与花朵进行互动。 当观者轻触任意一朵花瓣时，周围的一朵花就会随机亮起并释放香味，以引诱观者前去“传粉”，带来触觉、视觉和嗅觉的三重感官。

- English Version

"Floral Whispers" is an interactive installation that simulates the ecosystem between flowers and pollinators. In nature, many flowers briefly release    floral scents (VOCs) to attract pollinators and communicate with surrounding flowers to enhance their pollination rates. In this installation,         participants take on the role of pollinators and interact with the flowers. When participants lightly touches any petal, a random flower nearby will illuminate and release a fragrance, enticing participants to engage in "pollination," providing a multisensory experience involving touch, sight, and smell.

## 02 Photos
![DSC_5348](https://github.com/HHY1111/PCB-class-2023/assets/144415019/2195ed28-a411-4b39-bf9e-b183aa1e6ebd)
![DSC_5141](https://github.com/HHY1111/PCB-class-2023/assets/144415019/d05139fd-b6e6-47cf-bf01-5fdc138ef6e0)
![DSC_5117](https://github.com/HHY1111/PCB-class-2023/assets/144415019/88aa7a18-446f-4165-85a1-dd0016b3f7a5)
![DSC_5459](https://github.com/HHY1111/PCB-class-2023/assets/144415019/87b9d9a7-e724-4669-8a00-159458a18e61)



## 03 JLC Page 
https://pro.lceda.cn/editor#id=6e413bd88f104af395d7be28f594d4ff,tab=*ee70fd1d6c6a48a7a4b13d5598dd4678@6e413bd88f104af395d7be28f594d4ff
![WechatIMG2579](https://github.com/HHY1111/PCB-class-2023/assets/144415019/d63ed615-0bb1-44f0-bfe9-f7013f71d64c)


## 04 Source Code
```
#include <CapacitiveSensor.h>
#include <Servo.h>

//Servo servo1; // 伺服电机1，对应针脚3
//Servo servo2; // 伺服电机2，对应针脚5
//Servo servo3; // 伺服电机3，对应针脚6

Servo servos[] = {Servo(), Servo(), Servo()}; // 伺服电机数组
int servo[] = {3, 5, 6}; // 伺服电机对应的针脚数组

int pos = -20;    // variable to store the servo position
//int i;   //0，1，2行列随机数
int a;   //电机针脚随机数

long randNumber;        //Randomly generate random numbers from 1-5，using a random seed: and a different random number each time the program is launche

CapacitiveSensor   cs_7_8 = CapacitiveSensor(7,8);        // 10M resistor between pins 7 & 8, pin 2 is sensor pin, add a wire and or foil if desired, flower 1
CapacitiveSensor   cs_7_9 = CapacitiveSensor(7,9);        // 10M resistor between pins 7 & 9, pin 2 is sensor pin, add a wire and or foil if desired, flower 2
CapacitiveSensor   cs_7_10 = CapacitiveSensor(7,10);        // 10M resistor between pins 7 & 10, pin 2 is sensor pin, add a wire and or foil if desired, flower 3

const int rows = 3;
const int columns = 2;
int pins[rows][columns] = {   // int pins【共3组数组】【每个数组中有2个数】，使用数组来对应servo和LED的针脚
 {3, 2},    // flower1 servo针脚PIN3，对应LED针脚PIN2
 {5, 4},    // flower2 servo针脚PIN5，对应LED针脚PIN4
 {6, 11},     // flower3 servo针脚PIN6，对应LED针脚PIN11
 // 3针脚对应2针脚，5针脚对应4针脚，6针脚对应11针脚
};

void setup()                    
{
   randomSeed(analogRead(0));   //初始化随机数
   for (int a = 0; a < 3; a++) {
    servos[a].attach(servo[a]);
  }
   servos[a].attach(servo[a]);
   servos[a].write(-10);    //定义servo的初识角度是-10

   //servos[3].attach(3);
   //servos[5].attach(5);
   //servos[6].attach(6);
    

   //servos[3].write(-10);    //定义servo的初识角度是-10
   //servos[5].write(-10);    //定义servo的初识角度是-10
   //servos[6].write(-10);    //定义servo的初识角度是-10

   cs_7_8.set_CS_AutocaL_Millis(0xFFFFFFFF);     // turn off autocalibrate on channel 1 - just as an example
   cs_7_9.set_CS_AutocaL_Millis(0xFFFFFFFF);     // turn off autocalibrate on channel 1 - just as an example
   cs_7_10.set_CS_AutocaL_Millis(0xFFFFFFFF);     // turn off autocalibrate on channel 1 - just as an example

   pinMode(2, OUTPUT);  // LED output，flower 1
   pinMode(4, OUTPUT);  // LED output，flower 2
   pinMode(11, OUTPUT);  // LED output，flower 3

   Serial.begin(9600);

}

void loop()                    
{

    long start = millis();
    long Sensor1 =  cs_7_8.capacitiveSensor(30);
    long Sensor2 =  cs_7_9.capacitiveSensor(30);
    long Sensor3 =  cs_7_10.capacitiveSensor(30);

    Serial.print(Sensor1);                  // print sensor output 1
    Serial.print("\t");
    Serial.print(Sensor2);                  // print sensor output 2
    Serial.print("\t");
    Serial.print(Sensor3);                // print sensor output 3
    Serial.println("\t");

   if (Sensor1 < 30) {    // trigger a solenoid1，flower 1, !! "99" need to be tested!!
        do {
        randNumber = random(0, 3); // 生成3到6之间的随机数
        } while (randNumber == 0); // 如果随机数等于3或4，重新生成
        // Generate random numbers between 3-6 except for 4、3

        int i = randNumber;  
        int servoNumber= i;                         // 将randNumber付值给i
        int LEDpin = pins[i][1];                      // pin[i][1]
        
        Serial.println(randNumber);
        Serial.print("SERVO");
        Serial.println(pins[i][0]);
        Serial.print("LED");
        Serial.println(LEDpin);

         //for (int i = 0; i < 3; i++) {
         //if (servopin == servo[a]) {
             servos[i].write(80);
            digitalWrite(LEDpin,HIGH);
            delay(300);                             
            servos[i].write(-10);
            digitalWrite(LEDpin,LOW);    
            delay(50);  
         }

   else if (Sensor2 < 30) {    // trigger a solenoid2，flower 2
       do {
        randNumber = random(0, 3); // 生成3到6之间的随机数
        } while (randNumber == 1); // 如果随机数等于3或4，重新生成
        // Generate random numbers between 3-6 except for 4、3

 int i = randNumber;  
        int servoNumber= i;                         // 将randNumber付值给i
        int LEDpin = pins[i][1];                      // pin[i][1]
        
        Serial.println(randNumber);
        Serial.print("SERVO");
        Serial.println(pins[i][0]);
        Serial.print("LED");
        Serial.println(LEDpin);

         //for (int i = 0; i < 3; i++) {
         //if (servopin == servo[a]) {
            servos[i].write(80);
            digitalWrite(LEDpin,HIGH);
            delay(300);                             
            servos[i].write(-10);
            digitalWrite(LEDpin,LOW);    
            delay(50);  
         }
    
    
     else if (Sensor3 < 30) {                          // trigger a capacitive sensor，flower 3
       do {
        randNumber = random(0, 3); // 生成3到6之间的随机数
        } while (randNumber == 0); // 如果随机数等于3或4，重新生成
        // Generate random numbers between 3-6 except for 4、3

 int i = randNumber;  
        int servoNumber= i;                         // 将randNumber付值给i
        int LEDpin = pins[i][1];                      // pin[i][1]
        
        Serial.println(randNumber);
        Serial.print("SERVO");
        Serial.println(pins[i][0]);
        Serial.print("LED");
        Serial.println(LEDpin);

         //for (int i = 0; i < 3; i++) {
         //if (servopin == servo[a]) {
            servos[i].write(80);
            digitalWrite(LEDpin,HIGH);
            delay(300);                             
            servos[i].write(-10);
            digitalWrite(LEDpin,LOW);    
            delay(50);  
         }
    
    
    delay(100);                             // arbitrary delay to limit data to serial port 
}
```
