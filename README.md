# UART Shell with Timer Interrupts
This project adds onto my previous project, the UART Command Shell with I2C Sensor Communication, by exploring timers for interrupts and PWM. New commands were added for the user to change an LED's duty cycle and pulse the LED. Additionally, the previous project's "stream" command was updated to user timer for non-blocking data display. Bare-metal programming was utilized to emphasize register configuration, flag update enables, interrupt enables, duty-cycle and period manipulation, and interrupt request handlers. The code from the previous project was refactored for better readability and more clear function intentions. This project uses the UART communication protocol to send commands from my latptop to my STM32 NUCLEO-F411RE. This project builds on the embedded systems foundations established in the previous implementation.

# What was done
1. PWM Timer Initialization\
   A PWM signal was generated using a timer peripheral. This allowed for an LED's brightness to be changed manually in the code. To do this, a PWM timer was configured using GPIO, alternate function mode, PWM Mode 1, calculating prescaler (PSC) and auto-reload register (ARR) values, and setting/clearing necessary register flags.
2. LED Brightness UART Command\
   A command was added to the UART Shell, "dim XX", to allow the user to set the brightness percentage of the LED, with 0 being off and 100 being maximum brightness. The compare register CCR1 was used to change the duty cycle of the LED.
3. Stream Command Fix\
   The "stream" command from the last project was updated to use interrupts for non-blocking streaming. GPIO was not needed for this implementation. The timer was configured such that PSC and ARR created a period of two seconds, so the MPU-6500 sensor data was updated to the shell every two seconds. An IRQHandler function was also made to update the state of the data stream.
4. LED Pulse Command\
   A "pulse" command was added to smoothly pulse the LED until a "stop_pulse" command is entered. This used the same concepts from the "stream" command fix, while also creating a FSM for the led's state: 0 is off, 1 is increase brightness, and 2 is decrease brightness. This pulsing goes until prompted to stopped, and can be used concurrently with the "stream" command.
5. Code Refactor\
   The project's code was refactored to be more organized, easier to follow, and the code's intent is more clear. This was done by moving the "process_command" function into its own C file, and cleaning up the main loop.

# What was learned overall
This project reinforced and introduced topics related to embedded systems including GPIO enables, alternate function modes, timer interrupts, register configuration, flag update enables, interrupt enables, duty-cycle and PWM, concurrency, and bare-metal programming. The next project will implement the SPI communication protocol with a MFRC522 sensor, meaning the project would use UART, I2C, and SPI protocols. Additionally, this project still has great potential for future additions such as communication with an LCD, motion sensors, etc.
