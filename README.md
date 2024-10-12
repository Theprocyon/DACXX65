# DACXX65

- C library for TI DACXX65 Series

## Supported Models
- DAC8565
- DAC8165
- DAC7565

## How to use

1. Declare a `DACXX65_t` structure.

```c
DACXX65_t dac;
```

2. Call the initialization function `DACXX65_Init`.

```c
void DACXX65_Init(DACXX65_t *dac, DACXX65_BIT_t bit, void (*fp_spi_tx)(uint32_t), void (*fp_sync_enable)(bool));
```

### Important Notes for Initialization

- **SPI Transmission**: This library implements 24-bit SPI transmission using function pointers. Dependency injection is used to pass the function for SPI transmission, allowing flexibility in how SPI communication is handled in your specific hardware setup. The SPI transmission function (`fp_spi_tx`) **must be blocking**

- **SYNC Pin (Chip Select)**: The SPI CS (also known as SYNC) pin control is handled through another function pointer. If your hardware manages the CS/SYNC pin automatically, you can simply pass `NULL` or `0` to disable manual control of the SYNC pin in software.

4. Write data to a DAC channel.

```c
DACXX65_WriteChannel(&dac, DACXX65_CHANNEL_A, 0x0FFF);
```

The `DACXX65_WriteChannel` function writes the specified value to the given DAC channel. Channels include `DACXX65_CHANNEL_A`, `DACXX65_CHANNEL_B`, `DACXX65_CHANNEL_C`, `DACXX65_CHANNEL_D`, and `DACXX65_CHANNEL_ALL` for broadcasting to all channels.

## Example Code

```c
#include "dacxx65.h"

DACXX65_t dac;

void spi_send(uint32_t data) {
    // Blocking Write 24byte SPI (Must Be Bo)
}

void sync_control(bool enable) {
    if (enable) {
        // Enable SYNC (CS Low)
    } else {
        // Disable SYNC (CS High)
    }
}

int main(void) {
    // Initialize DAC with 16-bit resolution and assign function pointers
    DACXX65_Init(&dac, DACXX65_16BIT, spi_send, sync_control);

    return 0;
}
```

### Dependencies
- `stdint.h`, `stdbool.h`, `stddef.h` for standard data types and boolean support.

## License

MIT License. Feel free to use, modify, and distribute this library in your own projects.