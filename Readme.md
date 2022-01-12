# SemiHosting 
> Semihosting is a debugging tool that is built in to most ARM-based microcontrollers. It allows you to use input and output functions on a host computer that get forwarded to your microcontroller over a hardware debugging tool (e.g. ST-LINK).
## **There are other ways to debug as:**
* Serial (UART): fast, but you need extra pins and hardware (such as a USB to Serial converter board)
* Virtual COM Port (VCP): fast, but you need USB hardware (see my previous post for setting this up)
* Instrumentation Trace Macrocell (ITM): fast output over dedicated SWO pin, but it’s only available on Cortex-M3+ parts (i.e. Cortex-M0 chips don’t have it)
> We had used STM32F103C8T6 to prove if this method will be the best match for us or not to use it in VITAL project and through this document we will see the detailed steps to apply this method.


## **Steps**
### **1.Hardware Setup**
#### 1.1 GPIO13 OUTPUT
![img](/imgs/gpio13.PNG)
#### 1.2 SYS CONFIG
![img](/imgs/sys.PNG)
#### 1.3 RCC CONFIG
![img](/imgs/rcc.PNG)
## **2. Configurations Setup**
### **2.1 Debug Configurations**
![img](/imgs/debug_1.PNG)
> **Note:** Freq in Connection Setup depends on your own MCU So keep your eye on console when debug session starts as it will provide you with the info if you need to adjust freq in debug configuration
**2.2 Autoadjust from ST-Link to the required freq**
![img](/imgs/freq.PNG)
**2.3 When Freq is already adjusted in configuration as in second image**
![img](/imgs/f2.PNG) 

![img](/imgs/debug_2.PNG)
### **3. Paths and Symbols Configurations**
![img](/imgs/paths.PNG)
### **4. STARTUP Configurations**
![img](/imgs/startup.PNG)
### **5. Tool Settings-->Liberaries**
![img](/imgs/rdimon.PNG)
### **6. Tool Settings-->Miscellaneous**
![img](/imgs/flag.PNG)
### **7. Clock Configurations**
![img](/imgs/clock.PNG)
## **8. Code**
### **8.1 Includes**
```
#include <stdio.h>
extern void initialise_monitor_handles(void);
```
### **8.2 Call the function**
#### The rdimon library has to be initialized before it can run properly. It exposes a function to do that, then use it:
```
initialise_monitor_handles();
```
### **8.3 Intialize your PIN to be high**
```
/* Before while(1) */
/* USER CODE BEGIN 2 */
HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);
/* USER CODE END 2 */
```
### **8.4 Inside While(1):To toggle the pin**
```
// LED ON
HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
printf("LED ON\n");
HAL_Delay(5000);
// LED OFF
HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
printf("LED OFF\n");
HAL_Delay(2000);
```
## **9. Finally you need to run the code in the debug mode and that should be your final Outputin the console**
![img](/imgs/console.PNG)
## **10. References**
* https://www.codeinsideout.com/blog/stm32/semihosting/
* https://shawnhymel.com/1795/getting-started-with-stm32-nucleo-usb-virtual-com-port/
## **Demo**

![GIF](/imgs/demo.gif)
## **Conclusion** 
> To sum it up, It isn't the fastest way to debug your code but it's a way to figure out your hardware response espetiallly it was complex and you have limited resources to adjust varibles

