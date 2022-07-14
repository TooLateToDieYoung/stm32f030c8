# Independent Watch Dog

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t KR;        // <Key reg.>                          | [0x00]
  volatile uint32_t PR;        // <Prescaler reg.>                    | [0x04]
  volatile uint32_t RLR;       // <Reload reg.>                       | [0x08]
  volatile uint32_t SR;        // <Status reg.>                       | [0x0C]
  volatile uint32_t WINR;      // <Window reg.>                       | [0x10]
} IWDG_TypeDef;

#define IWDG              ((IWDG_TypeDef*) 0x40003000) // ? IWDG register basic address
```
