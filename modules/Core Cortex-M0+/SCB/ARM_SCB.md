# ARMv6M (by ARM company)

---

# Reister Defination
```C
/* according to ../datasheets/armv6m.pdf */
typedef struct {              // <register name>                                | [offset](bytes) | [option]
  volatile uint32_t CPUID;    // <CPUID Base reg.>                              | [0x00]          | [R/ ]
  volatile uint32_t ICSR;     // <Interrupt Control and State reg.>             | [0x04]          | [R/W]
  volatile uint32_t VTOR;     // <Vector Table Offset reg.>                     | [0x08]          | [R/W]
  volatile uint32_t AIRCR;    // <Application Interrupt and Reset Control reg.> | [0x0C]          | [R/W]
  volatile uint32_t SCR;      // <System Control reg.>                          | [0x10]          | [R/W]
  volatile uint32_t CCR;      // <Configuration Control reg.>                   | [0x14]          | [R/W]
  /*const*/uint32_t RESERVED; // <reserved>                                     | [0x18]          | [ / ]
  volatile uint32_t SHPR2;    // <System Handlers Priority reg. 2>              | [0x1C]          | [R/W]
  volatile uint32_t SHPR3;    // <System Handlers Priority reg. 3>              | [0x20]          | [R/W]
  volatile uint32_t SHCSR;    // <System Handler Control and State reg.>        | [0x24]          | [R/W]
} SCB_t;

/* The memory is designated in the core-relative area */
#define SCB              ((SCB_t*) 0xE000ED00) // ? SCB configuration struct
```