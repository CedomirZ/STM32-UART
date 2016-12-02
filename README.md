# STM32-UART
FreeRTOS

Pomoću paketa STM32 CubeMX izvršeno je konfigurisanje potrebnih periferija (UART1), izabrani GPIO pinovi koji se setuju, odnosno resetuju
(pinovi B0 i B1), omogućeno je korišćenje freeRTOS-a i izabran je mikrokontroler iz familije STM32F100xB koji se nalazi na STM32VLDISCOVERY
razvojnoj platformi. Na osnovu prethodno navedenih konfiguracija generisan je C kod u pomenutom paketu koji je otvoren pomoću IAR for ARM
kompajlera i koji koristi HAL (Hardware Abstraction Layer) drajver. C kod, koji se nalazi u main.c fajlu unutar Src direktorijuma,
opremljen je gotovim funkcijama za kreiranje taska (StartDefaultTask) kao i inicijalizaciju HAL, GPIO, sistemskog takta i UART-a. C kod za
task "StartDefaultTask" je korisnički unet kod čiji je zadatak da sa UART-a primi traženi paket (0xAA, 0x55, byte, byte, 0xEE), gde treći
bajt predstavlja broj GPIO pina na koji se postavlja vrednost LSB bita četvrtog bajta, pri čemu se navedena poruka ignoriše brisanjem
prijemnog bafera "Rx_Buffer" ukoliko GPIO ne postoji. Takođe, ukoliko ne dođe do prijema petog bajta 0xEE opet se briše prijemni bafer,
čime se komanda ne izvršava. Prijem paketa se obavlja pomoću ugrađene funkcije HAL_UART_Receive_IT, dok se setovanje/resetovanje GPIO pina
obavlja pomoću funkcije HAL_GPIO_WritePin.
