#### The process of initializing the ADC1

1. open ADC1, GPIOx clocks and set pins mode
	```C
	/*
	- stm32f030c8 ADC1 clock tree
		| Synchronous: HSE -> PLLCLK -> SYSCLK -> AHB1 -> APB2(PCLK)
		| Asynchronous: HSI14
	*/

	// ! must be checked in datasheet
	#define ADC_Ch0_Pin GPIO_Pin_0 // PB0
	#define ADC_Ch1_Pin GPIO_Pin_1 // PB1
	#define ADC_Pins    ( ADC_Ch0_Pin | ADC_Ch1_Pin )

	// set ADCEN ( ADC clock )
	RCC->APB2ENR |= RCC_APB2ENR_ADCEN;

	// set IOPBEN ( pin clock for reading )
	RCC->AHBENR |= RCC_AHBENR_GPIOBEN;

	// GPIO Analog In/Out Mode
	GPIOB->MODER |= ( GPIO_Mode_AN << ( ADC_Pins * 2 ) );
	```

2. calibrate ADC1 if long time unused
	```C
	// ! make sure ADC1->CR == 0

	// ? ensure ADSTART == 0
	if( ADC1->CR & ADC_CR_ADSTART ) {
		ADC1->CR |= ADC_CR_ADSTP;
		while( ADC1->CR & ADC_CR_ADSTP );
	}

	// ? ensure ADEN == 0
	if( ADC1->CR & ADC_CR_ADEN ) {
		ADC1->CR |= ADC_CR_ADDIS;
		while( ADC1->CR & ADC_CR_ADDIS );
	}

	// calibrate ADC1
	ADC1->CR |= ADC_CR_ADCAL;
	while( ADC1->CR & ADC_CR_ADCAL );

	// read calibration factor out
	uint32_t temp = ADC1->DR;
	```

3. configure to ADC1 registers
	```C
	// ! ensure ADEN == 1 && ADSTART == 0
	ADC1->CR |= ADC_CR_ADEN;
	while( !( ADC1->ISR & ADC_ISR_ADRDY ) );

	// clear old resolution configure
	ADC1->CFGR1 &= ~( ADC_CFGR1_RES );

	ADC1->CFGR1 |= ( 
		ADC_CFGR1_CONT      | // continue scan
		ADC_CFGR1_OVRMOD    | // verwrite new data
		((uint32_t)2 << 3)    // 8 bits resolution
	);

	ADC1->CFGR1 &= ~( 
		ADC_CFGR1_DISCEN  | // disable discontinuoue
		ADC_CFGR1_ALIGN   | // use align right
		ADC_CFGR1_SCANDIR   // use scan form ch0 to ch18
	);

	// clear old clock mode configure
	ADC1->CFGR2 &= ~( ADC_CFGR2_CKMODE );

	// ADC clocked by PCLK/4
	ADC1->CFGR2 |= ((uint32_t)1 << 30); // ! ADC clock must <= 14MHz

	// clear old sampling time configure
	ADC1->SMPR &= ~( ADC_SMPR_SMP );

	// set 55.5 ADC clock cycles
	ADC1->SMPR |= ((uint32_t)5);

	// select ch0 & ch1 to be converted ( mapping to GPIOx )
	ADC1->CHSELR = ( ADC_CHSELR_CHSEL0 | ADC_CHSELR_CHSEL1 );
	```
  
4. set method of receiving data
	- use DMA ( ADC1->CFGR1 )
		```C
		// open the DMA for ADC1 module
		ADC1->CFGR1 |= ( ADC_CFGR1_DMAEN & ADC_CFGR1_DMACFG );
		```
	- use interrupt ( ADC1->IER )
		```C
		// TODO
		// ADC1->IER |= ???
		```

---
#### The process of receiving data for ADC1
```C
ADC1->CR |= ADC_CR_ADSTART;
```

---
#### Register rules of ADC1

1. can write to ADCAL & ADEN if ADEN == 0

2. can write to ADSTART & ADDIS if ADEN = 1 & ADDIS = 0 

3. can write configures into ADC1 registers if ADEN = 1 & ADSTART = 0
	- the following 6 registers should satisfy
		=> IER, CFGRx, SMPR, TR, CHSELR, CCR

---
#### Description of ADC1 registers

1. ADC interrupt and status register ( ADC1->ISR ) 32bits

	* ADC overrun : OVR
		=> It is cleared by software writing 1 to it
		| 0 -> No overrun occurred
		| 1 -> Overrun has occurred

	* End of sequence flag : EOSEQ
		=> It is cleared by software writing 1
		| 0 -> Conversion sequence not complete
		| 1 -> Conversion sequence complete

	* End of conversion flag : EOC
		=> It is cleared by software writing 1 or by reading the ADC1->DR
		| 0 -> Channel conversion not complete
		| 1 -> Channel conversion complete

	* ADC ready : ADRDY
		=> It is set by hardware after the ADC has been enabled ( ADEN = 1 )
		| 0 -> ADC not yet ready to start conversion
		| 1 -> ADC is ready to start conversion

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|AWD|x|x|OVR|EOSEQ|EOC|x|ADRDY|

