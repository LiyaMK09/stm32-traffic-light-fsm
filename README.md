# STM32 Traffic Light Controller

A basic traffic light controller implemented using an STM32 microcontroller. The project demonstrates GPIO control, finite state machines, non-blocking timing, external interrupts, and button debouncing.

## Features

- Three LED traffic light system
- Finite State Machine (FSM) implementation
- Non-blocking timing using HAL_GetTick()
- Push button input using EXTI interrupt
- Software button debouncing
- STM32 HAL-based implementation

## Hardware Setup

<img width="504" height="627" alt="WhatsApp Image 2026-09-05 at 6 24 15 PM" src="https://github.com/user-attachments/assets/8cfb1c01-50b7-49c1-a5d6-7596a10232fe" />


## Hardware Components

- STM32 NUCLEO-F401RE
- LED x 3
- Push button
- 220Ω resistors x 3
- Breadboard
- Jumper wires

## Pin Configuration

|  Component  | STM32 Pin |
|-------------|-----------|
|     LED     |    PA5    |
|     LED     |    PA6    |
|     LED     |    PA7    |
| Push Button |    PA0    |

## Circuit

The LEDs are connected to GPIO output pins using current-limiting resistors.

The push button is connected between PA0 and GND.
PA0 uses the internal pull-up resistor.

## FSM

The traffic light is implemented as a finite state machine.

RED → YELLOW → GREEN → YELLOW → RED

The YELLOW state uses the previous state to determine whether the next state should be GREEN or RED.

## Timing

| State | Duration |
|-------|----------|
| RED   | 5 seconds|
| YELLOW| 2 seconds|
| GREEN | 5 seconds|

Timing is implemented using 'HAL_GetTick()' instead of
'HAL_Delay()', allowing the main loop to continue running
while the timer is being checked.

## Button Interrupt

The push button is configured using an EXTI falling-edge
interrupt.

When the button is pressed, the interrupt callback sets
'buttonPressed' to 1.

The main FSM then processes this event.

## Debouncing

Mechanical push buttons can generate multiple transitions
during a single press.

A 50 ms software debounce period is therefore implemented
using 'HAL_GetTick()'.

## Program Flow

              RED
               │
             5 sec
               ↓
            YELLOW
               │
             2 sec
               ↓
             GREEN
               │
        ┌──────┴──────┐
        │             │
      5 sec       Button press
        │             │
        └──────┬──────┘
               ↓
            YELLOW
               │
             2 sec
               ↓
              RED
