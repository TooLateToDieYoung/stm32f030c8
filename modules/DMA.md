# Direct Memory Access Controller

---

# Register
```C
typedef struct {              // <register name>                     | [offset](bytes)
  volatile uint32_t CCR;      // <channel x configuration reg.>      | [0x08 + (x - 1) * 0x14]
  volatile uint32_t CNDTR;    // <channel x number of data reg.>     | [0x0C + (x - 1) * 0x14]
  volatile uint32_t CPAR;     // <channel x peripheral address reg.> | [0x10 + (x - 1) * 0x14]
  volatile uint32_t CMAR;     // <channel x memory address reg.>     | [0x14 + (x - 1) * 0x14]
  /*const*/uint32_t RESERVED; // <reserved>                          | [0x18 + (x - 1) * 0x14]
} DMA_Channel_TypeDef;

typedef struct {                  // <register name>                 | [offset](bytes)
  volatile uint32_t ISR;          // <interrupt status reg.>         | [0x00]
  volatile uint32_t IFCR;         // <interrupt flag clear reg.>     | [0x04]
  DMA_Channel_TypeDef Channel1;   // <channel 1 registers>           | [0x08:0x18]
  DMA_Channel_TypeDef Channel2;   // <channel 2 registers>           | [0x1C:0x2C]
  DMA_Channel_TypeDef Channel3;   // <channel 3 registers>           | [0x30:0x40]
  DMA_Channel_TypeDef Channel4;   // <channel 4 registers>           | [0x44:0x54]
  DMA_Channel_TypeDef Channel5;   // <channel 5 registers>           | [0x58:0x68]
  /*const*/uint32_t RESERVED[15]; // <reserved>                      | [0x6C:0xA4]
  volatile uint32_t RMPCR;        // <Remap control reg.>            | [0xA8]
} DMA_TypeDef;

#define DMA              ((DMA_TypeDef*) 0x40020000) // ? DMA register basic address
```
