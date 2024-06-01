# Zadaca9-8085

По исчитување на податок со вредност 09h од изолирана
порта со адреса 0Bh почнувајќи од адреса POCET, сериски со
брзина од 1200bps се пренесуваат 255 податока. По
пренесувањето на податоци, на излезна изолирана порта на
адреса 0Ah се пренесува податок 0Ch. Да се напише
асемблерска програма базирана µP 8085. Фреквенцијата на
осцилаторот е 4MHz. 

**Resenie:**

Ќе го искористиме тоа дека µP знае битот 7 да го пренесе при SIM
инструкција на SOD пинот, само доколку битот 6 е 1.
Ts=0,5 µS
1/1200=833µS X*7=833 X=119

```
CEKAJ: IN 0Bh ;Се вчитува податокот од порта 0Bh

 CPI 09h ;Се споредува со 09h

 JNZ CEKAJ ;Се чека да дојде податок 09h

 LXI H,POCET ;Се полни HL парот со адресата на полето

 MVI D,255 ;Бројач на податоци (не повеќе од 255)

 PАK: MOV A,M ;Првиот податок од полето се запишува во акумулатор

 CALL PRATI ;се повикува процедура за праќање

 INX H ;наредната адреса во полето

 DCR D ;еден податок е пратен

 JNZ PAK ;ако не се испратени сите врати се отпочеток

 MVI A,0Ch ;откако ќе се испратат стави вредност OCh во акумулаторот

 OUT 0Ah ;таа вредност се испраќа на адреса 0Аh

 END ;Крај на програмата

DOCNI: MVI Е, 119d ;процедура за доцнење 833µs

LOOP: DCR Е ; 4 такта

 JNZ LOOP ;10 такта

 RET ; 14 такта * 0,5 µs = 7 µs * 119 = 833 µs

PRATI: MVI C,8h ; Треба да се праќа бит по бит

CIKL: MOV B,A ;Привремена копија и во B регистарот

 ORI 01000000 ;Го поставуваме битот 6 на 1, останатите не ги менуваме

 SIM ;се испраќа битот 7

 CALL DOCNI ;се доцни 833 µs

 MOV A,B ;се враќа податокот во ACC

 RLC ;сега се праќа битот 6

 DCR C ;еден бит помалку за праќање

JNZ CIKL ;Дали сме ги испратиле сите

 RET 
```


 ![Screenshot (1)](https://github.com/slavko444/8085-Zadaca-9/blob/main/code%209.png)
 ![Screenshot (1)](https://github.com/slavko444/8085-Zadaca-9/blob/main/code%209.1.png)
 
**Develop by:**

[Tamara Ristova ](https://github.com/Ristova123)


**Subject**

Microcomputer's systems

**Built With**

This project is built using the following tools:

- [8085 simulator](https://github.com/8085simulator/8085simulator.github.io?tab=readme-ov-file): Assembler and emulator for the Intel 8085 microprocessor.

**Getting Started**

To get a local copy up and running, follow these steps.

**Prerequisites**

In order to run this project you need:

A working computer
Connection to internet
Setup

**How to Run**

To run the program, you need an 8085 emulator or assembler. You can use emulators like DOSBox or TASM (Turbo Assembler). Here's how to run the program using 8085 simulator:

1. Download and install 8085 simulator from [here](https://github.com/8085simulator/8085simulator.github.io?tab=readme-ov-file).
2. Clone this repository to your local machine.
3. Open 8085 simulator and load the `Zadaca9 code.asm` file.
4. Assemble the code by pressing the Assemble button.
5. Run the program by pressing the Run button or by pressing F10.
