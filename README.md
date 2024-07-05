# Zest_Core_nRF5340

Zest_Core_nRF52840 custom target for Zephyr OS.

The nRF5340 is a dual-core SoC based on the Arm® Cortex®-M33 architecture, with:

* a full-featured Arm Cortex-M33F core with DSP instructions, FPU, and
  Armv8-M Security Extension, running at up to 128 MHz, referred to as
  the **application core**
* a secondary Arm Cortex-M33 core, with a reduced feature set, running at
  a fixed 64 MHz, referred to as the **network core**.

The `zest_core_nrf5340_nrf5340_cpuapp` build target provides support for the application
core on the nRF5340 SoC. The `zest_core_nrf5340_nrf5340_cpunet` build target provides
support for the network core on the nRF5340 SoC.

## Supported Features

The Zephyr `zest_core_nrf5340_nrf5340_cpuapp` board configuration supports the following hardware features:

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
| RTC       | on-chip                                                                                      | system-clock     |
| RTT       | on-chip                                                                                      | console          |
| SPI(M/S)  | on-chip                                                                                      | spi              |
| SPU       | on-chip                                                                                      | system protection|
| UART      | on-chip                                                                                      | serial           |
| USB       | on-chip                                                                                      | usb              |
| WDT       | on-chip                                                                                      | watchdog         |

The `zest_core_nrf5340_nrf5340_cpunet` board configuration supports the following hardware features:

| Interface | Controller                                                                                   | Driver/Component |
| :-------- | :------------------------------------------------------------------------------------------- | :--------------- |
| CLOCK     | on-chip                                                                                      | clock_control    |
| FLASH     | on-chip                                                                                      | flash            |
| GPIO      | on-chip                                                                                      | gpio             |
| I2C(M)    | on-chip                                                                                      | i2c              |
| MPU       | on-chip                                                                                      | arch/arm         |
| NVIC      | on-chip                                                                                      | arch/arm         |
| RADIO     | on-chip                                                                                      | Bluetooth        |
| RADIO     | on-chip                                                                                      | IEEE 802.15.4    |
| RTC       | on-chip                                                                                      | system-clock     |
| SPI(M/S)  | on-chip                                                                                      | spi              |
| UARTE     | on-chip                                                                                      | serial           |
| WDT       | on-chip                                                                                      | watchdog         |

## Usage

1. Add `Zest_Core_nRF52340` board to your workspace using the [6TRON module](https://github.com/catie-aq/zephyr_6tron-manifest.git)
2. Compile and flash application using west tool
   - Board name: `zest_core_nrf5340_nrf5340_cpuapp` or `zest_core_nrf5340_nrf5340_cpuapp_ns` or `zest_core_nrf5340_nrf5340_cpunet`

## Building Secure/Non-Secure Zephyr applications with Arm® TrustZone®

Applications on the nRF5340 may contain a Secure and a Non-Secure firmware image for the application core. The Secure image can be built using either Zephyr or [Trusted Firmware M](https://www.trustedfirmware.org/) (TF-M). Non-Secure firmware images are always built using Zephyr. The two alternatives are described below.

> **Note:** By default the Secure image for nRF5340 application core is built using TF-M.

### Building the Secure firmware with TF-M

The process to build the Secure firmware image using TF-M and the Non-Secure firmware image using Zephyr requires the following steps:

1. Build the Non-Secure Zephyr application for the application core using `-DBOARD=zest_core_nrf5340_nrf5340_cpuapp_ns`. To invoke the building of TF-M the Zephyr build system requires the Kconfig option `BUILD_WITH_TFM` to be enabled, which is done by default when building Zephyr as a Non-Secure application. The Zephyr build system will perform the following steps automatically:
    * Build the Non-Secure firmware image as a regular Zephyr application
    * Build a TF-M (secure) firmware image
    * Merge the output image binaries together
    * Optionally build a bootloader image (MCUboot)

> **Note:** Depending on the TF-M configuration, an application DTS overlay may be required, to adjust the Non-Secure image Flash and SRAM starting address and sizes.

2. Build the application firmware for the network core using `-DBOARD=zest_core_nrf5340_nrf5340_cpunet`.

### Building the Secure firmware using Zephyr

The process to build the Secure and the Non-Secure firmware images using Zephyr requires the following steps:

1. Build the Secure Zephyr application for the application core using `-DBOARD=zest_core_nrf5340_nrf5340_cpuapp` and `CONFIG_TRUSTED_EXECUTION_SECURE=y` and `CONFIG_BUILD_WITH_TFM=n` in the application project configuration file.
2. Build the Non-Secure Zephyr application for the application core using `-DBOARD=zest_core_nrf5340_nrf5340_cpuapp_ns`.
3. Merge the two binaries together.
4. Build the application firmware for the network core using `-DBOARD=zest_core_nrf5340_nrf5340_cpunet`.

When building a Secure/Non-Secure application for the nRF5340 application core, the Secure application will have to set the IDAU (SPU) configuration to allow Non-Secure access to all CPU resources utilized by the Non-Secure application firmware. SPU configuration shall take place before jumping to the Non-Secure application.

## Building a Secure only application

Build the Zephyr app in the usual way (see [build_an_application](https://docs.zephyrproject.org/latest/guides/build/index.html) and [application_run](https://docs.zephyrproject.org/latest/guides/build/run-app.html)), using `-DBOARD=zest_core_nrf5340_nrf5340_cpuapp` for the firmware running on the nRF5340 application core, and using `-DBOARD=zest_core_nrf5340_nrf5340_cpunet` for the firmware running on the nRF5340 network core.
