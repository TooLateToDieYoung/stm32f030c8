# Inter-Integrated Circuit

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t CR1;       // <Control reg. 1>                    | [0x00]
  volatile uint32_t CR2;       // <Control reg. 2>                    | [0x04]
  volatile uint32_t OAR1;      // <Own address reg. 1>                | [0x08]
  volatile uint32_t OAR2;      // <Own address reg. 2>                | [0x0C]
  volatile uint32_t TIMINGR;   // <Timing reg.>                       | [0x10]
  volatile uint32_t TIMEOUTR;  // <Timeout reg.>                      | [0x14]
  volatile uint32_t ISR;       // <Interrupt and status reg.>         | [0x18]
  volatile uint32_t ICR;       // <Interrupt clear reg.>              | [0x1C]
  volatile uint32_t PECR;      // <Packet error checking reg.>        | [0x20]
  volatile uint32_t RXDR;      // <Receive data reg.>                 | [0x24]
  volatile uint32_t TXDR;      // <Transmit data reg.>                | [0x28]
} I2C_TypeDef;

#define I2C1              ((I2C_TypeDef*) 0x40005400) // ? I2C1 register basic address
#define I2C2              ((I2C_TypeDef*) 0x40005800) // ? I2C2 register basic address
```
