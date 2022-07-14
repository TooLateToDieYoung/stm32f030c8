# Timer

---

# Register
```C
typedef struct {                  // <register name>                      | [offset](bytes)
  volatile uint16_t CR1;          // <control reg. 1>                     | [0x00]
  /*const*/uint16_t RESERVED01;   // <reserved>                           | [0x02]
  volatile uint16_t CR2;          // <control reg. 2>                     | [0x04]
  /*const*/uint16_t RESERVED02;   // <reserved>                           | [0x06]
  volatile uint16_t SMCR;         // <slave Mode Control reg.>            | [0x08]
  /*const*/uint16_t RESERVED03;   // <reserved>                           | [0x0A]
  volatile uint16_t DIER;         // <DMA / interrupt enable reg.>        | [0x0C]
  /*const*/uint16_t RESERVED04;   // <reserved>                           | [0x0E]
  volatile uint16_t SR;           // <status reg.>                        | [0x10]
  /*const*/uint16_t RESERVED05;   // <reserved>                           | [0x12]
  volatile uint16_t EGR;          // <event generation reg.>              | [0x14]
  /*const*/uint16_t RESERVED06;   // <reserved>                           | [0x16]
  volatile uint16_t CCMR1;        // <capture / compare mode reg. 1>      | [0x18]
  /*const*/uint16_t RESERVED07;   // <reserved>                           | [0x1A]
  volatile uint16_t CCMR2;        // <capture / compare mode reg. 2>      | [0x1C]
  /*const*/uint16_t RESERVED08;   // <reserved>                           | [0x1E]
  volatile uint16_t CCER;         // <capture / compare enable reg.>      | [0x20]
  /*const*/uint16_t RESERVED09;   // <reserved>                           | [0x22]
  volatile uint32_t CNT;          // <counter reg.>                       | [0x24]
  volatile uint16_t PSC;          // <prescaler reg.>                     | [0x28]
  /*const*/uint16_t RESERVED10;   // <reserved>                           | [0x2A]
  volatile uint32_t ARR;          // <auto-reload reg.>                   | [0x2C]
  volatile uint16_t RCR;          // <repetition counter reg.>            | [0x30]
  /*const*/uint16_t RESERVED12;   // <reserved>                           | [0x32]
  volatile uint32_t CCR1;         // <capture / compare reg. 1>           | [0x34]
  volatile uint32_t CCR2;         // <capture / compare reg. 2>           | [0x38]
  volatile uint32_t CCR3;         // <capture / compare reg. 3>           | [0x3C]
  volatile uint32_t CCR4;         // <capture / compare reg. 4>           | [0x40]
  volatile uint16_t BDTR;         // <break and dead-time reg.>           | [0x44]
  /*const*/uint16_t RESERVED17;   // <reserved>                           | [0x46]
  volatile uint16_t DCR;          // <DMA control reg.>                   | [0x48]
  /*const*/uint16_t RESERVED18;   // <reserved>                           | [0x4A]
  volatile uint16_t DMAR;         // <DMA address for full transfer reg.> | [0x4C]
  /*const*/uint16_t RESERVED19;   // <reserved>                           | [0x4E]
  volatile uint16_t OR;           // <option reg.>                        | [0x50]
  /*const*/uint16_t RESERVED20;   // <reserved>                           | [0x52]
} TIM_TypeDef;

/* Advanced-control timer */
#define TIM1              ((TIM_TypeDef*) 0x40012C00) // ? TIM1  register basic address

/* General-purpose timers */ // ? 4 channels
#define TIM2              ((TIM_TypeDef*) 0x40000000) // ? TIM2  register basic address
#define TIM3              ((TIM_TypeDef*) 0x40000400) // ? TIM3  register basic address

/* General-purpose timers */ // ? 1 channel
#define TIM14             ((TIM_TypeDef*) 0x40002000) // ? TIM14 register basic address

/* General-purpose timers */ // ? 2 channels
#define TIM16             ((TIM_TypeDef*) 0x40014400) // ? TIM16 register basic address
#define TIM17             ((TIM_TypeDef*) 0x40014800) // ? TIM17 register basic address
```