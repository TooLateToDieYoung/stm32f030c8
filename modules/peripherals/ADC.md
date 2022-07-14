# Analog-to-Digital Converter

---

# Register
```C
typedef struct {                  // <register name>                     | [offset](bytes)
  volatile uint32_t ISR;          // <interrupt and status reg.>         | [0x00]
  volatile uint32_t IER;          // <interrupt Enable reg.>             | [0x04]
  volatile uint32_t CR;           // <Control reg.>                      | [0x08]
  volatile uint32_t CFGR1;        // <Configuration reg. 1>              | [0x0C]
  volatile uint32_t CFGR2;        // <Configuration reg. 2>              | [0x10]
  volatile uint32_t SMPR;         // <Sampling time reg.>                | [0x14]
  /*const*/uint32_t RESERVED1[2]; // <reserved>                          | [0x18:0x1C]
  volatile uint32_t TR;           // <watchdog threshold reg.>           | [0x20]
  /*const*/uint32_t RESERVED2;    // <reserved>                          | [0x24]
  volatile uint32_t CHSELR;       // <channel selection reg.>            | [0x28]
  /*const*/uint32_t RESERVED3[5]; // <reserved>                          | [0x2C:0x3C]
  volatile uint32_t DR;           // <data reg.>                         | [0x40]
} ADC_TypeDef;

typedef struct {                  // <register name>                     | [offset](bytes)
  volatile uint32_t CCR;          // <common configuration reg.>         | [0x00]
} ADC_Common_TypeDef;

#define ADC1              ((ADC_TypeDef*)        0x40012400) // ? ADC1 register basic address
#define ADC               ((ADC_Common_TypeDef*) 0x40012708) // ? ADC  register basic address
```
