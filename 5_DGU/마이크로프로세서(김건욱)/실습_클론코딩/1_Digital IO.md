
LED 켜기

```c
#include <msp430.h> 

/**
 * main.c
 */
int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;	// stop watchdog timer
	
	P1DIR = 0x0001; // LED dir set

	while(1)
	{
	    P1OUT = 0x0001; // LED ON(1=5v)
	}

	return 0;
}

```

LED On/Off 반복

```c
#include <msp430.h> 

/**
 * main.c
 */
int main(void)
{
	WDTCTL = WDTPW | WDTHOLD;	// stop watchdog timer
	
	P1DIR |= BIT0; // LED dir set (other bit preserving)

	while(1)
	{
	    P1OUT = 0x0001; // LED ON(1=5v)
	    __delay_cycles(500000);
	    
	    P1OUT &= ~BIT0;
	    __delay_cycles(500000);
	}

	return 0;
}

```

LED On/Off 반복 by xor

```c
#include <msp430.h> 

/**
 * main.c
 */
int main(void)
{
    WDTCTL = WDTPW | WDTHOLD;   // stop watchdog timer

    P1DIR |= BIT0; // LED dir set (other bit preserving)

    while(1)
    {

        P1OUT ^= BIT0; // LED toggle
        __delay_cycles(500000);

    }
}

```

>MSP430 하드웨어 LED 에 실제로 오른쪽 LED 에는 P1.0 , 왼쪽 LED 에는 P4.7 이라고 써있음.

스위치 누르면 LED On

```c
#include <msp430.h> 

/**
 * main.c
 */
int main(void)
{
    WDTCTL = WDTPW | WDTHOLD;   // stop watchdog timer

    P1DIR |= BIT0; // left LED dir set
    P4DIR |= BIT7; // right LED dir set 
 
    P1OUT &= ~BIT0; // left LED off(0=0v)
    P4OUT &= ~BIT7; // right LED off(0=0v)

    P2OUT |= BIT1; // left switch pullup set ;
    P2REN |= BIT1; // p2 pullup enable

    P1OUT |= BIT1; // right switch pullup set;
    P1REN |= BIT1; // p1 pullup enable

    while(1)
    {
            if((P2IN & BIT1) == 0) // switch close(0)
            {
                P1OUT |= BIT0; // left LED on(1=5v) other pin preserve
            }
            else if((P1IN & BIT1) == 0)
            {
                P4OUT |= BIT7; // right LED on(1=5v) other pin preserve
            }
            else
            {
                P1OUT &= ~BIT0; // left LED off(0=0v) other pin preserve
                P4OUT &= ~BIT7; // right LED off(0=0v) other pin preserve
            }

    }
}

```

채터링 체험

```c
#include <msp430.h> 

/**
 * main.c
 */
int main(void)
{
    WDTCTL = WDTPW | WDTHOLD;   // stop watchdog timer

    P1DIR |= BIT0; // left LED dir
    P4DIR |= BIT7; // right LED dir

    P1OUT &= ~BIT0; // left LED off
    P4OUT &= ~BIT7; // right LED off

    P2OUT |= BIT1; // left switch;
    P2REN |= BIT1;

    P1OUT |= BIT1; // right switch;
    P1REN |= BIT1;

    while(1)
    {
            if((P2IN & BIT1) == 0){
                P1OUT ^= BIT0; // left LED toggle
            }
            else if((P1IN & BIT1) == 0)
            {
                P4OUT ^= BIT7; // right LED toggle
            }

    }
}

```

체터링 방지 소프트웨어 처리(while)