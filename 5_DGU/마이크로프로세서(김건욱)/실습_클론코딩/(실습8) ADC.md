
#### msp 에 탑재된 ADC
- 12bit 로 디지털 값 표현
	- 0V 아날로그 입력 -> 0 디지털값
	- 3.3V 아날로그 입력 -> 0xFFF 디지털값
		- 이것을 기준으로 0V~3.3V 값 계산 가능
			- ex) 2V -> (2/3.3) * 0xFFF(4095) = 0x7FFF(2481)
#### ADC 모드
- 싱글채널, 순차채널, 반복싱글채널, 반복순차채널 4가지 모드가 있음
- 실습에는 싱글채널, 반복싱글채널만 사용
#### PxSEL
ADC 의 역할을 하는 핀을 1로 두면 특수기능(ADC)를 사용
(0 이면 io로 사용)

```c
#include <msp430.h>

unsigned int data = 0;

void main(void)
{
    WDTCTL = WDTPW | WDTHOLD;

    P1DIR |= BIT0;
    P4DIR |= BIT7;

    P1OUT &= ~BIT0;
    P4OUT &= ~BIT7;

    P6SEL |= BIT0;
    ADC12CTL0 = ADC12SHT02 + ADC12ON;
    ADC12CTL1 = ADC12SHP;
    ADC12MCTL0 = ADC12INCH_0;
    ADC12CTL0 |= ADC12ENC;

    while(1)
    {
        ADC12CTL0 |= ADC12SC;
        while(!(ADC12IFG & BIT0));
        data = ADC12MEM0; // ADC12MEM0 에 가변저항값이 저장되어있음
        if(data > 3000)
        {
            P1OUT |= BIT0;
            P4OUT |= BIT7;
        }
        else if(data > 2000)
        {
            P1OUT &= ~BIT0;
            P4OUT |= BIT7;
        }
        else
        {
            P1OUT &= ~BIT0;
            P4OUT &= ~BIT7;
        }
    }

}

```

#### ADC 반복 싱글 채널

```c
#include <msp430.h>

unsigned int data = 0;

void main(void)
{
    WDTCTL = WDTPW | WDTHOLD;
	/* Board LED set */
    P1DIR |= BIT0;
    P4DIR |= BIT7;
	P1OUT &= ~BIT0;
    P4OUT &= ~BIT7;

	/* Motor set */
	P2DIR |= (BIT0 | BIT2);
	P2OUT &= ~(BIT0 | BIT2);

	/* PWM set */
    P6SEL |= BIT0;
    ADC12CTL0 = ADC12SHT02 + ADC12ON;
    ADC12CTL1 = ADC12SHP;
    ADC12MCTL0 = ADC12INCH_0;
    ADC12CTL0 |= ADC12ENC;

    while(1)
    {
        ADC12CTL0 |= ADC12SC;
        while(!(ADC12IFG & BIT0));
        data = ADC12MEM0; // ADC12MEM0 에 가변저항값이 저장되어있음
        if(data > 3000)
        {
            P1OUT |= BIT0;
            P4OUT |= BIT7;
        }
        else if(data > 2000)
        {
            P1OUT &= ~BIT0;
            P4OUT |= BIT7;
        }
        else
        {
            P1OUT &= ~BIT0;
            P4OUT &= ~BIT7;
        }
    }

}

```