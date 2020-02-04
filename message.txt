#include <avr/io.h>
#include <util/delay.h>
#include <stdlib.h>
#include <string.h>
#include <avr/sfr_defs.h>
#define F_CPU_8000000UL
void uart_initial(int BAUDRATE)
{
	UBRR0H |=(unsigned char)(BAUDRATE>>8);
	UBRR0L |=(unsigned char)(BAUDRATE);
	UCSR0B |=(1<<TXEN0)|(1<<RXEN0);
	UCSR0C |=(1<<UCSZ01)|(1<<UCSZ00);
}
int main(void)
{
	int i=0;
	char beg[]="AT+CMGF=1";
	char st[]="AT+CMGS=\"9016187448\"";
	char msg[]="Hello";
	uart_initial(51);
	DDRD=0x00;
	while(1)
	{
		
		if (bit_is_set(PIND,0))
		{
			
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
			for(i=0;i<strlen(msg);i++)
			{
				UDR0=msg[i];
				_delay_ms(100);
			}	UDR0='\r';
			UDR0=(26);
			_delay_ms(10000);
			break;
		
		}
		
		
   }

}