# Zest_Core_nRF52832

[Zest_Core_nRF52832](https://6tron.io/zest_core/zest_core_nrf52832-qfaa_1_2_0) custom target for Zephyr OS.

## Supported Features

The Zephyr Zest_Core_nRF52832 board configuration supports the following hardware features:

| Interface | Controller                                                                                   | Driver/Component |
| :-------- | :------------------------------------------------------------------------------------------- | :--------------- |
| ADC       | on-chip                                                                                      | adc              |
| CLOCK     | on-chip                                                                                      | clock_control    |
| FLASH     | on-chip                                                                                      | flash            |
| GPIO      | on-chip                                                                                      | gpio             |
| I2C(M)    | on-chip                                                                                      | i2c              |
| MPU       | on-chip                                                                                      | arch/arm         |
| NVIC      | on-chip                                                                                      | arch/arm         |
| PWM       | on-chip                                                                                      | pwm              |
| RADIO     | on-chip                                                                                      | Bluetooth        |
| RTC       | on-chip                                                                                      | system-clock     |
| RTT       | on-chip                                                                                      | console          |
| SPI(M/S)  | on-chip                                                                                      | spi              |
| UART      | on-chip                                                                                      | serial           |
| WDT       | on-chip                                                                                      | watchdog         |

## Usage

1. Add `Zest_Core_nRF52832` board to your workspace using the [6TRON module](https://github.com/catie-aq/zephyr_6tron-manifest.git)
2. Compile and flash application using west tool
   - Board name: `Zest_Core_nRF52832`
