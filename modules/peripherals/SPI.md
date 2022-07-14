# Serial Peripheral Interface

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint16_t CR1;       // <Control reg. 1>                    | [0x00]
  /*const*/uint16_t RESERVED0; // <Reserved>                          | [0x02]
  volatile uint16_t CR2;       // <Control reg. 2>                    | [0x04]
  /*const*/uint16_t RESERVED1; // <Reserved>                          | [0x06]
  volatile uint16_t SR;        // <Status reg.>                       | [0x08]
  /*const*/uint16_t RESERVED2; // <Reserved>                          | [0x0A]
  volatile uint16_t DR;        // <data reg.>                         | [0x0C]
  /*const*/uint16_t RESERVED3; // <Reserved>                          | [0x0E]
  volatile uint16_t CRCPR;     // <CRC polynomial reg.>               | [0x10]
  /*const*/uint16_t RESERVED4; // <Reserved>                          | [0x12]
  volatile uint16_t RXCRCR;    // <Rx CRC reg.>                       | [0x14]
  /*const*/uint16_t RESERVED5; // <Reserved>                          | [0x16]
  volatile uint16_t TXCRCR;    // <Tx CRC reg.>                       | [0x18]
  /*const*/uint16_t RESERVED6; // <Reserved>                          | [0x1A]
  volatile uint16_t I2SCFGR;   // <SPI I2S mode configuration reg.>   | [0x1C]
  /*const*/uint16_t RESERVED7; // <Reserved>                          | [0x1E]
  volatile uint16_t I2SPR;     // <SPI I2S mode prescaler reg.>       | [0x20]
  /*const*/uint16_t RESERVED8; // <Reserved>                          | [0x22]
} SPI_TypeDef;

#define SPI1              ((SPI_TypeDef*) 0x40013000) // ? SPI1 register basic address
#define SPI2              ((SPI_TypeDef*) 0x40003800) // ? SPI2 register basic address
```
