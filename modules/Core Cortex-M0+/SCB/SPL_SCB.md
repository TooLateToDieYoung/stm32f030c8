# Standard Peripheral Library (by ST company)

---

# Reister Defination
```C
// ! Do not directly manipulate the registers to configure the SCB
// ! as this is not safe for the entire system.

/* ./STM32F0xx_StdPeriph_Lib_V1.5.0/Libraries/CMSIS/Include/core_cm0.h */
typedef struct {
  __I  uint32_t CPUID;     /*!< Offset: 0x000 (R/ )  CPUID Base Register                                   */
  __IO uint32_t ICSR;      /*!< Offset: 0x004 (R/W)  Interrupt Control and State Register                  */
       uint32_t RESERVED0;
  __IO uint32_t AIRCR;     /*!< Offset: 0x00C (R/W)  Application Interrupt and Reset Control Register      */
  __IO uint32_t SCR;       /*!< Offset: 0x010 (R/W)  System Control Register                               */
  __IO uint32_t CCR;       /*!< Offset: 0x014 (R/W)  Configuration Control Register                        */
       uint32_t RESERVED1;
  __IO uint32_t SHP[2];    /*!< Offset: 0x01C (R/W)  System Handlers Priority Registers. [0] is RESERVED   */
  __IO uint32_t SHCSR;     /*!< Offset: 0x024 (R/W)  System Handler Control and State Register             */
} SCB_Type;

/* The memory is designated in the core-relative area */
#define SCS_BASE (0xE000E000UL)         /*!< System Control Space Base Address */
#define SCB_BASE (SCS_BASE + 0x0D00UL)  /*!< System Control Block Base Address */
#define SCB      ((SCB_Type*)SCB_BASE)  /*!< SCB configuration struct          */
```