# STM32 Ethernet Project Analysis and Applied Changes

## 1. Critical Finding: Ethernet DMA Memory Placement

The original linker script did not explicitly place the following Ethernet sections:

- `.RxDescripSection`
- `.TxDescripSection`
- `.Rx_PoolSection`

For STM32H7 Ethernet, DMA descriptors and Rx buffers must be placed in DMA-accessible RAM, typically D2 SRAM at `0x30000000`.

Applied change in `CM7/STM32H755ZITX_FLASH.ld` and `CM7/STM32H755ZITX_RAM.ld`:

```ld
RAM_D2 (xrw) : ORIGIN = 0x30000000, LENGTH = 288K
```

and dedicated Ethernet sections were added at:

```text
Rx descriptors : 0x30000000
Tx descriptors : 0x30000080
Rx pool         : 0x30000100
```

## 2. UART Debug Output

The original project initialized LwIP before COM/UART debug output. This made early Ethernet diagnostics difficult.

Applied changes:

- COM1 initialization moved before `MX_LWIP_Init()`.
- `printf()` retargeting added through `__io_putchar()`.
- Startup banner added.
- Periodic network status logging added.

## 3. Ethernet TX Error Handling

The original code called:

```c
HAL_ETH_Transmit(&heth, &TxConfig, ETH_DMA_TRANSMIT_TIMEOUT);
```

but did not check the return value.

Applied change:

```c
if (HAL_ETH_Transmit(&heth, &TxConfig, ETH_DMA_TRANSMIT_TIMEOUT) != HAL_OK)
{
  errval = ERR_IF;
}
```

## 4. Cache Maintenance Helpers

Cache clean/invalidate helper functions were added in `ethernetif.c`.

This is important when D-Cache is enabled later. Even if cache is currently disabled, this makes the Ethernet path safer for future performance tuning.

## 5. Project Hygiene

The provided improved ZIP excludes:

- `.git/`
- `Debug/`
- `Release/`
- compiled `.elf`, `.map`, `.o`, `.d` outputs

These files should not normally be shipped inside a clean source package.

## 6. Recommended Next Steps

1. Build CM7 project in STM32CubeIDE.
2. Confirm UART output.
3. Confirm link state changes when Ethernet cable is unplugged/plugged.
4. Ping `192.168.1.50` from PC.
5. After basic ping works, add Modbus TCP client/server tests.
