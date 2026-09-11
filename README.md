# 🌉 ESP32 UART Bridge

A lightning-fast, transparent serial bridge for the ESP32. This project turns your ESP32 into a full-duplex USB-to-Serial adapter, using FreeRTOS to pin separate transmit and receive tasks to different CPU cores for maximum, non-blocking throughput.

## 🔌 Wiring

| ESP32 Pin | Connection |
| :--- | :--- |
| **GPIO 16** (RX) | Target Device **TX** |
| **GPIO 17** (TX) | Target Device **RX** |
| **GND** | Target Device **GND** |

> **⚠️ 3.3V Logic Warning:** The ESP32 is strictly 3.3V. If your target device operates at 5V, use a logic level shifter or a voltage divider on the ESP32's RX pin to prevent permanent chip damage.

---

## 🚀 Installation

Simply download and upload **v1** of the source code to your ESP32 using the Arduino IDE (Core 2.x / 3.x).

---

## 💡 Tips & Variations

*   **Bridge two external devices (No USB):** Swap `Serial` for a second `HardwareSerial` instance on different pins.
*   **Asymmetrical Speeds:** The bridge is just relaying bytes. You can pass completely different baud rates to the UART initializations if your devices run at different speeds.
*   **Hardware Flow Control:** Need RTS/CTS? Pass the extra parameters natively directly into `UartB.begin()`.
*   **Need more speed?** At 115200 baud, this uses ~2% CPU. For >1 Mbaud, increase the buffer sizes and bump the baud rates on both sides.

## 🛑 Common Gotchas

*   **Forbidden Pins:** Never use UART1's default pins (**GPIO 9 & 10**) on a classic ESP32. These are tied to the internal SPI flash.
*   **Shhh! No Debugging:** Do not use `Serial.print("Hello")` in this sketch. It will inject your text directly into the target device's data stream.
*   **Baud Matching:** On standard boards (CP2102/CH340), your PC terminal must match the bridge's baud rate. (On ESP32-S2/S3 with native USB, the PC baud rate is ignored).
