# API
My first repository on GitHub

#include <p32mx360f512l.h>

#pragma config POSCMOD=XT, FNOSC=PRIPLL 
#pragma config FPLLIDIV=DIV_2, FPLLMUL=MUL_18, FPLLODIV=DIV_1
#pragma config FPBDIV=DIV_8, FWDTEN=OFF, CP=OFF, BWP=OFF
#define CSEE    _RD12       
#define TCSEE   _TRISD12    
#define SPI_CONF    0x8120  
#define SPI_BAUD    15      
#define SEE_WRSR    1       	
#define SEE_WRITE   2       
#define SEE_READ    3       
#define SEE_WDI     4       
#define SEE_STAT    5       
#define SEE_WEN     6       

int writeSPI2(int i)
{
    SPI2BUF = i;
    while(!SPI2STATbits.SPIRBF);
    return SPI2BUF;
}

main()
{
    int i;

    TCSEE = 0;
    CSEE = 1;
    SPI2CON = SPI_CONF;
    SPI2BRG = SPI_BAUD;

    while(1)
    {
        CSEE = 0;
        writeSPI2(SEE_WEN);
        CSEE = 1;
        CSEE = 0;
        writeSPI2(SEE_STAT);
        i = writeSPI2(0);
        CSEE = 1;
    }
}
