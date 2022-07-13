# Nested Vectored Interrupt Controller

---

# Register
```C
// ! Do not directly manipulate the registers to configure the NVIC
// ! as this is not safe for the entire system.

/* The following is defined on <core_cm0.h> */
typedef struct {                   // <register name>                     | [offset](bytes)
  volatile uint32_t ISER[1];       // <Interrupt Set Enable reg.>         | [0x0000]
  /*const*/uint32_t RESERVED0[31]; // <reserved>                          | [0x0004:0x007C]
  volatile uint32_t ICER[1];       // <Interrupt Clear Enable reg.>       | [0x0080]
  /*const*/uint32_t RESERVED1[31]; // <reserved>                          | [0x0084:0x009C]
  volatile uint32_t ISPR[1];       // <Interrupt Set Pending reg.>        | [0x0100]
  /*const*/uint32_t RESERVED2[31]; // <reserved>                          | [0x0104:0x017C]
  volatile uint32_t ICPR[1];       // <Interrupt Clear Pending reg.>      | [0x0180]
  /*const*/uint32_t RESERVED3[31]; // <reserved>                          | [0x0184:0x019C]
  /*const*/uint32_t RESERVED4[64]; // <reserved>                          | [0x0200:0x029C]
  volatile uint32_t IP[8];         // <Interrupt Priority reg.>           | [0x0300]
} NVIC_TypeDef;

/* The memory is designated in the core-relative area */
#define NVIC              ((NVIC_TypeDef*) 0xE000E100) // ? NVIC register basic address
```
