# Standard Peripheral Library (by ST company)

---

# Reister Defination
```C
// ! Do not directly manipulate the registers to configure the SCB
// ! as this is not safe for the entire system.

/* ./STM32F0xx_StdPeriph_Lib_V1.5.0/Libraries/CMSIS/Include/core_cm0.h */
typedef struct {
  __IO uint32_t CTRL;  /*!< Offset: 0x000 (R/W)  SysTick Control and Status Register */
  __IO uint32_t LOAD;  /*!< Offset: 0x004 (R/W)  SysTick Reload Value Register       */
  __IO uint32_t VAL;   /*!< Offset: 0x008 (R/W)  SysTick Current Value Register      */
  __I  uint32_t CALIB; /*!< Offset: 0x00C (R/ )  SysTick Calibration Register        */
} SysTick_Type;

/* The memory is designated in the core-relative area */
#define SCS_BASE     (0xE000E000UL)                // System Control Space Base Address
#define SysTick_BASE (SCS_BASE + 0x0010UL)         // SysTick Base Address
#define SysTick      ((SysTick_Type*)SysTick_BASE) // SysTick configuration struct
```

---

# Configure Fuction Defination
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
  SysTick->CTRL  = SysTick_CTRL_CLKSOURCE_Msk | SysTick_CTRL_TICKINT_Msk | SysTick_CTRL_ENABLE_Msk ;

  return (0);
}
```

---

# Using Configure Function
```C
SysTick_Config(SystemCoreClock / 1000); // open for system tick -> period = 1ms
```