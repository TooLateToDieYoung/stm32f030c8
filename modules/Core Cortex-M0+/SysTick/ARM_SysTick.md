# ARMv6M (by ARM company)

---

# Register Defination
```C
/* according to ../datasheets/armv6m.pdf */
typedef struct {               // <register name>           | [offset](bytes) | [option]
  volatile uint32_t CSR;       // <Control and Status reg.> | [0x00]          | [R/W]
  volatile uint32_t RVR;       // <Reload Value reg.>       | [0x04]          | [R/W]
  volatile uint32_t CVR;       // <Current Value reg.>      | [0x08]          | [R/W]
  volatile uint32_t CALIB;     // <Calibration reg.>        | [0x0C]          | [R/ ]
} SysTick_t;

#define SysTick      ((SysTick_t*)0xE000E010UL) // SysTick configuration struct
```

---

# Configure Fuction Defination
```C
void SysTick_Configure(void)
{
  const uint32_t SystemCoreClock = 48000000;      // 48MHz
  const uint32_t ticks = SystemCoreClock / 1000 ; // 48M / 1000 = 48k -> period = 1ms
  // At a frequency of 48MHz, it counts 48k times,
  // so the system SysTick frequency is 1kHz ( period is equal to 1ms )

  SysTick->RVR = ticks - 1;         // set reload register
  SysTick->CVR = (uint32_t)0;       // clear current value register
  SCB->SHPR3  |= (uint32_t)3 << 30; // set interrupt -> System Handlers Priority = 3
  SysTick->CSR = (uint32_t)7;       // set CLKSOURCE, TICKINT, ENABLE
}
```