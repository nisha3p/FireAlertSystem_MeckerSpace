/*
 * GccLibrary1.c
 *
 * Created: 02-02-2020 10.11.15 AM
 * Author : yatin
 */ 

#include <avr/io.h>
#include <util/delay.h>
#include <stdlib.h>
#include <string.h>
#define F_CPU_8000000UL

void uart_initial(int BAUDRATE)
{
	UBRR0H |=(unsigned char)(BAUDRATE>>8);
	UBRR0L |=(unsigned char)(BAUDRATE);
	UCSR0B |=(1<<TXEN0)|(1<<RXEN0);
	UCSR0C |=(1<<UCSZ01)|(1<<UCSZ00);
}
/*8void uart_transmit (unsigned char str)
{
	while(!(UCSR0A & (1<<UDRE0)));
	UDR0=str;
}
unsigned char uart_receiver(void)
{
	while(!(UCSR0A & (1<<RXC0)));
	return UDR0;
}*/
int main(void)
{
	int i=0;
	char st[]="ATD9558323468;";
	uart_initial(51);
	
	
	while(1)
	{
		_delay_ms(10000);
		while(!(UCSR0A & (1<<UDRE0)));
		for(i=0;i<14;i++)
		{
			UDR0=st[i];
			_delay_ms(10);
	    }
		//UDR0=st[6];
	//_delay_ms(10000);
	UDR0='\r';
	//_delay_ms(10000);
	break;
}
/* Replace with your library code */

}


