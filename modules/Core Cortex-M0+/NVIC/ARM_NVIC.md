# ARMv6M (by ARM company)

---

# Register Defination
```C
/* according to ../datasheets/armv6m.pdf */
typedef struct {                   // <register name>                | [offset](bytes) | [option]
  volatile uint32_t ISER[1];       // <Interrupt Set Enable reg.>    | [0x0000]        | [R/W]
  /*const*/uint32_t RESERVED0[31]; // <reserved>                     | [0x0004:0x007C] | [ / ]
  volatile uint32_t ICER[1];       // <Interrupt Clear Enable reg.>  | [0x0080]        | [R/W]
  /*const*/uint32_t RESERVED1[31]; // <reserved>                     | [0x0084:0x009C] | [ / ]
  volatile uint32_t ISPR[1];       // <Interrupt Set Pending reg.>   | [0x0100]        | [R/W]
  /*const*/uint32_t RESERVED2[31]; // <reserved>                     | [0x0104:0x017C] | [ / ]
  volatile uint32_t ICPR[1];       // <Interrupt Clear Pending reg.> | [0x0180]        | [R/W]
  /*const*/uint32_t RESERVED3[31]; // <reserved>                     | [0x0184:0x019C] | [ / ]
  /*const*/uint32_t RESERVED4[64]; // <reserved>                     | [0x0200:0x029C] | [ / ]
  volatile uint32_t IP[8];         // <Interrupt Priority reg.>      | [0x0300]        | [R/W]
} NVIC_t;

/* The memory is designated in the core-relative area */
#define NVIC              ((NVIC_t*) 0xE000E100) // ? NVIC configuration struct
```
