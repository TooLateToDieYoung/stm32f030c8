# System Control Block

---

# Register

## In the datasheet
|offset|Name|Type|Description|
|:-:||:-:||:-:||:-:|
|0xE000E008|ACTLR|RW|
|0xE000ED00|CPUID|RO|
|0xE000ED04|ICSR|RW|
|0xE000ED08|VTOR|RW|
|0xE000ED0C|AIRCR|RW|
|0xE000ED10|SCR|RW|
|0xE000ED14|CCR|RO|
|0xE000ED1C|SHPR2|RW|
|0xE000ED20|SHPR3|RW|
|0xE000ED24|SHCSR|RW|
|0xE000ED30|DFSR|RW|

```C
// ! Do not directly manipulate the registers to configure the SCB
// ! as this is not safe for the entire system.

/* The following is defined on <core_cm0.h> */
typedef struct {               // <register name>                                | [offset](bytes) | [option]
  volatile uint32_t CPUID;     // <CPUID Base reg.>                              | [0x00]          | [R/ ]
  volatile uint32_t ICSR;      // <Interrupt Control and State reg.>             | [0x04]          | [R/W]
  /*const*/uint32_t RESERVED0; // <reserved>                                     | [0x08]          | [ / ]
  volatile uint32_t AIRCR;     // <Application Interrupt and Reset Control reg.> | [0x0C]          | [R/W]
  volatile uint32_t SCR;       // <System Control reg.>                          | [0x10]          | [R/W]
  volatile uint32_t CCR;       // <Configuration Control reg.>                   | [0x14]          | [R/W]
  /*const*/uint32_t RESERVED1; // <reserved>                                     | [0x18]          | [ / ]
  /*const*/uint32_t RESERVED2; // <reserved>                                     | [0x1C]          | [ / ]
  volatile uint32_t SHPPR3;    // <System Handlers Priority reg. 3>              | [0x20]          | [R/W]
  volatile uint32_t SHCSR;     // <System Handler Control and State reg.>        | [0x24]          | [R/W]
} SCB_TypeDef;

/* The memory is designated in the core-relative area */
#define SCB              ((SCB_TypeDef*) 0xE000ED00) // ? SCB register basic address
```
