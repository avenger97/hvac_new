#include <avr/io.h>
#include<stdint.h>
#include<avr/interrupt.h>
#include<util/delay.h>
#define SET_BIT(PORT,BIT) PORT|=(1<<BIT)
#define CLR_BIT(PORT,BIT) PORT&=~(1<<BIT)

struct {
volatile unsigned int FLAG_ISR1:1;
volatile unsigned int FLAG_ISR0:1;
}FLAG_BIT;
uint8_t PIN_READ_D;
uint8_t PIN_READ_B;

int main(void)
{
    SET_BIT(DDRD,PD6);//pwm

    CLR_BIT(DDRD,PD2);// blower speed
    SET_BIT(PORTD,PD2);
    CLR_BIT(DDRD,PD3);// evaporator
    SET_BIT(PORTD,PD3);

    CLR_BIT(DDRD,PD0);// full heat mode
    SET_BIT(PORTD,PD0);
    CLR_BIT(DDRD,PD1);// economy mode
    SET_BIT(PORTD,PD1);
    CLR_BIT(DDRB,PB0);// full ac on
    SET_BIT(PORTB,PB0);

    SET_BIT(DDRB,PB3);
    SET_BIT(DDRB,PB2);
    SET_BIT(DDRB,PB1);
    CLR_BIT(PORTB,PB1);



    EICRA|=(1<<ISC11);
    EICRA&=~(1<<ISC10);
    EICRA|=(1<<ISC01);
    EICRA&=~(1<<ISC00);

    EIMSK|=(1<<INT1);
    EIMSK|=(1<<INT0);

    TCCR0A|=((1<<WGM01) | (1<<WGM00));
    TCCR0A|=(1<<COM0A1);
    TCCR0A|=(1<<COM0A0);

    TCNT0=0x00;
    OCR0A=255;
        TCCR0B|=((1<<CS00)|(1<<CS02));
        TCCR0B&=~(1<<CS01);
        sei();

while(1){
            PIN_READ_D=PIND;
            PIN_READ_B=PINB;
            if(PIND & (1<<PD0)){
                //heat mode
                SET_BIT(PORTB,PB1);
            }
            else if(!(PIND & (1<<PD0))){
                //heat mode
                CLR_BIT(PORTB,PB1);
            }
            else if (PIND & (1<<PD1)){
                //economy mode
                SET_BIT(PORTB,PB2);
            }
             else if(!(PIND & (1<<PD1))){
                //heat mode
                CLR_BIT(PORTB,PB2);
            }
            else if(PINB & (1<<PB0)){
                //full ac
                SET_BIT(PORTB,PB3);
            }
             else if(!(PINB & (1<<PB0))){
                //heat mode
                CLR_BIT(PORTB,PB3);
            }







            if(FLAG_BIT.FLAG_ISR0==1){
                OCR0A=OCR0A-13;
                FLAG_BIT.FLAG_ISR0=0;
            }

            else if (FLAG_BIT.FLAG_ISR1==1)
            {

                OCR0A=OCR0A+13;
                FLAG_BIT.FLAG_ISR1=0;
            }

    }
}
    ISR(INT1_vect){
                FLAG_BIT.FLAG_ISR1=1;
    }
    ISR(INT0_vect){
                FLAG_BIT.FLAG_ISR0=1;
    }
