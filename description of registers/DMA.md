## the process of initializing the DMA

1. Enable the peripheral clock on DMA
    ```C
    RCC->AHBENR |= RCC_AHBENR_DMA1EN;
    ```

2. Configure the source address
    ```C
    // ? DMA1_Channelx means channel x in DMA1
    // ! if channel 1 in DMA1 is selected -> DMA1_Channel1

    // e.g. ADC1->DR or GPIOB->BSRR etc.
    DMA1_Channel1->CPAR = (uint32_t)(SourceAddress);
    ```

3. Configure the destination address
    ```C
    #define TRANFER_SIZE 5                      // user-defined
    uint32_t myDestinationBuffer[TRANFER_SIZE]; // user-defined
    DMA1_Channel1->CMAR = (uint32_t)(myDestinationBuffer);
    ```

4. Configure the number of DMA tranfer to be performs on DMA channel x
    ```C
    #define TRANFER_SIZE 5 // user-defined
    DMA1_Channel1->CNDTR = TRANFER_SIZE;
    ```

5. Configure other details

    * about interrupt
      ```C
      // Enable interrupts for the selected channel
      DMA1->ISR |= ??? ;
      ```

    * about each channel configuration
      ```C
      // clear all of old configurations
      DMA1_Channel1->CCR &= ~((uint32_t)0);

      // write new configuration
      DMA1_Channel1->CCR |= ??? ;
      ```

6. Enable DMA Channel x
    ```C
    // ! must be set in the end

    // Enable the channel 1 in DMA1
    DMA1_Channel1->CCR |= DMA_CCR_EN;
    ```

---
## Introduction of DMA1 registers

1. stm32f030c8 has 7 channels of DMA1
    - each channel is connected to some designated peripherals
    - each channel works independently
  
2. interrupts and status are managed by shared registers
    - check status through shared registers
      => DMA1->ISR and DMA1->IFCR
  
3. other configurations of DMA is splited into several register
    => Each channel records its own transmission information ( x = 1..7 )
    - DMA1_Channelx->CPAR  ( DMA_CPARx )
    - DMA1_Channelx->CMAR  ( DMA_CMAR1 )
    - DMA1_Channelx->CNDTR ( DMA_CNDTRx )
    - DMA1_Channelx->CCR   ( DMA_CCRx )

---
## Description of DMA1 registers

1. DMA interrupt status register (DMA1->ISR) 32bits
    ```C
    // ? x = 1..7 for DMA1

    // ? These bits are cleared by writing 1 to
    //   the corresponding bit in the DMA1->IFCR register
    ```
    * Channel x transfer error flag : TEIFx
      - 0 -> No transfer error (TE) on channel x
      - 1 -> A transfer error (TE) occurred on channel x
    
    * Channel x half transfer flag : HTIFx
      - 0 -> No half transfer (HT) event on channel x
      - 1 -> A half transfer (HT) event occurred on channel x

    * Channel x transfer complete flag : TCIFx
      - 0 -> No transfer complete (TC) event on channel x
      - 1 -> A transfer complete (TC) event occurred on channel x

    * Channel x global interrupt flag : GIFx
      - 0 -> No TE, HT or TC event on channel x
      - 1 -> A TE, HT or TC event occurred on channel x

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|TEIF7|HTIF7|TCIF7|GIF7|TEIF6|HTIF6|TCIF6|GIF6|TEIF5|HTIF5|TCIF5|GIF5|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|TEIF4|HTIF4|TCIF4|GIF4|TEIF3|HTIF3|TCIF3|GIF3|TEIF2|HTIF2|TCIF2|GIF2|TEIF1|HTIF1|TCIF1|GIF1|

---
2. DMA interrupt flag clear register ( DMA1->IFCR ) 32bits
    ```C
    // ? x = 1..7 for DMA1

    // ? write 1 to these bits to 
    //   clear the corresponding bit in the DMA1->ISR register
    ```
    * CTEIFx <-> TEIFx

    * CHTIFx <-> HTIFx

    * CTCIFx <-> TCIFx

    * CGIFx  <-> GIFx

---
3. DMA channel x configuration register ( DMA1_Channelx->CCR ) 32bits

    * Memory to memory mode : MEM2MEM
      - 0 -> Memory to memory mode disabled
      - 1 -> Memory to memory mode enabled

    * Channel priority level : PL[1:0]
      - 00 -> Low
      - 01 -> Medium
      - 10 -> High
      - 11 -> Very high

    * Memory size : MSIZE[1:0]
      ? destination size
      - 00 ->  8bits
      - 01 -> 16bits
      - 10 -> 32bits
      - 11 -> Reserved

    * Peripheral size : PSIZE[1:0]
      ? source size
      - 00 ->  8bits
      - 01 -> 16bits
      - 10 -> 32bits
      - 11 -> Reserved

    * Memory increment mode : MINC
      ```C
      // ? destination address increment after each DMA operation
      ```
      - 0 -> Memory increment mode disabled
      - 1 -> Memory increment mode enabled

    * Peripheral increment mode : PINC
      ```C
      // ? source address increment after each DMA operation
      ```
      - 0 -> Peripheral increment mode disabled
      - 1 -> Peripheral increment mode enabled

    * Circular mode : CIRC
      ```C
      // ? if the series ends, automatically return to the first destination address
      ```
      - 0 -> Circular mode disabled
      - 1 -> Circular mode enabled

    * Data transfer direction : DIR
      - 0 -> Read from peripheral
      - 1 -> Read from memory

    * Transfer error interrupt enable : TEIE
      - 0 -> TE interrupt disabled
      - 1 -> TE interrupt enabled

    * Half transfer interrupt enable : HTIE
      - 0 -> HT interrupt disabled
      - 1 -> HT interrupt enabled

    * Transfer complete interrupt enable : TCIE
      - 0 -> TC interrupt disabled
      - 1 -> TC interrupt enabled

    * Channel enable : EN
      ! must be enabled in final
      - 0 -> Channel disabled
      - 1 -> Channel enabled

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|MEM2MEM|PL[1:0]||MSIZE[1:0]||PSIZE[1:0]||MINC|PINC|CIRC|DIR|TEIE|HTIE|TCIE|EN|

---
4. DMA channel x peripheral address register ( DMA1_Channelx->CPAR ) 32bits

    * Peripheral address : PA[31:0] ( B[31:0] )

---
5.  DMA channel x memory address register ( DMA1_Channelx->CMAR ) 32bits

    * Memory address : MA[31:0] ( B[31:0] )

---
6. DMA channel x number of data register ( DMA1_Channelx->CNDTR ) 32bits

    * Number of data to transfer : NDT[15:0] ( B[15:0] )
      ```C
      // ? Use lower 16 bits -> 0 up to 65535
      ```
