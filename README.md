# STM32 Ethernet Test (LwIP)

<<<<<<< HEAD
This project is a cleaned and improved STM32 Ethernet test project for **NUCLEO-H755ZI-Q / STM32H755ZITx** using **LAN8742 PHY**, **RMII**, and **LwIP NO_SYS polling mode**.

## Hardware

- STM32 NUCLEO-H755ZI-Q
- LAN8742 Ethernet PHY
- PC connected on the same subnet
- UART COM1 / USART3 for debug logs

## Network Configuration

| Parameter | Value |
|---|---|
| STM32 IP | `192.168.1.50` |
| Netmask | `255.255.255.0` |
| Gateway | `192.168.1.1` |
| Mode | Static IP, DHCP disabled |

## Main Improvements

- Ethernet DMA descriptor sections added to linker script.
- Rx pool section added to linker script.
- UART debug output added.
- Periodic network status logging added.
- `HAL_ETH_Transmit()` error return is now checked.
- Cache maintenance helper functions added for Ethernet Rx/Tx buffers.
- `.git` and build output folders removed from the distributable project package.

## System Architecture

```text
PC / PLC
   │
   │ Ethernet
   ▼
LAN8742 PHY
   │ RMII
   ▼
STM32H755 Nucleo
   │
   ├── LwIP stack
   ├── Ethernet driver
   └── Application diagnostics
```

## Expected UART Output

```text
========================================
 STM32H755 Ethernet Test - LwIP NO_SYS
 Board : NUCLEO-H755ZI-Q
 PHY   : LAN8742 / RMII
 UART  : COM1 115200 8N1
========================================
LINK DEGISTI -> UP
NETIF: UP | LINK: UP | IP: 192.168.1.50
```

## Test Steps

1. Open the project with STM32CubeIDE.
2. Build **CM7** project.
3. Flash/debug the board.
4. Open UART serial monitor at `115200 8N1`.
5. Connect Ethernet cable.
6. On PC, set same subnet, for example:
   - PC IP: `192.168.1.78`
   - Mask: `255.255.255.0`
7. Test from PC:

```powershell
ping 192.168.1.50
arp -a
```

## Notes

- This project uses polling mode: `MX_LWIP_Process()` is called in the main loop.
- DHCP is disabled.
- Descriptor and Rx pool placement in D2 SRAM is critical on STM32H7 Ethernet projects.
=======
This project focuses on Ethernet communication using STM32 (Nucleo H755) with LwIP stack.

---

## 🚀 Features
- LwIP integration
- Ethernet communication test
- FreeRTOS support
- STM32CubeIDE project

---

## 🔧 Hardware
- STM32H755 Nucleo
- LAN8742 Ethernet PHY

---

## 🧠 Software
- STM32CubeIDE
- LwIP
- FreeRTOS

---

## 📡 Goal
To establish stable Ethernet communication between STM32 and PC / PLC systems.

---

## 📊 Status
🟡 In development
>>>>>>> 90980377dda4f5852561a96a4cfc99932070c48d
