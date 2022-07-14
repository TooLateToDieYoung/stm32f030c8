# General Purpose Input / Output

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t MODER;     // <mode reg.>                         | [0x00]
  volatile uint16_t OTYPER;    // <output type reg.>                  | [0x04]
  /*const*/uint16_t RESERVED0; // <Reserved>                          | [0x06]
  volatile uint32_t OSPEEDR;   // <output speed reg.>                 | [0x08]
  volatile uint32_t PUPDR;     // <pull-up / pull-down reg.>          | [0x0C]
  volatile uint16_t IDR;       // <input data reg.>                   | [0x10]
  /*const*/uint16_t RESERVED1; // <Reserved>                          | [0x12]
  volatile uint16_t ODR;       // <output data reg.>                  | [0x14]
  /*const*/uint16_t RESERVED2; // <Reserved>                          | [0x16]
  volatile uint32_t BSRR;      // <bit set / reset reg.>              | [0x18]
  volatile uint32_t LCKR;      // <configuration lock reg.>           | [0x1C]
  volatile uint32_t AFRL;      // <GPIO alternate function reg. low>  | [0x20]
  volatile uint32_t AFRH;      // <GPIO alternate function reg. high> | [0x24]
  volatile uint16_t BRR;       // <bit reset reg.>                    | [0x28]
  /*const*/uint16_t RESERVED3; // <Reserved>                          | [0x2A]
} GPIO_TypeDef;

#define GPIOA             ((GPIO_TypeDef*) 0x48000000) // ? GPIOA register basic address
#define GPIOB             ((GPIO_TypeDef*) 0x48000400) // ? GPIOB register basic address
#define GPIOC             ((GPIO_TypeDef*) 0x48000800) // ? GPIOC register basic address
#define GPIOD             ((GPIO_TypeDef*) 0x48000C00) // ? GPIOD register basic address
#define GPIOE             ((GPIO_TypeDef*) 0x48001000) // ? GPIOE register basic address
#define GPIOF             ((GPIO_TypeDef*) 0x48001400) // ? GPIOF register basic address
```
