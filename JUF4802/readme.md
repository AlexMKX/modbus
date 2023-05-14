# JUF4802 RS 485 PWM Controller

![image](https://github.com/AlexMKX/modbus/assets/25821291/2e6b856d-71ac-47a4-b2e5-afd3b6eab85e)

| Address       | Function                                                                           | Description | Functions |
|---------------|------------------------------------------------------------------------------------|-------------|-----------|
| 0x01          | Fan status in first 4 bits 0x1 for fan1, <br/>0x2 for fan2 0x4 for fan 4 and so on | read/write  | 0x3/0x2   |
| 0x02 (0xFFFF) | Unit address                                                                       | read/write  | 0x3/0x6   |
| 0x03          | percentage of duty cycle (0-100                                                    | read/write  | 0x3/0x6   |
| 0x05          | Governor mode 0x01 - minimal speed, 0x00 turn off                                  | read/write  | 0x3/0x6   |
| 0x06          | Fans count 0x01..0x04                                                              | read/write  | 0x3/0x6   |
| 0x07..0x0A    | Fan RPM                                                                            | read        | 0x3       |
| 0x0B          | PWM Frequency 0=500 Hz, 1 = 1 KHz, 2 = 2 KHz, 3 = 5 KHz, 4 = 10 KHz, 5 = 25 KHz    | read/write  | 0x3/0x6   |
| 0x20          | Factory reset ? 0x00AA                                                             | Write       | 0x6       |

