# DEPRECATED PROJECT
최신 버전의 안드로이드와 호환되지 않습니다.
그리고 더이상 업데이트를 제공하지 않습니다.

# Android-Bluetooth-Test-Module
Android -- ATmaga128 or Arduino Bluetooth Communication Module

[![API](https://img.shields.io/badge/API-9%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=9)


# Example
[GooglePlay](https://play.google.com/store/apps/details?id=com.ybproject.sig_test_BTmodule) 

![](https://github.com/yongbeam/Android-Bluetooth-Test-Module/blob/master/Cap%202016-03-16%2020-26-25-521.jpg?raw=true)
![](https://github.com/yongbeam/Android-Bluetooth-Test-Module/blob/master/Cap%202016-03-16%2020-26-27-530.jpg?raw=true)


# Usage

### Check BluetoothChatService.java , MY_UUID
```JAVA
// Unique UUID for this application
private static final UUID MY_UUID = UUID.fromString("00001101-0000-1000-8000-00805F9B34FB");
```


### ATmaga128,  LED control using Bluetooth Demo
```C
#include <mega128.h>
#include <delay.h>
#define F_CPU 16000000L
#define BAUD_DIV (F_CPU/8/BAUD - 1)
#define BAUD_DIV_H BAUD_DIV >> 8
#define BAUD_DIV_L BAUD_DIV
#define BAUD 9600 // 9600 for fb155bc, 38400 for HC-05, 9600 for HC-06(default)

#define TX_CH(ch, val) do { while(!(UCSR##ch##A & 0x20)); UDR##ch=val; } while(0)
#define RX_CH(ch, val) do { while(!(UCSR##ch##A & 0x80)); val = UDR##ch; } while(0)
#define AVAIL_RX(ch) (UCSR##ch##A & 0x80)

char Usart0_Rx(void); // RX 수신완료 함수

unsigned char PutChar0 (unsigned char c)
{
 while (!(UCSR0A & 0x20)) ;
 UDR0 = c;
 return 0;
}
void atcommand (flash unsigned char *s)
{
 while (*s != '') {
  PutChar0(*s);
  s++;
 }
}

void main(void)
{
    char ch;
    char a;
    DDRB = 0xff; 
    UCSR0A = 0x00; // 초기 설정이 현재 할려는 예제의 설정에 필요 없음
    UCSR0B = 0x18; // 수신허용 RXEN0=1, 송신허용TXEN0=1(미사용이나설정) 
    UCSR0C = 0x06; //전송 문자의 데이터 비트수 8비트
    
    UBRR0H = 0; // 9600으로 설정하기 때문에 상위 값에는 값이 필요없음
    UBRR0L = 103;  // BAUD 9600값은  UBRR0 = 103
    DDRB=0XFF;
    PORTB=0X00;
    UCSR0A = 2; UCSR0B=0x18; UBRR0H=BAUD_DIV_H ; UBRR0L=BAUD_DIV_L;
    UCSR1A = 2; UCSR1B=0x18; UBRR1H=BAUD_DIV_H ; UBRR1L=BAUD_DIV_L;

    atcommand("AT+BTSCAN\r"); 
    
        while(1)
        {
          a=Usart0_Rx();
          
          if(a=='1') //수신된 값이 a이면 LED를 OFF
            {
              PORTG = 0x00; // LED off  
            }
          if(a=='2')//수신된 값이 b이면 LED를 ON
            {
              PORTG = 0xff; // LED on
            } 
            
          if(a=='2')//수신된 값이 b이면 LED를 ON
            {
              PORTG = 0xff; // LED on
            }
               
          if(a=='4')//수신된 값이 b이면 LED를 ON
            {
              PORTG=0x00;   
              delay_ms(500); 
              PORTG=0x01;   
              delay_ms(500);
              PORTG=0x02;   
              delay_ms(500);
              PORTG=0x03;   
              delay_ms(500); 
              PORTG=0x04;   
              delay_ms(500);
              PORTG=0x05;   
              delay_ms(500);  
              PORTG=0x06;   
              delay_ms(500); 
              PORTG=0x07;   
              delay_ms(500);
              PORTG=0x08;   
              delay_ms(500);
              PORTG=0x09;   
              delay_ms(500);
            }
     }                                             

}

// UCSR0A 레지스터 데이터가 있으면(또는 입력되면) 자동으로 RXC0 = 1 이면
// 수신완료 int 요청 cpu는 UDR0 값 읽고 cpu가 값을 읽으면 자동 clear 됨  
char Usart0_Rx(void)
{
  while(!(UCSR0A & 0x80));
  return UDR0; 
} 
```


# License

    Copyright 2016 LeeYongBeom

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
