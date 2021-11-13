# Embedded_Lab_5
Code for parts 2 &amp; 3 of lab 5


//Part 2

  #include "msp430G2553.h"

  void main(void)

  {

  WDTCTL = WDTPW + WDTHOLD; // Stop WDT

  P1DIR |= BIT6; // P1.6 to output

  TA0CTL = TASSEL_2 + MC_1+ID_3;//+TACLR;

   TA0CCR0 = 32768; // Set maximum count value (PWM Period

  TA0CCR1 = 3277; // initialize counter compare value

  TA0CCTL0 |= CCIE;

  TA0CCTL1 |= CCIE;

   TA0CCTL0 &=~CCIFG;

  TA0CCTL1 &=~CCIFG;

  _enable_interrupts(); // Enter LPM0

  }

  #pragma vector = TIMER0_A0_VECTOR //define the interrupt service vector

  __interrupt void TA0_ISR (void) // interrupt service routine

  {

  P1OUT |=BIT6;

  TA0CCTL0 &=~CCIFG;

  }

  #pragma vector = TIMER0_A1_VECTOR


  __interrupt void TA1_ISR (void) {

  P1OUT &=~BIT6;

  TA0CCTL1 &=~CCIFG;

  }

//Part 3

  #include "msp430G2553.h"

  #define BUTTON1 BIT1

  void main(void)

  {

  WDTCTL = WDTPW + WDTHOLD; // Stop WDT

  P1DIR |= BIT6; // P1.6 to output

  P1SEL |= BIT6; // P1.6 to TA0.1

  P1SEL2 &= ~BIT6; // P1.6 to TA0.1

  P1REN |= BUTTON1; // Enables resistor

  P1OUT |= BUTTON1; // Pull-up Resistor

  P1IE |= BUTTON1; // Enables interrupt

  P1IES |= ~BUTTON1; // Falling edge select

  P1IFG &= ~BUTTON1; // Clears any flags

  TA0CTL = TASSEL_2 + MC_1+ID_3;//+TACLR;

  TA0CCR0 = 32768; // Set maximum count value (PWM Period

  TA0CCR1 = 6554; // initialise counter compare value

   TA0CCTL1 = OUTMOD_7; // PWM set to reset

  _enable_interrupts(); // Enter LPM0

  }
 
  #pragma vector=PORT1_VECTOR // Button interrupt

  __interrupt void Port_1(void) {

  P1OUT ^=BIT6;

  P1IFG &= ~BUTTON1;

  }
