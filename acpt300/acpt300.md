# MODBUS Communication Protocol

## 1. Summary

This protocol complies with the MODBUS communication protocol. It adopts the subset RTU mode of the MODBUS protocol and uses the RS485 half-duplex mode.

## 2. Serial Data Format

- **Serial Port Settings**: No parity, 8 data bits, 1 stop bit.
- **Example**: `9600, N, 8, 1` meaning: 9600bps, no parity, 8 data bits, 1 stop bit.
- **Supported Baud Rates**: 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200
- **CRC Check Polynomial**: `0xA001`
- **Data Processing**: Data in communication is processed as double-byte shaping data. If identified as a floating point, decimal points must be read to determine the data size.

## 3. Communication Format

### 3.1 Read Command Format (03 Function Code) Example:

**A. Send Read Command Format**:
- Example: `0X01 0X03 0X00 0X00 0X00 0X01 0X84 0X0A`

| Field       | Value |
|-------------|-------|
| Address     | 0X01  |
| Function    | 0X03  |
| Data Start (H) | 0X00  |
| Data Start (L) | 0X00  |
| Data Nos. (H)  | 0X00  |
| Data Nos. (L)  | 0X01  |
| CRC16 (L)   | 0X84  |
| CRC16 (H)   | 0X0A  |

**B. Return Read Format**:
- Example: `0X01 0X03 0X02 0X00 0X01 0X79 0X84`

| Field       | Value |
|-------------|-------|
| Address     | 0X01  |
| Function    | 0X03  |
| Data Length | 0X02  |
| Data (H)    | 0X00  |
| Data (L)    | 0X01  |
| CRC16 (L)   | 0X79  |
| CRC16 (H)   | 0X84  |

### 3.2 Write Command Format (06 Function Code) Example:

**A. Send Write Command Format**:
- Example: `0X01 0X06 0X00 0X00 0X00 0X02 0X08 0X0B`

| Field       | Value |
|-------------|-------|
| Address     | 0X01  |
| Function    | 0X06  |
| Data Start (H) | 0X00  |
| Data Start (L) | 0X00  |
| Data (H)    | 0X00  |
| Data (L)    | 0X02  |
| CRC16 (L)   | 0X08  |
| CRC16 (H)   | 0X0B  |

**B. Return Write Format**:
- Same as send format.

### 3.3 Abnormal Response Return

- Example: `0X01 0X80+ Function Code 0x01 (Illegal Function) 0x02 (Illegal Data Address) 0x03 (Illegal Data Value)`

## 4. Supported Commands and Data Meanings

The MODBUS-RTU protocol command list:

| Function Code | Data Start Address | Data Nos. | Data Bytes | Data Range     | Command Meaning                   |
|---------------|--------------------|-----------|------------|---------------|-----------------------------------|
| 0x03          | 0x0000             | 1         | 2          | 1-255         | Read slave address                |
| 0x03          | 0x0001             | 1         | 2          | 0-115200      | Read baud rate                    |
| 0x03          | 0x0003             | 1         | 2          | 0-####        | Decimal point, 0-3 decimal places |
| 0x03          | 0x0002             | 1         | 2          | 0-Mpa, Kpa, Pa, Bar, etc. | Read pressure units               |
| 0x03          | 0x0004             | 1         | 2          | -32768-32767  | Measured output value             |
| 0x03          | 0x0005             | 1         | 2          | -32768-32767  | Transmitter zero point            |
| 0x03          | 0x0006             | 1         | 2          | -32768-32767  | Transmitter extreme point         |
| 0x03          | 0x000c             | 1         | 2          | -32768-32767  | Zero offset value                 |
| 0x06          | 0x0000             | 2         | 1-255      | Rewrite slave address             |
| 0x06          | 0x0001             | 2         | 0-115200   | Revise baud rate                  |
| 0x06          | 0x000c             | 2         | -32768-32767 | Zero offset value               |
| 0x06          | 0x000F             | 2         | 0-1        | Save to user/factory area         |
| 0x06          | 0x0010             | 2         | 1          | Restore factory parameters        |

## 5. Notes

1. **Modifying Baud Rate**: After modifying, the transmitter will reply at the old baud rate, then switch to the new baud rate.
2. **Modifying Address**: The address is returned as the old value, then the transmitter automatically updates to the new address.
3. **Save & Restore Commands**: Returns the original value, indicating the command was accepted.
4. **Restoring Factory Data**: The parameters may differ from user-saved data, so search for the transmitter again after restoring.
5. **Modifiable Data**: Only addresses, baud rates, and zero offset values are user-modifiable.
6. **Calibration**: General users should not modify calibration data. Contact the company for calibration software if needed.
7. **Floating Point Data**: Data is stored as integers; divide by the appropriate power of ten to get the floating-point number.

