# STM32-HAL

[![Build Status](https://github.com/david-oconnor/stm32-hal/workflows/CI/badge.svg)](https://github.com/david-oconnor/stm32-hal/actions)
[![Crate](https://img.shields.io/crates/v/stm32-hal2.svg)](https://crates.io/crates/stm32-hal2)
[![Docs](https://docs.rs/stm32-hal2/badge.svg)](https://docs.rs/stm32-hal2)


This library provides access to STM32 peripherals that are similar across
families. It's based on the 
[STM32 Peripheral Access Crates](https://github.com/stm32-rs/stm32-rs) generated by [svd2rust](https://github.com/rust-embedded/svd2rust)

It's designed to provide a consistent API across multiple families, with minimal code repetition.
This makes it easier to switch MCUs within, or across families, for a given project. 

Motivation: Use STM32s in real-world hardware projects. Be able to switch MCUs with
minimal code change. Prioritize getting hardware working with a reasonable feature set.
As of this library's creation, this is not feasible with existing HAL crates due
to lack of maintainer time, and differing priorities. When the `stm32fyxx` ecosystem
is viewed as a whole, there's a lot of DRY.

The library is heavily influence by the `stm32fyxx` HALs, and a number of the modules here are modified 
versions of those.

If using concurrently with another hal, you need to instantiate a set of
MCU peripherals for each. Ie:
```rust
let mut dp = stm32_hal::pac::Peripherals::take().unwrap();
let mut dp2 = unsafe { stm32l4xx_hal::pac::Peripherals::steal() };
```

Examples of features this crate includes that aren't present in some HALs:

    - Low power modes
    - RTC wakeup handler; RTCC trait support
    - Repeating starts on I2C/SMBUS
    - PWM features
    - Timer frequency < 1Hz
    - More predictable clock cfg
    - Read and write onboard flash
    - DAC support
    
The intent isn't to support every STM32 family: Main support will be for newer ones,
like L4, L5, H7, and U5.

Most peripheral modules are independent: The only dependency they have within the crate
is the `ClockCfg` trait, which we may move to a standalone crate later. This makes
it easy to interchange them with other projects.

Pre-release. Currently supports F3, L4, and L5, and H7.

The `syntax_overview` example provides a demonstration of how to get started.

PRs encouraged.