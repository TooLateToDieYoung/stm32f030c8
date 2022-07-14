# Standard Peripheral Library (by ST company)

---

# Reister Defination
```C
// ! Do not directly manipulate the registers to configure the NVIC
// ! as this is not safe for the entire system.

/* ./STM32F0xx_StdPeriph_Lib_V1.5.0/Libraries/CMSIS/Include/core_cm0.h */
typedef struct {
  __IO uint32_t ISER[1];       /*!< Offset: 0x000 (R/W)  Interrupt Set Enable Register    */
       uint32_t RESERVED0[31];
  __IO uint32_t ICER[1];       /*!< Offset: 0x080 (R/W)  Interrupt Clear Enable Register  */
       uint32_t RSERVED1[31];
  __IO uint32_t ISPR[1];       /*!< Offset: 0x100 (R/W)  Interrupt Set Pending Register   */
       uint32_t RESERVED2[31];
  __IO uint32_t ICPR[1];       /*!< Offset: 0x180 (R/W)  Interrupt Clear Pending Register */
       uint32_t RESERVED3[31];
       uint32_t RESERVED4[64];
  __IO uint32_t IP[8];         /*!< Offset: 0x300 (R/W)  Interrupt Priority Register      */
} NVIC_Type;


/* The memory is designated in the core-relative area */
#define SCS_BASE  (0xE000E000UL)          /*!< System Control Space Base Address */
#define NVIC_BASE (SCS_BASE + 0x0100UL)   /*!< NVIC Base Address                 */
#define NVIC      ((NVIC_Type*)NVIC_BASE) /*!< NVIC configuration struct         */
```
