# I2C bus lockup after brownout
Date: 2026-09-25
Board / FW version: rev B, fw 1.4.2
Symptom: After brownout, all I2C transfers return -EIO. Only a full power cycle fixes it.
Root cause: MCU resets mid-transaction, sensor still holds SDA low waiting for clocks. Bus stuck.
Fix: At boot, if SDA is low, toggle SCL 9 times as GPIO, then send STOP. Zephyr: i2c_recover_bus().
How to verify: Brownout with bench PSU (drop to 2.5V for 50ms) during sensor read, 20 times. No lockup.
