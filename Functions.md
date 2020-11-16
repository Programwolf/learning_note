[toc]

# Digital I/O

## digitalRead()

[原文链接](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalread/)

>数字I/O口**读输入**电平函数

**Description**

Reads the value from a specified(指定) digital pin, either `HIGH` or `LOW`.

**Syntax**

`digitalRead(pin)`

**Parameters**

`pin`: the Arduino pin number you want to read

**Returns**

`HIGH` or `LOW`

**Example Code**

Sets pin 13 to the same value as pin 7, declared(声明) as an input.
```Arduino
int ledPin = 13;  // LED connected to digital pin 13
int inPin = 7;    // pushbutton connected to digital pin 7
int val = 0;      // variable to store(存储) the read value

void setup() {
  pinMode(ledPin, OUTPUT);  // sets the digital pin 13 as output
  pinMode(inPin, INPUT);    // sets the digital pin 7 as input
}

void loop() {
  val = digitalRead(inPin);   // read the input pin
  digitalWrite(ledPin, val);  // sets the LED to the button's value
}
```



---------------------------------------------



## digitalWrite()

[原文链接](https://www.arduino.cc/reference/en/language/functions/digital-io/digitalwrite/)

>数字I/O口**输出**电平定义函数

**Description**

Write a `HIGH` or a `LOW` value to a digital pin.

If the pin has been configured as an `OUTPUT` with `pinMode()`, its voltage(电压) will be set to the corresponding(相应的) value: 5V (or 3.3V on 3.3V boards) for `HIGH`, 0V (ground) for `LOW`.

If the pin is configured as an `INPUT`, `digitalWrite()` will enable (`HIGH`) or disable (`LOW`) the internal pullup on the input pin. It is recommended to set the `pinMode()` to `INPUT_PULLUP` to enable the internal pull-up resistor. See the [Digital Pins](#digital-pins) tutorial for more information.

If you do not set the `pinMode()` to `OUTPUT`, and connect an LED to a pin, when calling `digitalWrite(HIGH)`, the LED may appear dim. Without explicitly setting `pinMode()`, `digitalWrite()` will have enabled the internal pull-up resistor, which acts like a large current-limiting(限流) resistor.

**Syntax**

`digitalWrite(pin, value)`

**Parameters**

`pin`: the Arduino pin number.
`value`: `HIGH` or `LOW`.

**Returns**

Nothing

**Example Code**

The code makes the digital pin 13 an `OUTPUT` and toggles(切换) it by alternating(交替) between `HIGH` and `LOW` at one second pace(步速).
```Arduino
void setup() {
  pinMode(13, OUTPUT);    // sets the digital pin 13 as output
}

void loop() {
  digitalWrite(13, HIGH); // sets the digital pin 13 on
  delay(1000);            // waits for a second
  digitalWrite(13, LOW);  // sets the digital pin 13 off
  delay(1000);            // waits for a second
}
```

**tips:**

- **pullup** 上拉电阻，上拉就是将不确定的信号通过一个电阻钳位在高电平，电阻同时起限流作用。下拉同理，也是将不确定的信号通过一个电阻钳位在低电平。上拉是对器件输入电流，下拉是输出电流



---------------------------------------------



## pinMode()

[原文链接](https://www.arduino.cc/reference/en/language/functions/digital-io/pinmode/)

>数字I/O口输入输出模式定义函数

**Description**

Configures the specified pin to behave either as an input or an output. See the [Digital Pins](#digital-pins) page for details on the functionality of the pins.

As of Arduino 1.0.1, it is possible to enable the internal(内部的) pullup resistors(上拉电阻) with the mode `INPUT_PULLUP`. Additionally, the `INPUT` mode explicitly(明确的) disables the internal pullups.

**Syntax**(语法)

`pinMode(pin, mode)`

**Parameters**

`pin`: the Arduino pin number to set the mode of.
`mode`: `INPUT`, `OUTPUT`, or `INPUT_PULLUP`. See the **Digital Pins** page for a more complete description of the functionality.

**Returns**

Nothing

**Example Code**

The code makes the digital pin 13 `OUTPUT` and Toggles(切换) it `HIGH` and `LOW`
```Arduino
void setup() {
  pinMode(13, OUTPUT);    // sets the digital pin 13 as output
}

void loop() {
  digitalWrite(13, HIGH); // sets the digital pin 13 on
  delay(1000);            // waits for a second
  digitalWrite(13, LOW);  // sets the digital pin 13 off
  delay(1000);            // waits for a second
}
```



-----------------------------------------------

-----------------------------------------------        



# Analog I/O

## analogRead()

[原文链接](https://www.arduino.cc/reference/en/language/functions/analog-io/analogread/)

>模拟I/O口读输入函数

**Description**

Reads the value from the specified analog pin. Arduino boards contain a multichannel(多信道), 10-bit analog to digital converter(转换器). This means that it will *map* input voltages between 0 and the operating voltage(5V or 3.3V) *into*(map...into... 映射) integer values between 0 and 1023. On an Arduino UNO, for example, this yields(产生) a resolution between readings of: 5 volts / 1024 units or, 0.0049 volts (4.9 mV) per unit. 

The input range can be changed using [analogReference()](#analogreference), while the resolution can be changed (only for Zero, Due and MKR boards) using **analogReadResolution()**.

<!--给analogReadResolution()设置锚点-->

BOARD                   |	OPERATING VOLTAGE	| USABLE PINS	| MAX RESOLUTION
------------------------|-------------------|-------------|---------------
Mega, Mega2560, MegaADK |       5 Volts     |  A0 to A14  | 10 bits

**Syntax**

`analogRead(pin)`

**Parameters**

`pin`: the name of the analog input pin to read from (A0 to A5 on most boards, A0 to A6 on MKR boards, A0 to A7 on the Mini and Nano, A0 to A15 on the Mega).

**Returns**

The analog reading on the pin. Although it is limited to the resolution of the analog to digital converter (0-1023 for 10 bits or 0-4095 for 12 bits). Data type: `int`.

**Example Code**

The code reads the voltage on analogPin and displays it.
```Arduino
int analogPin = A3; // potentiometer wiper (middle terminal) connected to analog pin 3
                    // outside leads to ground and +5V
int val = 0;  // variable to store the value read

void setup() {
  Serial.begin(9600);           //  setup serial
}

void loop() {
  val = analogRead(analogPin);  // read the input pin
  Serial.println(val);          // debug value
}
```

----------------------------------------------

## analogReference()

[原文链接](https://www.arduino.cc/reference/en/language/functions/analog-io/analogreference/)

**Description**

Configures the reference voltage used for analog input (i.e.(id est 即) the value used as the top of the input range). The options are:

Arduino AVR Boards (Uno, Mega, Leonardo, etc.)

- **DEFAULT**: the default analog reference of 5 volts (on 5V Arduino boards) or 3.3 volts (on 3.3V Arduino boards)
- **INTERNAL**: a built-in reference, equal to 1.1 volts on the ATmega168 or ATmega328P and 2.56 volts on the ATmega32U4 and ATmega8 (not available on the Arduino Mega)
- **INTERNAL1V1**: a built-in 1.1V reference (Arduino Mega only)
- **INTERNAL2V56**: a built-in 2.56V reference (Arduino Mega only)
- **EXTERNAL**: the voltage applied to the AREF pin (0 to 5V only) is used as the reference.

**Syntax**

`analogReference(type)`

**Parameters**

`type`: which type of reference to use (see list of options in the description).

**Returns**

Nothing


----------------------------------------------

## analogWrite()

[原文链接](https://www.arduino.cc/reference/en/language/functions/analog-io/analogwrite/)

**Description**

Writes an analog value (**PWM** wave) to a pin. Can be used to light a LED at varying(变化的) brightnesses or drive a motor(马达) at various speeds. After a call to `analogWrite()`, the pin will generate(生成) a steady rectangular(矩形) wave of the specified *duty cycle*(工作周期) until the next call to `analogWrite()` (or a call to `digitalRead()` or `digitalWrite()`) on the same pin.

BOARD	| PWM PINS       | PWM FREQUENCY
------|----------------|--------------
Mega  |2 - 13, 44 - 46 | 490 Hz (pins 4 and 13: 980 Hz)

You do not need to call `pinMode()` to set the pin as an output before calling `analogWrite()`.
**The analogWrite function has nothing to do with the analog pins or the `analogRead()` function**.

**Syntax**

`analogWrite(pin, value)`

**Parameters**

`pin`: the Arduino pin to write to. Allowed data types: `int`.
`value`: the duty cycle: between 0 (always off) and 255 (always on). Allowed data types: int.

**Returns**

Nothing

**Example Code**

Sets the output to the LED proportional to the value read from the potentiometer.
```Arduino
int ledPin = 9;      // LED connected to digital pin 9
int analogPin = 3;   // potentiometer connected to analog pin 3
int val = 0;         // variable to store the read value
0
void setup() {
  pinMode(ledPin, OUTPUT);  // sets the pin as output
}

void loop() {
  val = analogRead(analogPin);  // read the input pin
  analogWrite(ledPin, val / 4); // analogRead values go from 0 to 1023, analogWrite values from 0 to 255
}
```

**tips:**

- [PWM](https://baike.baidu.com/item/%E8%84%89%E5%86%B2%E5%AE%BD%E5%BA%A6%E8%B0%83%E5%88%B6/10813756?fromtitle=PWM&fromid=3034961&fr=aladdin): Pulse width modulation 脉冲宽度调制
- [PWM wave](https://baike.baidu.com/item/PWM%E6%B3%A2%E5%BD%A2/2816630?fromtitle=PWM%20wave&fromid=24092976&fr=aladdin): Pulse width modulation wave 脉冲宽度调制波形




----------------------------------------------

----------------------------------------------


# Advanced I/O

<!--今日任务呀-->


## tone()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/tone/)

>`tone()`函数可以产生固定频率的PWM信号来驱动扬声器发声

**Description**

Generates a square wave of the specified frequency (and 50% duty cycle) on a pin. A duration(时长) can be specified, otherwise the wave continues until a call to `noTone()`. The pin can be connected to a *piezo buzzer*(压电式蜂鸣器) or other speaker to play tones.

**Only one tone can be generated at a time.** If a tone is already playing on a different pin, the call to `tone()` will have no effect. If the tone is playing on the same pin, the call will set its frequency.

Use of the `tone()` function will interfere(干扰) with PWM output on pins 3 and 11 (on boards other than the Mega).

It is not possible to generate tones lower than 31Hz. For technical(技术) details, see [Brett Hagman’s notes](https://github.com/bhagman/Tone#ugly-details).

**Syntax**

`tone(pin, frequency)`
`tone(pin, frequency, duration)`

**Parameters**

`pin`: the Arduino pin on which to generate the tone.
`frequency`: the frequency of the tone in hertz(Hz). Allowed data types: unsigned int.
`duration`: the duration of the tone in milliseconds (optional). Allowed data types: unsigned long.

**Returns**

Nothing


------------------------------------------------



## noTone()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/notone/)

>`noTone()`函数用来停止`tone()`函数发声。

**Description**

Stops the generation of a square wave triggered(引起) by `tone()`. Has no effect if no tone is being generated.

**Syntax**

`noTone(pin)`

**Parameters**

`pin`: the Arduino pin on which to stop generating the tone

**Returns**

Nothing

---------------------------------------------

## pulseIn()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/pulsein/)

>读引脚的脉冲信号

**Description**

Reads a pulse(脉冲) (either `HIGH` or `LOW`) on a pin. For example, if value is `HIGH`, `pulseIn()` waits for the pin to go from `LOW` to `HIGH`, starts timing(计时), then waits for the pin to go `LOW` and stops timing. Returns the length of the pulse in microseconds(微秒) or gives up and returns 0 if no complete pulse was received within the timeout.

The timing of this function has been determined empirically(以经验为主的) and will probably show errors in **longer** pulses. **Works on pulses from 10 microseconds to 3 minutes in length.**

**Syntax**

`pulseIn(pin, value)`
`pulseIn(pin, value, timeout)`

**Parameters**

`pin`: the number of the Arduino pin on which you want to read the pulse. Allowed data types: `int`.
`value`: type of pulse to read: either `HIGH` or `LOW`. Allowed data types: `int`.
`timeout` (optional): the number of microseconds to wait for the pulse to start; default is one second. Allowed data types: `unsigned long`.

**Returns**

The length of the pulse (in microseconds) or 0 if no pulse started before the timeout. Data type: `unsigned long`.

**Example Code**

The example prints the time duration of a pulse on pin 7.
```Arduino
int pin = 7;
unsigned long duration;

void setup() {
  Serial.begin(9600);
  pinMode(pin, INPUT);
}

void loop() {
  duration = pulseIn(pin, HIGH);
  Serial.println(duration);
}
```


--------------------------------------------------------------------------------



## pulseInLong()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/pulseinlong/)

**Description**

`pulseInLong()` is an alternative to `pulseIn()` which is better at handling(处理) long pulse and interrupt(中断) affected scenarios(场景).

Reads a pulse (either `HIGH` or `LOW`) on a pin. For example, if value is `HIGH`, `pulseInLong()` waits for the pin to go from `LOW` to `HIGH`, starts timing, then waits for the pin to go `LOW` and stops timing. Returns the length of the pulse in microseconds or gives up and returns 0 if no complete pulse was received within the timeout.

The timing of this function has been determined empirically and will probably show errors in **shorter** pulses. **Works on pulses from 10 microseconds to 3 minutes in length.** **This routine**(例程) **can be used only if interrupts are activated**(激活)**.** Furthermore(此外) the highest resolution is obtained with large intervals(间隔).

**Syntax**

`pulseInLong(pin, value)`
`pulseInLong(pin, value, timeout)`

**Parameters**

`pin`: the number of the Arduino pin on which you want to read the pulse. Allowed data types: `int`.
`value`: type of pulse to read: either `HIGH` or `LOW`. Allowed data types: `int`.
`timeout` (optional): the number of microseconds to wait for the pulse to start; default is one second. Allowed data types: `unsigned long`.

**Returns**

The length of the pulse (in microseconds) or 0 if no pulse started before the timeout. Data type: `unsigned long`.

**Example Code**
The example prints the time duration of a pulse on pin 7.
```Arduino
int pin = 7;
unsigned long duration;

void setup() {
  Serial.begin(9600);
  pinMode(pin, INPUT);
}

void loop() {
  duration = pulseInLong(pin, HIGH);
  Serial.println(duration);
}
```
---------------------------------------------

## shiftIn()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/shiftin/)

>将一个字节的数据通过移位的方式逐位输入

**Description**

Shifts(移位) in a byte of data one bit at a time. Starts from either the most (i.e. the leftmost) or least (rightmost) significant(有效的) bit. *For each bit, the clock pin is pulled high, the next bit is read from the data line, and then the clock pin is taken low.*(在输入数据时，Arduino首先在**时钟引脚**输出高电平，然后通过**数据输入引脚**读取一位数据，读取结束后**时钟引脚**将被Arduino设置为低电平。)

**If you’re interfacing with a device that’s clocked by rising edges, you’ll need to make sure that the clock pin is low before the first call to `shiftIn()`, e.g. with a call to `digitalWrite(clockPin, LOW)`.**
(如果与Arduino进行数据通讯的设备是在时钟引脚脉冲信号上升沿发送数据，请确保在调用`shiftIn()`前，应先通过`digitalWrite(clockPin, LOW)`语句，将时钟引脚设置为`LOW`。这样做是为了确保数据读取准确无误。)

Note: this is a software implementation(实现); Arduino also provides an SPI library that uses the hardware implementation, which is faster but only works on specific pins.

**Syntax**

`byte incoming = shiftIn(dataPin, clockPin, bitOrder)`

**Parameters**

`dataPin`: the pin on which to input each bit. Allowed data types: int.
`clockPin`: the pin to toggle to signal a read from **dataPin**.
`bitOrder`: which order to shift in the bits; either **MSBFIRST** or **LSBFIRST**. (Most Significant Bit First, or, Least Significant Bit First).

**Returns**

The value read. Data type: `byte`.

---------------------------------------------

## shiftOut()

[原文链接](https://www.arduino.cc/reference/en/language/functions/advanced-io/shiftout/)

>将一个字节的数据通过移位输出的方式逐位输出

**Description**

Shifts out a byte of data one bit at a time. Starts from either the most (i.e. the leftmost) or least (rightmost) significant bit. *Each bit is written in turn to a data pin, after which a clock pin is pulsed (taken high, then low) to indicate that the bit is available.*(在输出数据时，当一位数据写入数据输出引脚时，时钟引脚将输出脉冲信号，指示该位数据已被写入数据输出引脚等待读取。)

Note- **if you’re interfacing with a device that’s clocked by rising edges, you’ll need to make sure that the clock pin is low before the call to `shiftOut()`, e.g. with a call to `digitalWrite(clockPin, LOW)`.**

This is a software implementation; see also the SPI library, which provides a hardware implementation that is faster but works only on specific pins.

**Syntax**

`shiftOut(dataPin, clockPin, bitOrder, value)`

**Parameters**

`dataPin`: the pin on which to output each bit. Allowed data types: `int`.
`clockPin`: the pin to toggle once the dataPin has been set to the correct value. Allowed data types: `int`.
`bitOrder`: which order to shift out the bits; either **MSBFIRST** or **LSBFIRST**. (Most Significant Bit First, or, Least Significant Bit First).
`value`: the data to shift out. Allowed data types: `byte`.

**Returns**

Nothing

**Example Code**

For accompanying circuit, see the tutorial on controlling a 74HC595 shift register.
```Arduino
//**************************************************************//
//  Name    : shiftOutCode, Hello World                         //
//  Author  : Carlyn Maw,Tom Igoe                               //
//  Date    : 25 Oct, 2006                                      //
//  Version : 1.0                                               //
//  Notes   : Code for using a 74HC595 Shift Register           //
//          : to count from 0 to 255                            //
//****************************************************************

//Pin connected to ST_CP of 74HC595
int latchPin = 8;
//Pin connected to SH_CP of 74HC595
int clockPin = 12;
////Pin connected to DS of 74HC595
int dataPin = 11;

void setup() {
  //set pins to output because they are addressed in the main loop
  pinMode(latchPin, OUTPUT);
  
  /*
  使用`shiftOut()`函数前，数据引脚（`dataPin`）和时钟引脚（`clockPin`）必须
  先通过`pinMode()`指令设置为输出（`OUTPUT`）模式。
  */

  pinMode(clockPin, OUTPUT);    
  pinMode(dataPin, OUTPUT);
}

void loop() {
  //count up routine
  for (int j = 0; j < 256; j++) {
    //ground latchPin and hold low for as long as you are transmitting
    digitalWrite(latchPin, LOW);
    shiftOut(dataPin, clockPin, LSBFIRST, j);
    //return the latch pin high to signal chip that it
    //no longer needs to listen for information
    digitalWrite(latchPin, HIGH);
    delay(1000);
  }
}
```
**Notes and Warnings**

**The dataPin and clockPin must already be configured as outputs by a call to `pinMode()`.**
(使用`shiftOut()`函数前，数据引脚（`dataPin`）和时钟引脚（`clockPin`）必须先通过`pinMode()`指令设置为输出（`OUTPUT`）模式。)

**`shiftOut` is currently written to output 1 byte (8 bits)** so it requires a two step operation to output values larger than 255.

```Arduino
// Do this for MSBFIRST serial
int data = 500;
// shift out highbyte
shiftOut(dataPin, clock, MSBFIRST, (data >> 8));
// shift out lowbyte
shiftOut(dataPin, clock, MSBFIRST, data);

// Or do this for LSBFIRST serial
data = 500;
// shift out lowbyte
shiftOut(dataPin, clock, LSBFIRST, data);
// shift out highbyte
shiftOut(dataPin, clock, LSBFIRST, (data >> 8));
```




---------------------------------------------

---------------------------------------------

# Time

## millis()

[原文链接](https://www.arduino.cc/reference/en/language/functions/time/millis/)

>获取Arduino开机后运行的时间长度(毫秒)，最长50天

**Description**

Returns the number of milliseconds since the Arduino board began running the current program. This number will overflow (go back to zero), after approximately(大约) 50 days.

**Parameters**

None

**Returns**

Number of milliseconds since the program started (`unsigned long`)

Please note that the return value for `millis()` is an `unsigned long`, logic errors may occur if a programmer tries to do arithmetic(运算) with smaller data types such as int's. Even signed long may encounter errors as its maximum value is half that of its unsigned counterpart.
(就是不要将该数值与整型数据或其它数据类型进行运算，很可能会出现错误)

**Example**
```Arduino
unsigned long time;

void setup(){
  Serial.begin(9600);
}
void loop(){
  Serial.print("Time: ");
  time = millis();
  //prints time since program started
  Serial.println(time);
  // wait a second so as not to send massive amounts of data
  delay(1000);
}
```


```Arduino
/* 
 *  millisBlink
 * by 太极创客 (2017-11-21)
 * www.taichi-maker.com
 * 本示例程序使用millis()函数控制Arduino开发板内置LED的点亮和熄灭。
*/
unsigned long previousBlinkTime;
int blinkInterval = 1000; //LED闪烁时间间隔
bool toggle;

void setup() {
  pinMode(LED_BUILTIN, OUTPUT); 
  Serial.begin(9600);
}

void loop() {  
  unsigned long currentMillis = millis(); // 获取当前时间
  millisBlink(currentMillis);

  /* 由于使用millis()函数控制LED闪烁，所以loop函数中没有delay。
   * 于是我们可以在loop函数中流畅的执行其他操作。
  */

}

void millisBlink(unsigned long currentTime) { 
  //检查是否到达时间间隔
  if (currentTime - previousBlinkTime >= blinkInterval) {    //如果时间间隔达到了
    toggle = (toggle == 1) ? 0 : 1;    
    digitalWrite(LED_BUILTIN, toggle);                       //执行闪烁LED操作
    
    previousBlinkTime = currentTime;  // 将检查时间复位   
    
    Serial.print(F("toggle = "));Serial.println(toggle);          
  }  else if (currentTime - previousBlinkTime <= 0) {   // 如果millis时间溢出
    previousBlinkTime = currentTime;
  }
}
```



-------------------------------------------------------------------------------------------------------



## micros()

[原文链接](https://www.arduino.cc/reference/en/language/functions/time/micros/)

>获取Arduino开机后运行的时间长度(微秒)，最长70分钟

**Description**

Returns the number of microseconds since the Arduino board began running the current program. This number will overflow (go back to zero), after approximately 70 minutes. On 16 MHz Arduino boards (e.g. Duemilanove and Nano), this function has a resolution of four microseconds (i.e. the value returned is always a multiple of four). On 8 MHz Arduino boards (e.g. the LilyPad), this function has a resolution of eight microseconds.

Note: there are 1,000 microseconds in a millisecond and 1,000,000 microseconds in a second.

**Parameters**

None

**Returns**

Number of microseconds since the program started (`unsigned long`)

**Example**
```Arduino
unsigned long time;

void setup(){
  Serial.begin(9600);
}
void loop(){
  Serial.print("Time: ");
  time = micros();
  //prints time since program started
  Serial.println(time);
  // wait a second so as not to send massive amounts of data
  delay(1000);
}
```


-------------------------------------------------------------------------------------------------------


## delay()

[原文链接](https://www.arduino.cc/reference/en/language/functions/time/delay/)

>用于暂停程序运行。暂停时间可以由delay()函数的参数进行控制，单位是毫秒

**Description**

Pauses the program for the amount of time (in milliseconds) specified as parameter. (There are 1000 milliseconds in a second.)

**Syntax**

`delay(ms)`

**Parameters**

`ms`: the number of milliseconds to pause. Allowed data types: `unsigned long`.

**Returns**

Nothing

**Example Code**

The code pauses the program for one second before toggling the output pin.
```Arduino
int ledPin = 13;              // LED connected to digital pin 13

void setup() {
  pinMode(ledPin, OUTPUT);    // sets the digital pin as output
}

void loop() {
  digitalWrite(ledPin, HIGH); // sets the LED on
  delay(1000);                // waits for a second
  digitalWrite(ledPin, LOW);  // sets the LED off
  delay(1000);                // waits for a second
}
```

**Notes and Warnings**

While it is easy to create a blinking(闪烁) LED with the `delay()` function and many sketches use short delays for such tasks as switch debouncing, the use of `delay()` in a sketch(程序，作者真的谦虚) has significant drawbacks. No other reading of sensors, mathematical calculations, or pin manipulation can go on during the delay function, so in effect, it brings most other activity to a halt. For alternative approaches to controlling timing see the [Blink Without Delay](https://www.arduino.cc/en/Tutorial/BuiltInExamples/BlinkWithoutDelay)**(mark)** sketch, which loops, polling the `millis()` function until enough time has elapsed. More knowledgeable programmers usually avoid the use of `delay()` for timing of events longer than 10’s of milliseconds unless the Arduino sketch is very simple.

<!--学一下Blink Without Delay-->

Certain things do go on while the `delay()` function is controlling the Atmega chip, however, because **the delay function does not disable interrupts**. Serial communication that appears at the RX pin is recorded, PWM (analogWrite) values and pin states are maintained, and interrupts will work as they should.



-------------------------------------------------------------------------------------------------------


## delayMicroseconds()

[原文链接](https://www.arduino.cc/reference/en/language/functions/time/delaymicroseconds/)

>参数单位是微秒

**Description**

Pauses the program for the amount of time (in microseconds) specified by the parameter. There are a thousand microseconds in a millisecond and a million microseconds in a second.

Currently, **the largest value that will produce an accurate delay is 16383.** This could change in future Arduino releases. For delays longer than a few thousand microseconds, you should use `delay()` instead.

**Syntax**

`delayMicroseconds(us)`

**Parameters**

`us`: the number of microseconds to pause. Allowed data types: `unsigned int`.

**Returns**

Nothing

**Example Code**

The code configures pin number 8 to work as an output pin. It sends a train of pulses of approximately 100 microseconds period. The approximation is due to execution of the other instructions in the code.
```Arduino
int outPin = 8;               // digital pin 8

void setup() {
  pinMode(outPin, OUTPUT);    // sets the digital pin as output
}

void loop() {
  digitalWrite(outPin, HIGH); // sets the pin on
  delayMicroseconds(50);      // pauses for 50 microseconds
  digitalWrite(outPin, LOW);  // sets the pin off
  delayMicroseconds(50);      // pauses for 50 microseconds
}
```

**Notes and Warnings**

This function works very accurately in the range 3 microseconds and up(精度是3微秒). We cannot assure that delayMicroseconds will perform precisely for smaller delay-times.

As of Arduino 0018, `delayMicroseconds()` no longer disables interrupts.


-------------------------------------------------------------------------------------------------------

-------------------------------------------------------------------------------------------------------


# Communication
## Serial

[原文链接](https://www.arduino.cc/reference/en/language/functions/communication/serial/)

Used for **communication between the Arduino board and a computer** or other devices. All Arduino boards have at least one serial port(端口) (also known as a UART or USART), and some have several.

BOARD | USE CDC NAME | SERIAL PINS | SERIAL1 PINS | SERIAL2 PINS | SERIAL3 PINS
------|--------------|-------------|--------------|--------------|-------------
Mega  |              | 0(RX),1(TX) | 19(RX),18(TX)| 17(RX),16(TX)| 15(RX),14(TX)

You can use the Arduino environment’s built-in(内置的) serial monitor(监视器) to communicate with an Arduino board. Click the serial monitor button in the toolbar and select the same baud rate(波特率) used in the call to `begin()`.

```Arduino
void setup()
{
    Serial.begin(9600); // 打开串口，设置波特率为9600
}
```
Serial communication on pins TX/RX uses TTL logic levels (5V or 3.3V depending on the board). Don’t connect these pins directly to an RS232 serial port; they operate at +/- 12V and can damage your Arduino board.

**tips :**

- **baud** 波特 信息传输速率的单位
- **RX** Receiver接收器
- **TX** Transmitter发送器


--------------------------------------------------



## Stream

[原文链接](https://www.arduino.cc/reference/en/language/functions/communication/stream/)

Stream is the base class for character and binary based streams. **It is not called directly, but invoked**(调用) **whenever you use a function that relies on it.**

Stream defines the reading functions in Arduino. When using any core functionality that uses a `read()` or similar method, you can safely assume it calls on the Stream class. For functions like `print()`, Stream inherits(继承) from the Print class.


---------------------------------------------------


---------------------------------------------------



# Digital Pins

[原文链接](http://arduino.cc/en/Tutorial/DigitalPins)

The pins on the Arduino can be configured as either inputs or outputs. This document explains the functioning of the pins in those modes. While the title of this document refers to digital pins, it is important to note that vast majority(绝大多数) of Arduino (Atmega) analog pins, may be configured, and used, in exactly the same manner(方式) as digital pins.

**Properties of Pins Configured as INPUT**

Arduino (Atmega) pins default to inputs, so they don't need to be explicitly declared as inputs with `pinMode()` when you're using them as inputs. Pins configured this way are said to be in a **high-impedance state**(高阻态). Input pins make extremely small demands on the circuit(电路) that they are sampling(采样), equivalent(相当) to a series resistor of 100 megohm(兆欧) in front of the pin. This means that it takes very little current to move the input pin from one state to another, and can make the pins useful for such tasks as implementing(实现) a *capacitive touch sensor*(电容式触摸感应器), reading an LED as a *photodiode*(光电二极管), or reading an analog sensor with a scheme(方案) such as [RCTime](#rctime).

This also means however, that pins configured as pinMode(pin, INPUT) with nothing connected to them, or with wires connected to them that are not connected to other circuits, will report seemingly random changes in pin state, picking up electrical noise from the environment, or capacitively coupling(耦合) the state of a nearby pin.

**Pullup Resistors with pins configured as INPUT**

Often it is useful to steer(引导) an input pin to a known state if no input is present. This can be done by adding a pullup resistor (to +5V), or a pulldown resistor (resistor to ground) on the input. A 10K resistor is a good value for a pullup or pulldown resistor.

**Properties of Pins Configured as INPUT_PULLUP**

There are 20K pullup resistors built into the Atmega chip(芯片) that can be accessed from software. These built-in pullup resistors are accessed by setting the `pinMode()` as `INPUT_PULLUP`. This effectively inverts(反转) the behavior of the `INPUT` mode, where `HIGH` means the sensor is off, and `LOW` means the sensor is on.

The value of this pullup depends on the microcontroller(单片机) used. On most AVR-based boards, the value is guaranteed(保证) to be between 20kΩ and 50kΩ. On the Arduino Due, it is between 50kΩ and 150kΩ. For the exact(精确的) value, consult(查阅) the datasheet(数据表) of the microcontroller on your board.

When connecting a sensor to a pin configured with `INPUT_PULLUP`, the other end should be connected to ground. In the case of a simple switch, this causes the pin to read `HIGH` when the switch is open, and `LOW` when the switch is pressed.

The pullup resistors provide enough current to dimly(依稀的) light an LED connected to a pin that has been configured as an input. If LEDs in a project seem to be working, but very dimly, this is likely what is going on.

The pullup resistors are controlled by the same registers (internal chip memory locations) that control whether a pin is `HIGH` or `LOW`. Consequently(因此), a pin that is configured to have pullup resistors turned on when the pin is an `INPUT`, will have the pin configured as `HIGH` if the pin is then switched to an `OUTPUT` with `pinMode()`. This works in the other direction as well, and an output pin that is left in a `HIGH` state will have the pullup resistors set if switched to an input with `pinMode()`.

Prior(优先) to Arduino 1.0.1, it was possible to configure the internal pull-ups in the following manner:

```Arduino
pinMode(pin, INPUT);           // set pin to input
digitalWrite(pin, HIGH);       // turn on pullup resistors
```
**NOTE**: Digital pin 13 is harder to use as a digital input than the other digital pins because it has an LED and resistor attached(附着) to it that's soldered(焊接) to the board on most boards. If you enable its internal 20k pull-up resistor, it will hang at around 1.7V instead of the expected 5V because the onboard LED and series(串联) resistor pull the voltage level down, meaning it always returns `LOW`. If you must use pin 13 as a digital input, set its `pinMode()` to `INPUT` and use an external(外部的) pull down resistor.

**Properties of Pins Configured as OUTPUT**

Pins configured as `OUTPUT` with `pinMode()` are said to be in a low-impedance state(低阻态). This means that they can provide a substantial amount of current(电流) to other circuits. Atmega pins can source (provide positive current) or sink (provide negative current) up to 40 mA (milliamps) of current to other devices/circuits. This is enough current to brightly light up an LED (don't forget the series resistor), or run many sensors, for example, but not enough current to run most relays(继电器), solenoids(螺线管), or motors(电动机).

Short circuits on Arduino pins, or attempting(试图) to run high current devices from them, can damage or destroy the output transistors in the pin, or damage the entire Atmega chip. Often this will result in a "dead" pin in the microcontroller but the remaining chip will still function adequately(充分的). For this reason it is a good idea to connect `OUTPUT` pins to other devices with 470Ω or 1k resistors, unless maximum current draw from the pins is required for a particular application.


return to [digitalWrite()](#digitalwrite)

-------------------------------------------------------------

-------------------------------------------------------------


# RCTime

[原文链接](https://www.arduino.cc/en/Tutorial/RCtime/)

In situations where all of the Duino's A/D pins are used, RCtime is a workaround for reading any kind of resistive sensors on any digital pin.

This RCtime function duplicates(重复) the Basic Stamp's function of the same name. **It can be used to read resistive sensors of any type. Change the capacitor size to achieve the desired resolution**(分辨率).

The function can also be used to read voltage output type sensors, such as the Sharp infrared distance sensor's with some caveats.

One virtue of RCtime is that it can be very wide ranging, reporting values that would require a 16-18 bit A/D input to read. One downside is that it's not perfectly linear, because charging a capacitor through a resistor does not yield a linear curve.

```Arduino
int sensorPin = 4;              // 220 or 1k resistor connected to this pin
long result = 0;
void setup()                    // run once, when the sketch starts
{

   Serial.begin(9600);

   Serial.println("start");      // a personal quirk 
}
void loop()                     // run over and over again
{

   Serial.println( RCtime(sensorPin) );

   delay(10);

}

long RCtime(int sensPin){         //一种用数字引脚读取电阻式传感器的方法

   long result = 0;

   pinMode(sensPin, OUTPUT);       // make pin OUTPUT

   digitalWrite(sensPin, HIGH);    // make pin HIGH to discharge capacitor - study the schematic

   delay(1);                       // wait a  ms to make sure cap is discharged

   pinMode(sensPin, INPUT);        // turn pin into an input and time till pin goes low

   digitalWrite(sensPin, LOW);     // turn pullups off - or it won't work

   while(digitalRead(sensPin)){    // wait for pin to go low

      result++;

   }

   return result;                   // report results   
}
```

return to [Digital Pins](#digital-pins)