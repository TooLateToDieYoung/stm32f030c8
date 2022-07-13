# System Window Watch Dog

---

# Register
```C
typedef struct {               // <register name>                     | [offset](bytes)
  volatile uint32_t CR;        // <Control reg.>                      | [0x00]
  volatile uint32_t CFR;       // <Configuration reg.>                | [0x04]
  volatile uint32_t SR;        // <Status reg.>                       | [0x08]
} WWDG_TypeDef;

#define WWDG              ((WWDG_TypeDef*) 0x40002C00) // ? WWDG register basic address
```
