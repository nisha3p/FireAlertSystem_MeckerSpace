/*
 * bmp2.c
 *
 * Created: 15-02-2020 17:07:00
 * Author : Anadi
*/

#define F_CPU_8000000UL
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>
#include <avr/sfr_defs.h>
#include <bmp180.h>
#include "bmp180.c"
#include <i2c.h>




void uart_initial(int BAUD){

	UBRR0H |=(unsigned char)(BAUD>>8);
	UBRR0L |=(unsigned char)(BAUD);
	UCSR0B |=(1<<TXEN0)|(1<<RXEN0);
	UCSR0C |=(1<<UCSZ01)|(1<<UCSZ00);
}
void uart_transmit (float c)
{
	while(!(UCSR0A & (1<<UDRE0)));
	UDR0=c;
}

int main(void)
{    
	DDRB=0b00001110;
	int i=0,a,d;
	char beg[]="AT+CMGF=1";
	char st[]="AT+CMGS=\"9016187448\"";
	char msggas[]="gas sensor has detected smoke !!";
	char msgtemp[]="temperature sensor has detected dangerously high values!!";
	char newmsg[]="Huge suspection of fire bpoth gas and temp sensors have  been detected";
	
	uart_initial(51);
	//int c=11;
	int c;
	int r=40;
	
	while(1)
	{
		
		PORTB=0b00001000;
		//c=929;
		init_sensor(bmp180_mode_1);
		c =(int)calculate_temperature()/10;
		//uart_transmit (c);
		_delay_ms(100);

		//	if(c>=r)

		if(bit_is_clear(PINB,0)&& (c>=r))
		{
			PORTB=0b00000110;
			_delay_ms(1000);
			while(!(UCSR0A & (1<<UDRE0)));
			for(i=0;i<strlen(beg);i++)
			{
				UDR0=beg[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(st);i++)
			{
				UDR0=st[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(newmsg);i++)
			{
				UDR0=newmsg[i];
				_delay_ms(100);
			}	UDR0='\r';
			UDR0=(26);
			_delay_ms(10000);
			PORTB=0b00000000;
			_delay_ms(60000);
		}

		else if(bit_is_clear(PINB,0))
		{
			PORTB=0b00000010;
			_delay_ms(1000);
			while(!(UCSR0A & (1<<UDRE0)));
			for(i=0;i<strlen(beg);i++)
			{
				UDR0=beg[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(st);i++)
			{
				UDR0=st[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(msggas);i++)
			{
				UDR0=msggas[i];
				_delay_ms(100);
			}	UDR0='\r';
			UDR0=(26);
			_delay_ms(10000);
			PORTB=0b00000000;
			_delay_ms(60000);
		}
		
		
		else if(c>=r){
			PORTB=0b00000100;
			_delay_ms(1000);
			while(!(UCSR0A & (1<<UDRE0)));
			for(i=0;i<strlen(beg);i++)
			{
				UDR0=beg[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(st);i++)
			{
				UDR0=st[i];
				_delay_ms(100);
			}	UDR0='\r';
			_delay_ms(10000);
			for(i=0;i<strlen(msgtemp);i++)
			{
				UDR0=msgtemp[i];
				_delay_ms(100);
			}
			/*a=c/10;
			a=a+48;
			d=c%10+48;
			UDR0=a;
			_delay_ms(100);
			UDR0=d;*/
			_delay_ms(100);
			UDR0='\r';
			UDR0=(26);
			_delay_ms(10000);
			
			PORTB=0b00000000;
			_delay_ms(60000);
		}

		
		
		_delay_ms(100);
		
		
	}

	return 0;
}