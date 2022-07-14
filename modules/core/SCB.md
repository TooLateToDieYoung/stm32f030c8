# System Control Block

---

# Register
```C
// ! Do not directly manipulate the registers to configure the SCB
// ! as this is not safe for the entire system.

/* The following is defined on <core_cm0.h> */
typedef struct {               // <register name>                                | [offset](bytes) | [option]
  volatile uint32_t CPUID;     // <CPUID Base reg.>                              | [0x00]          | [R/ ]
  volatile uint32_t ICSR;      // <Interrupt Control and State reg.>             | [0x04]          | [R/W]
  /*const*/uint32_t RESERVED0;
  volatile uint32_t AIRCR;     // <Application Interrupt and Reset Control reg.> | [0x0C]          | [R/W]
  volatile uint32_t SCR;       // <System Control reg.>                          | [0x10]          | [R/W]
  volatile uint32_t CCR;       // <Configuration Control reg.>                   | [0x14]          | [R/W]
  /*const*/uint32_t RESERVED1;
  /*const*/uint16_t RESERVED2;
  volatile uint16_t SHP;       // <System Handlers Priority reg.>                | [0x20]          | [R/W]
  volatile uint32_t SHCSR;     // <System Handler Control and State reg.>        | [0x24]          | [R/W]
} SCB_TypeDef;

/* The memory is designated in the core-relative area */
#define SCB              ((SCB_TypeDef*) 0xE000ED00) // ? SCB register basic address
```
