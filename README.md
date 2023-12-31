# TLC6C598 Shift register / LED driver breakout board

The correct timing can be achieved with SPI polarity=0, phase=0. In CubeMX,
this corresponds to CPOL=Low, CPHA=1 edge.

/CLS can be tied high.

![Timing Diagram](timing.png)

After shifting in data, you have to manually raise and lower a GPIO to act as a
register clock to cause the TLC to transfer that data to the storage registers
for display, which is done on the rising edge of RCK.

Since the high time of the register clock is the same as the serial clock, you
can achieve this by pulling the clock GPIO low before transmitting data and
then pulling it high when you're done.

```
	// Register clock low.
	HAL_GPIO_WritePin(GPIOA, GPIO_RCK, GPIO_PIN_RESET);
	
	// Shift data ...
	HAL_SPI_Transmit(&hspi1, (uint8_t*) data, 1, 1);

	// Register clock high: See the blinkies.
	HAL_GPIO_WritePin(GPIOA, GPIO_RCK, GPIO_PIN_SET);
```

![PCB Image](pcb.png)
