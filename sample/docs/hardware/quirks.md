# Hardware quirks (not in the datasheet)

- **PCA9685** runs about 3% fast on our boards. Calibrate prescaler per board, don't trust 25MHz.
- **Rev B board**: TP12 is 1V8, not 3V3 as on the silkscreen.
- **Sensor XYZ**: needs 10ms after power-on before first I2C read, datasheet says 1ms.
