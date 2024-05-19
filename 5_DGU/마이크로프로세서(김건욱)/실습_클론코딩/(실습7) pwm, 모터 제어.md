MSP 에 ADC 는 있고 DAC 는 없음
(ADC : 아날로그 -> 디지털, DAC : 디지털 -> 아날로그)

#### PWM (Pulse Width Modulation)
펄스 폭 변조.
msp 의 출력인 5v 를 쪼개서 전압을 낮출 수 있음
- 타이머 인터럽트로 구현
	- TAxR, TAxCCR1 으로 OUTPUT toggle
#### PxSEL
0 : 핀을 io 로 사용
1 : 특수기능(pwm) 으로 사용
- 특정 핀만 지원함

#### TA2.2, TA2.1 로 모터 제어
TA2.2 와 TA2.1 의 차이 만큼 모터가 회전
P2.5 핀 : TA2.2 에 연결
P2.4 핀 : TA2.1 에 연결

#### 실습1 : 모터 움직이기
```c
#include <msp430.h>

void main(void)
{
    WDTCTL = WDTPW | WDTHOLD;
    P2DIR |= (BIT5 | BIT4);
    P2SEL |= (BIT5 | BIT4);

    TA2CTL = TASSEL_2 + MC_1; // SMCLK l 1MHz / Up mode to CCR0
    TA2CCR0 = 1000; // Up to 1000
    // each 1ms, overflow

    TA2CCTL2 = OUTMOD_6; // PWN set
    TA2CCR2 = 0;
    TA2CCTL1 = OUTMOD_6; // PWN set
    TA2CCR1 = 0;

    while(1)
    {
        TA2CCR2 = 800; // 0~1000, motor speed mod by pwn mod
        TA2CCR1 = 0;
    }
}
// 작동테스트 완료
```