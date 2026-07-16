The Devboard is divided into the following blocks:  
1. **Block 1 — USB Power Input & Protection**  
2. **Block 2 — 3.3 V Power Supply**  
3. **Block 3 — STM32 Minimum System**  
4. **Block 4 — SWD Programming Interface**  
5. **Block 5 — User Interface**  
6. **Block 6 — GPIO Expansion**  
   ## lets understand why each component was placed and what is it's function block wise
Block 1 — USB Power Input & Protection: Provide safe 5 V input from a USB-C connector and protect the board against faults before supplying the voltage regulator
   
       
   | Ref | Component                 | Why it was used                                               | Function                                                      |
| --- | ------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| J1  | USB-C Receptacle (16-pin) | Standard USB connector for power and future USB communication | Accepts 5 V power from a USB cable                            |
| F1  | MF-R110 Resettable Fuse   | Prevents excessive current from damaging the board            | Disconnects power during overcurrent and automatically resets |
| D1  | ESD9B5.0ST5G TVS Diode    | USB connectors are exposed to electrostatic discharge         | Protects the power input from ESD and voltage spikes          |
| C1  | 10 µF Capacitor           | Bulk input capacitor                                          | Smooths incoming 5 V supply and handles sudden current demand |
| C2  | 100 nF Capacitor          | High-frequency bypass capacitor                               | Filters high-frequency noise on the USB supply                |  

Block 2 — 3.3 V Power Supply: Convert USB 5 V into a stable 3.3 V supply for the STM32 and peripherals  
   
   | Ref | Component        | Why it was used                                                    | Function                                                      |
| --- | ---------------- | ------------------------------------------------------------------ | ------------------------------------------------------------- |
| U1  | AP2112K-3.3 LDO  | Low-noise regulator capable of supplying the STM32 and peripherals | Converts 5 V to regulated 3.3 V                               |
| C3  | 10 µF Capacitor  | Output stability                                                   | Prevents regulator oscillation and supplies transient current |
| C4  | 100 nF Capacitor | High-frequency filtering                                           | Removes switching noise from the regulator output             |

3. Block 3 — STM32 Minimum System: Provide all circuitry required for reliable STM32 operation.  
   
| Ref | Component     | Why it was used         | Function                                       |
| --- | ------------- | ----------------------- | ---------------------------------------------- |
| U2  | STM32F103C8Tx | Main microcontroller    | Executes firmware and controls all peripherals |
| C5  | 100 nF        | Local decoupling        | Filters VDD (Pin 24)                           |
| C6  | 100 nF        | Local decoupling        | Filters VDD (Pin 36)                           |
| C7  | 100 nF        | Local decoupling        | Filters VDD (Pin 48)                           |
| C8  | 100 nF        | Analog supply bypass    | Stabilizes VDDA for ADC and analog peripherals |
| Y1  | 8 MHz Crystal | Accurate external clock | Provides precise system clock reference        |
| C9  | 22 pF         | Crystal load capacitor  | Ensures stable crystal oscillation             |
| C10 | 22 pF         | Crystal load capacitor  | Ensures stable crystal oscillation             |
| R4  | 10 kΩ         | NRST pull-up            | Keeps MCU out of reset during normal operation |
| SW2 | Push Button   | Manual reset            | Pulls NRST low to reset the MCU                |
| R5  | 10 kΩ         | BOOT0 pull-down         | Keeps BOOT0 low for normal Flash boot          |
| SW1 | Push Button   | Bootloader entry        | Pulls BOOT0 high when pressed during reset     |

4. Block 4 — SWD Programming Interface: Allow firmware upload and debugging using an ST-Link programmer  
   
 **Power LED**
| Ref | Component      | Why it was used                           | Function                             |
| --- | -------------- | ----------------------------------------- | ------------------------------------ |
| J2  | 2×5 SWD Header | Standard ARM Cortex programming interface | Connects ST-Link debugger to the MCU |

5. Block 5 — User Interface: Provide simple visual status indication
   
| Ref | Component | Why it was used            | Function                         |
| --- | --------- | -------------------------- | -------------------------------- |
| D2  | Green LED | Indicates board is powered | Lights whenever 3.3 V is present |
| R1  | 1 kΩ      | LED current limiting       | Protects LED and regulator       |

**User LED**
| Ref | Component | Why it was used               | Function                       |
| --- | --------- | ----------------------------- | ------------------------------ |
| D3  | User LED  | Software-controlled indicator | Used for testing and debugging |
| R6  | 330 Ω     | LED current limiting          | Limits current through the LED |  

**UART header**
| Ref | Component | Why it was used               | Function                       |
| --- | --------- | ----------------------------- | ------------------------------ |
| D3  | User LED  | Software-controlled indicator | Used for testing and debugging |
| R6  | 330 Ω     | LED current limiting          | Limits current through the LED |

6. Block 6 — GPIO Expansion: Expose nearly every GPIO pin of the STM32 for external circuits  
   
**Left header**
   | Ref | Component          | Function                                                          |
| --- | ------------------ | ----------------------------------------------------------------- |
| J4  | 1×20 Female Header | Provides access to GPIOs including NRST, BOOT0, PA0–PA7, PB0–PB15 |

**right header**
| Ref | Component          | Function                                                                                  |
| --- | ------------------ | ----------------------------------------------------------------------------------------- |
| J5  | 1×20 Female Header | Provides access to GPIOs including PC13–PC15, PA8–PA15, PB3–PB9, SWD signals, power rails |


# Overall system architecture 
USB-C Input  
     │  
     Resettable Fuse  
     │  
     TVS Protection  
     │  
     Input Capacitors  
     │  
     AP2112K 3.3V Regulator  
     │  
     STM32F103C8T6  
     ├────────────── SWD Header  
     ├────────────── UART Header    
     ├────────────── Power LED  
     ├────────────── Crystal Oscillator  
     ├────────────── Reset Circuit  
     ├────────────── Boot Circuit  
     └────────────── GPIO Headers  
       