---

2. ADC control register ( ADC1->CR ) 32bits

	* ADC calibration : ADCAL
		=> be allowed to set when all bits of ADC1->CR registers are 0
		| 0 -> Calibration complete
		| 1 -> Write 1 to calibrate the ADC
		| 1 -> Read 1 means that a calibration is in progress

	* ADC stop conversion command : ADSTP
		=> write 1 is only effective when ( ADSTART == 1 && ADDIS == 0 ) 
		| 0 -> No ADC stop conversion command ongoing
		| 1 -> Write 1 to stop the ADC
		| 1 -> Read 1 means that an ADSTP command is in progress

	* ADC start conversion command : ADSTART
		=> be allowed to set when ( ADEN == 1 && ADDIS == 0 )
		| 0 -> No ADC conversion is ongoing
		| 1 -> Write 1 to start the ADC conversion
		| 1 -> Read 1 means that the ADC is operating and may be converting

	* ADC disable command : ADDIS
		=> write 1 is only effective when ( ADEN == 1 && ADSTART == 0 )
		| 0 -> No ADDIS command ongoing
		| 1 -> Write 1 to disable the ADC
		| 1 -> Read 1 means that an ADDIS command is in progress

	* ADC enable command : ADEN
		=> be allowed to set when all bits of ADC1->CR registers are 0
		| 0 -> No ADDIS command ongoing
		| 1 -> Write 1 to disable the ADC
		| 1 -> Read 1 means that an ADDIS command is in progress

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|ADCAL|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|ADSTP|x|ADSTART|ADDIS|ADEN|

---

3. ADC configuration register 1 ( ADC1->CFGR1 ) 32bits

	* Discontinuous mode : DISCEN
		=> if set it, ADC just read 1 data per time
		| 0 -> Discontinuous mode disabled
		| 1 -> Discontinuous mode enabled

	* Single / continuous conversion mode : CONT
		=> if set it, ADC will repeat when the end of scan
		| 0 -> Single conversion mode
		| 1 -> Continuous conversion mode

	* Overrun management mode : OVRMOD
		=> if old data not yet read when new data is coming
		| 0 -> Preserve old data
		| 1 -> overwrite new data

	* Data alignment : ALIGN
		| 0 -> Right alignment
		| 1 -> Left alignment

	* Data resolution : RES[1:0]
		=> decide the resolution of the conversion
		=> With lower resolution, the conversion speed is faster
		| 00 -> 12 bits
		| 01 -> 10 bits
		| 10 ->  8 bits
		| 11 ->  6 bits

	* Scan sequence direction : SCANDIR
		| 0 -> Upward scan ( from CHSEL0 to CHSEL18 )
		| 1 -> Backward scan ( from CHSEL18 to CHSEL0 )

	* Direct memory access configuration : DMACFG
		=> it is effective only when DMAEN = 1
		| 0 -> DMA one shot mode selected
		| 1 -> DMA circular mode selected

	* Direct memory access enable : DMAEN
		=> DMA is better than interrupts against ADC module
		| 0 -> DMA disabled
		| 1 -> DMA enabled

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|AWDCH[4:0]|||||x|x|AWDEN|AWDSGL|x|x|x|x|x|DISCEN|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|AUTOFF|WAIT|CONT|OVRMOD|EXTEN[1:0]||x|EXTSEL[2:0]|||ALIGN|RES[1:0]||SCANDIR|DMACFG|DMAEN|

---
4. ADC configuration register 2 ( ADC1->CFGR2 ) 32bits

	* ADC clock mode : CKMODE[1:0]
		| 00 -> HSI14
		| 01 -> PCLK/2
		| 10 -> PCLK/4

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|CKMODE[1:0]||x|x|x|x|x|x|x|x|x|x|x|x|x|x|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|

---
5. ADC sampling time register ( ADC1->SMPR ) 32bits

	* Sampling time selection : SMP[2:0]
		=> select the sampling time that applies to all channels
		| 000 ->   1.5 ADC clock cycles
		| 001 ->   7.5 ADC clock cycles
		| 010 ->  13.5 ADC clock cycles
		| 011 ->  28.5 ADC clock cycles
		| 100 ->  41.5 ADC clock cycles
		| 101 ->  55.5 ADC clock cycles
		| 110 ->  71.5 ADC clock cycles
		| 111 -> 239.5 ADC clock cycles

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|x|

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|SMP[2:0]|||

---
6. ADC channel selection register (ADC1->CHSELR)

	* Channel-x selection : CHSELx
		=> define the channels of the sequence which will be converted
		| 0 -> not selected
		| 1 -> selected

