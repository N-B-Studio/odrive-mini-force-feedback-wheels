# odrive-mini-force-feedback-wheels

## Overview
This project shows how to build a custom force-feedback wheel using an MKS ODrive Mini v1.0, a BLDC motor, and an OpenFFBoard-compatible F407 controller. The instructions below cover hardware, firmware, GUI tools, and configuration.
<img width="2847" height="1613" alt="image" src="https://github.com/user-attachments/assets/ce981abf-3224-47fe-a21f-5df691fa7e76" />

## Required Components
- 5056 140kv BLDC motor
- MKS ODrive Mini v1.0
- F407VGxx development board
- WCMCU-2551 CAN module
- 3D printed steering wheel shell
  - https://makerworld.com/en/models/1313111-g29-g920-g923-logitech-mod-steering-wheel?from=search#profileId-1347733b

## Step 1: Assemble the Hardware
1. Assemble the hardware components according to the image below:
    <img width="2744" height="1380" alt="image" src="https://github.com/user-attachments/assets/3c15b8d9-0c66-4ff3-850e-58b2afa79467" />
2. Wire the F407 and ODrive based on the wiring guide image:
    <img width="620" height="629" alt="wiring-guide" src="https://github.com/user-attachments/assets/2d25693a-dcdb-48b3-aa9c-c1dd870f200c" />

## Step 2: Setup ODrive
1. Download the `MKS_ODrive_MINI_0_5_1_20250326.hex` firmware file.
2. Flash the firmware using your preferred STM32 programmer/tool (e.g., STM32CubeProgrammer).
3. Power the ODrive Mini with a 4S or 6S LiPo battery (nominal 14.8V or 22.2V respectively).
4. Use `odrivetool` to configure the ODrive:
   - Connect via USB to your computer and launch `odrivetool` in the terminal
   - Set motor type: `odrv0.axis0.motor.config.motor_type = MotorType.GIMBAL` (or `HIGH_CURRENT` as needed)
   - **Configure SPI encoder (AS5047P):**
     - Set `odrv0.axis0.encoder.config.mode = ENCODER_MODE_SPI`
     - Set CS pin to GPIO7: `odrv0.axis0.encoder.config.spi_cs_pin_gpio = 7`
     - Set `odrv0.axis0.encoder.config.calib_scan_distance = 50` (encoder calibration distance)
   - **Calibrate motor and encoder:**
     - Run `odrv0.axis0.requested_state = AXIS_STATE_MOTOR_CALIBRATION` and wait
     - Run `odrv0.axis0.requested_state = AXIS_STATE_ENCODER_OFFSET_CALIBRATION` and wait
   - **Configure current limits** (5056 140kv motor: 20-30A continuous):
     - Set `odrv0.axis0.motor.config.current_lim = 10` (start conservative, increase after testing)
     - Set `odrv0.axis0.motor.config.current_lim_margin = 8`
   - **Enable CAN communication:**
     - Set `odrv0.can.config.baud_rate = 500000` (typical CAN baud rate)
     - Configure node ID if using CAN communication with F407
   - Save configuration: `odrv0.save_configuration()`
5. Reboot the ODrive and verify USB connection

## Step 3: Setup OpenFFBoard
1. Install the OpenFFBoard Configurator from the downloaded Windows package.
2. Launch the configurator and connect to the F407 board.
3. Load or create a profile for your wheel setup.
4. Configure motor parameters, steering limits, and sensor inputs.
5. Set the CAN/BLE/USB communication mode if required.
6. Save the configuration and reboot the board.
7. Test wheel movement and force feedback gradually at low torque settings.
8. Adjust damping, friction, and torque curves until the wheel behaves smoothly.

## Configuration Tips
- **Battery**: Use a 4S (nominal 14.8V) or 6S (nominal 22.2V) LiPo battery depending on your motor specifications.
- **Encoder**: The AS5047P SPI encoder requires a GPIO pin (e.g., GPIO7) for chip select. Ensure SPI clock and MISO/MOSI lines are properly connected.
- **Motor Calibration**: Calibration sequences must complete successfully before operation. The motor should respond smoothly during calibration.
- **Current Limits**: The 5056 140kv motor typically handles 20-30A continuous. Start at 10A and gradually increase after testing.
- **CAN Communication**: Verify CAN bus termination resistors (120Ω) are in place if using CAN with the F407 board.

## Safety Notes
- Start with low current limits and work up gradually to avoid hardware damage.
- Always test electrical connections before applying power to the system.
- Ensure the motor and wheel are mounted securely before enabling force feedback.
- Never apply excessive torque commands during initial testing; begin with small test commands.
- Keep emergency stop procedures in place during testing and operation.

## Reference
- OpenFFBoard F407 firmware
  - https://github.com/Ultrawipf/OpenFFBoard/releases/download/v1.17.0/Firmware.zip
- MKS ODrive Mini v1.0 firmware
  - https://github.com/makerbase-motor/MKS-ODrive/blob/master/Firmware/MKS%20ODrive%20MINI/MKS_ODrive_MINI_0_5_1_20250326.hex
- OpenFFBoard Configurator (Windows, Python 3.12)
  - https://github.com/Ultrawipf/OpenFFBoard/releases/download/v1.17.0/OpenFFBoard-Configurator-windows-latest-py3.12.zip

## Useful Links
- OpenFFBoard releases: https://github.com/Ultrawipf/OpenFFBoard/releases
- MKS ODrive Mini firmware: https://github.com/makerbase-motor/MKS-ODrive
