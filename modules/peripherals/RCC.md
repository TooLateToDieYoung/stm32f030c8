# Reset and Clock Control

---

# Register
```C  
typedef struct {              // <register name>                     | [offset](bytes)
  volatile uint32_t CR;       // <clock control reg.>                | [0x00]
  volatile uint32_t CFGR;     // <clock configuration reg.>          | [0x04]
  volatile uint32_t CIR;      // <clock interrupt reg.>              | [0x08]
  volatile uint32_t APB2RSTR; // <APB2 peripheral reset reg.>        | [0x0C]
  volatile uint32_t APB1RSTR; // <APB1 peripheral reset reg.>        | [0x10]
  volatile uint32_t AHBENR;   // <AHB peripheral clock reg.>         | [0x14]
  volatile uint32_t APB2ENR;  // <APB2 peripheral clock enable reg.> | [0x18]
  volatile uint32_t APB1ENR;  // <APB1 peripheral clock enable reg.> | [0x1C]
  volatile uint32_t BDCR;     // <Backup domain control reg.>        | [0x20]
  volatile uint32_t CSR;      // <clock control & status reg.>       | [0x24]
  volatile uint32_t AHBRSTR;  // <AHB peripheral reset reg.>         | [0x28]
  volatile uint32_t CFGR2;    // <clock configuration reg. 2>        | [0x2C]
  volatile uint32_t CFGR3;    // <clock configuration reg. 3>        | [0x30]
  volatile uint32_t CR2;      // <clock control reg. 2>              | [0x34]
} RCC_TypeDef;

#define RCC             ((RCC_TypeDef*) 0x40021000) // ? RCC register basic address
```