-tx-
|B31|B30|B29|B28|B27|B26|B25|B24|B23|B22|B21|B20|B19|B18|B17|B16|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|x|x|x|x|x|x|x|x|x|x|x|x|x|CHSEL[18:16]|||

-tx-
|B15|B14|B13|B12|B11|B10|B9|B8|B7|B6|B5|B4|B3|B2|B1|B0|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|CHSEL[15:0]||||||||||||||||

---

#### C language example

1. ADC1 + DMA
	```C
	/* 
	- ADC1 is connected to DMA channel 1 ( see datasheet about DMA )
	- Consider DMA.md for details on the DMA code section
	*/

	// ! must be checked in datasheet
	#define ADC_Ch0_Pin GPIO_Pin_0 // PB0
	#define ADC_Ch1_Pin GPIO_Pin_1 // PB1
	#define ADC_Pins    ( ADC_Ch0_Pin | ADC_Ch1_Pin )

	#define NUMBER_OF_ADC_CHANNEL 2
	static uint32_t ADCConvertChannels[NUMBER_OF_ADC_CHANNEL] = {0};

	void Init_Configure()
	{

	/* setting clock & GPIOx first */

		RCC->APB2ENR |= RCC_APB2ENR_ADCEN;
		RCC->AHBENR |= RCC_AHBENR_GPIOBEN;
		GPIOB->MODER |= ( GPIO_Mode_AN << ( ADC_Pins * 2 ) );

	/* calibrate ADC1 before use it */

		if( ADC1->CR & ADC_CR_ADSTART ) {
			ADC1->CR |= ADC_CR_ADSTP;
			while( ADC1->CR & ADC_CR_ADSTP );
		}

		if( ADC1->CR & ADC_CR_ADEN ) {
			ADC1->CR |= ADC_CR_ADDIS;
			while( ADC1->CR & ADC_CR_ADDIS );
		}

		ADC1->CR |= ADC_CR_ADCAL;
		while( ADC1->CR & ADC_CR_ADCAL );

		uint32_t temp = ADC1->DR;

	/* configure to ADC1 registers */

		ADC1->CR |= ADC_CR_ADEN;
		while( !( ADC1->ISR & ADC_ISR_ADRDY ) );

		ADC1->CFGR1 &= ~( ADC_CFGR1_RES );
		ADC1->CFGR1 |= ( 
			ADC_CFGR1_CONT     |
			ADC_CFGR1_OVRMOD   |
			((uint32_t)2 << 3) |
			ADC_CFGR1_DMACFG   |  // Configure about DMA
			ADC_CFGR1_DMAEN       // Enable DMA

		);
		ADC1->CFGR1 &= ~( 
			ADC_CFGR1_DISCEN  |
			ADC_CFGR1_ALIGN   |
			ADC_CFGR1_SCANDIR
		);

		ADC1->CFGR2 &= ~( ADC_CFGR2_CKMODE );
		ADC1->CFGR2 |= ((uint32_t)1 << 30); // ! ADC clock must <= 14MHz

		ADC1->SMPR &= ~( ADC_SMPR_SMP );
		ADC1->SMPR |= ((uint32_t)5);
		
		ADC1->CHSELR = ( ADC_CHSELR_CHSEL0 | ADC_CHSELR_CHSEL1 );


	/* set up DMA about ADC1 configurations */
	// ? see dma.c for details about DMA

		// Enable the peripheral clock on DMA
		RCC->AHBENR |= RCC_AHBENR_DMA1EN;

		// Reset interrupt pending bits for DMA1 Channel1
		DMA1->IFCR |= ( 
			DMA_ISR_GIF1  |
			DMA_ISR_TCIF1 |
			DMA_ISR_HTIF1 |
			DMA_ISR_TEIF1 
		);

		// Configure the peripheral data register address
		DMA1_Channel1->CPAR = (uint32_t)(SourceAddress);

		// Configure the memory address
		DMA1_Channel1->CMAR = (uint32_t)(ADCConvertChannels);

		// Configure the number of DMA tranfer
		// to be performs on DMA channel 1
		DMA1_Channel1->CNDTR = NUMBER_OF_ADC_CHANNEL;

		// clear all configurations
		DMA1_Channel1->CCR &= ~((uint32_t)0);
		DMA1_Channel1->CCR |= (
			( 3 << 12 )  | // Very high priority level
			( 2 << 10 )  | // Destination memory size 32bits
			( 2 << 8 )   | // Peripheral size 32bits
			DMA_CCR_MINC | // Destination address auto increment
			DMA_CCR_CIRC   // Circular mode
		);

		// Enable DMA Channel 1
		DMA1_Channel1->CCR |= DMA_CCR_EN;

	/* GO! */
		// ADC1->CR |= ADC_CR_ADSTART;
	}
	```
