### Martin Merida - 23002494
# Lab 3

## funciones (modificadas/agregadas):
-timer_init(void): configura el DMTIMER2 para generar una interrupción por overflow cada ~2 segundos y habilita su IRQ en el controlador de interrupciones.

-enable_irq(void) (root.s): habilita las interrupciones IRQ en el CPU limpiando el bit I del CPSR.

-irq_handler(void) (root.s): handler de excepción IRQ; guarda registros, llama a timer_irq_handler() en C, restaura registros y retorna de la interrupción.

-timer_irq_handler(void): rutina de servicio del timer; limpia el flag de overflow, reconoce la interrupción en el INTC y escribe "Tick\n" por UART.

-main(void): inicializa el sistema (mensajes, timer_init(), enable_irq()) y luego imprime números aleatorios continuamente con un pequeño delay.

## Explicación de arquitectura:
-Hardware: el DMTIMER2 desborda y genera la IRQ 68.

-Controlador de interrupciones: la IRQ 68 se desenmascara y se enruta con prioridad.

-CPU ARM: si enable_irq() limpió el bit I del CPSR, el CPU acepta la IRQ y salta al vector 0x18.

-Vector/handler: el vector salta a irq_handler, que salva contexto y llama a timer_irq_handler() en C.

-ISR en C: timer_irq_handler() limpia el overflow y hace acknowledge al INTC, imprime "Tick\n", y vuelve; irq_handler retorna con subs pc, lr, #4.

Para correr el código, desde la carpeta que contiene el archivo de build_and_run.sh, correr el siguiente comando:

```bash
./build_and_run.sh
