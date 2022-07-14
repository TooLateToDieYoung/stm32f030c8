# System Tick

---

# Register

## Standard Peripheral Library (by ST company)
* ### Reister Defination
  ```C
  /* STM32F0xx_StdPeriph_Lib_V1.5.0\Libraries\CMSIS\Include\core_cm0.h */
  typedef struct {               // <register name>           | [offset](bytes) | [option]
    volatile uint32_t CTRL;      // <Control and Status reg.> | [0x00]          | [R/W]
    volatile uint32_t LOAD;      // <Reload Value reg.>       | [0x04]          | [R/W]
    volatile uint32_t VAL;       // <Current Value reg.>      | [0x08]          | [R/W]
    volatile uint32_t CALIB;     // <Calibration reg.>        | [0x0C]          | [R/ ]
  } SysTick_Type;

  /* The memory is designated in the core-relative area */
  #define SCS_BASE     (0xE000E000UL)                // System Control Space Base Address
  #define SysTick_BASE (SCS_BASE + 0x0010UL)         // SysTick Base Address
  #define SysTick      ((SysTick_Type*)SysTick_BASE) // SysTick configuration struct
  ```

* ### Configure Fuction Defination
  ```C
  __STATIC_INLINE uint32_t SysTick_Config(uint32_t ticks)
  {
    // Error if reloading the value is impossible
    if ((ticks - 1) > SysTick_LOAD_RELOAD_Msk)  return (1);

    // set reload register
    SysTick->LOAD  = ticks - 1;

    // set Priority for Systick Interrupt
    NVIC_SetPriority (SysTick_IRQn, (1<<__NVIC_PRIO_BITS) - 1);

    // Load the SysTick Counter Value
    SysTick->VAL   = 0;

    // Enable SysTick IRQ and SysTick Timer
    SysTick->CTRL  = SysTick_CTRL_CLKSOURCE_Msk |
                     SysTick_CTRL_TICKINT_Msk   |
                     SysTick_CTRL_ENABLE_Msk    ;

    return (0);
  }
  ```

* ### Using Configure Function
  ```C
  SysTick_Config(SystemCoreClock / 1000); // open for system tick -> period = 1ms
  ```

## ARMv6M
* ### Defination
  ```C
  /* according to ./datasheets/armv6m.pdf */
  typedef struct {               // <register name>           | [offset](bytes) | [option]
    volatile uint32_t CSR;       // <Control and Status reg.> | [0x00]          | [R/W]
    volatile uint32_t RVR;       // <Reload Value reg.>       | [0x04]          | [R/W]
    volatile uint32_t CVR;       // <Current Value reg.>      | [0x08]          | [R/W]
    volatile uint32_t CALIB;     // <Calibration reg.>        | [0x0C]          | [R/ ]
  } SysTick_t;

  #define SysTick      ((SysTick_t*)0xE000E010UL) // SysTick configuration struct
  ```

* ### Configure System Tick
  ```C
  void SysTick_Configure(void)
  {
    const uint32_t SystemCoreClock = 48000000;      // 48MHz
    const uint32_t ticks = SystemCoreClock / 1000 ; // 48M / 1000 = 48k -> period = 1ms
    // At a frequency of 48MHz, it counts 48k times,
    // so the system SysTick frequency is 1kHz ( period is equal to 1ms )

    SysTick->RVR  = ticks - 1;         // set reload register
    SysTick->CVR  = (uint32_t)0;       // clear current value register
    SCB->SHPPR3   = (uint32_t)3 << 30; // set interrupt -> System Handlers Priority = 3
    SysTick->CSR  = (uint32_t)7;       // set CLKSOURCE, TICKINT, ENABLE
  }
  ```