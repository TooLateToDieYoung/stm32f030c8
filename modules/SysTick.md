# System Tick

---

# Register
```C
/* The following is defined on <core_cm0.h> */
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t CTRL;      // <Control and Status reg.>           | [0x00]
  volatile uint32_t LOAD;      // <Reload Value reg.>                 | [0x04]
  volatile uint32_t VAL;       // <Current Value reg.>                | [0x08]
  volatile uint32_t CALIB;     // <Calibration reg.>                  | [0x0C]
} SysTick_TypeDef;

/* The memory is designated in the core-relative area */
#define SysTick              ((SysTick_TypeDef*) 0xE000E010) // ? SysTick register basic address
```
