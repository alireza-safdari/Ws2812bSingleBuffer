# Ws2812bSingleBuffer

This library is based on https://github.com/rogerclarkmelbourne/WS2812B_STM32_Libmaple however it uses a single buffer allowing Blue Pill and Black Pill to operate with 1024 LEDs. Regardless of number of LED's used this library use half as much memory which can be beneficial. Also, since the memory allocation is done on the heap (dynamically), it may be difficult to know if you have run out of memory. So, this library would be handy for cases where you are close to full memory utilization.

This library in the same fashion as WS2812B_STM32_Libmaple uses DMA and SPI. For connections you may refer to do the original library.

# How is the memory footprint reduced by half?
The original library uses 2 buffers, one for storing the new frame and one for transmitting the old frame, the buffers are ping pong buffers. While this may be necessary for slower systems, STM32 with DMA and SPI is fast enough to transmit the frame quickly and is fast enough to render the frame before you notice any issue on the LEDs. Experiencing this firsthand, I decided to use a single buffer to improve the memory footprint in a larger project. The only side effect is that now you must time your renders to start after you are certain that the transmission of the old frame is complete. This is simply done by storing a time frame for when you call function show. Alternatively, if your code runs in an almost fixed timed loop, you can simply count the loop iterations to know when it is time to render safely.

# Future work
1)	Adding an example
2)	Adding a mechanism to handle the timing internally, so the user would just check with the library if rendering is allowed
