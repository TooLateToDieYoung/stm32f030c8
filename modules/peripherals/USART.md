# Universal Synchronous Asynchronous Eeceiver Transmitter

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t CR1;       // <Control reg. 1>                    | [0x00]
  volatile uint32_t CR2;       // <Control reg. 2>                    | [0x04]
  volatile uint32_t CR3;       // <Control reg. 3>                    | [0x08]
  volatile uint16_t BRR;       // <Baud rate reg.>                    | [0x0C]
  /*const*/uint16_t RESERVED1; // <Reserved>                          | [0x0E]
  volatile uint16_t GTPR;      // <Guard time and prescaler reg.>     | [0x10]
  /*const*/uint16_t RESERVED2; // <Reserved>                          | [0x12]
  volatile uint32_t RTOR;      // <Receiver Time Out reg.>            | [0x14]
  volatile uint16_t RQR;       // <Request reg.>                      | [0x18]
  /*const*/uint16_t RESERVED3; // <Reserved>                          | [0x1A]
  volatile uint32_t ISR;       // <Interrupt and status reg.>         | [0x1C]
  volatile uint32_t ICR;       // <Interrupt flag Clear reg.>         | [0x20]
  volatile uint16_t RDR;       // <Receive Data reg.>                 | [0x24]
  /*const*/uint16_t RESERVED4; // <Reserved>                          | [0x26]
  volatile uint16_t TDR;       // <Transmit Data reg.>                | [0x28]
  /*const*/uint16_t RESERVED5;  // <Reserved>                         | [0x2A]
} USART_TypeDef;

#define USART1             ((USART_TypeDef*) 0x40013800) // ? USART1 register basic address
#define USART2             ((USART_TypeDef*) 0x48000400) // ? USART2 register basic address
```
