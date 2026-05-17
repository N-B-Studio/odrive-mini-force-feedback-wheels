# odrive-mini-force-feedback-wheels

## Overview
This project shows how to build a custom force-feedback wheel using an MKS ODrive Mini v1.0, a BLDC motor, and an OpenFFBoard-compatible F407 controller. The instructions below cover hardware, firmware, GUI tools, and configuration.


## Required Components
- 5056 140kv BLDC motor
- MKS ODrive Mini v1.0
- F407VGxx development board
- WCMCU-2551 CAN module
- 3D printed steering wheel shell
  - https://makerworld.com/en/models/1313111-g29-g920-g923-logitech-mod-steering-wheel?from=search#profileId-1347733b

## Firmware and Tools
- OpenFFBoard F407 firmware
  - https://github.com/Ultrawipf/OpenFFBoard/releases/download/v1.17.0/Firmware.zip
- MKS ODrive Mini v1.0 firmware
  - https://github.com/makerbase-motor/MKS-ODrive/blob/master/Firmware/MKS%20ODrive%20MINI/MKS_ODrive_MINI_0_5_1_20250326.hex
- OpenFFBoard Configurator (Windows, Python 3.12)
  - https://github.com/Ultrawipf/OpenFFBoard/releases/download/v1.17.0/OpenFFBoard-Configurator-windows-latest-py3.12.zip

## Step 1: Assemble the Hardware
1. Mount the 5056 140kv BLDC motor securely inside the wheel base.
<img width="2744" height="1380" alt="image" src="https://github.com/user-attachments/assets/3c15b8d9-0c66-4ff3-850e-58b2afa79467" />
2. Attach the motor shaft to the steering wheel hub using a compatible coupler.
3. Connect the motor phase wires to the MKS ODrive Mini v1.0 motor outputs.
4. Connect the ODrive Mini power input to your battery or PSU, respecting voltage and current limits.
5. Wire the F407VGxx dev board and the WCMCU-2551 CAN module together if using CAN communication.
<img width="620" height="629" alt="wiring-guide" src="https://github.com/user-attachments/assets/2d25693a-dcdb-48b3-aa9c-c1dd870f200c" />
6. Connect the F407 board to the wheel controls, pedals, and any sensors you are using.
7. Confirm all connectors are secure and all grounds are common.
<img width="3066" height="1657" alt="image" src="https://github.com/user-attachments/assets/b235c63d-2e9f-42a3-b04c-4ed393c17c79" />

## Step 2: Flash Firmware
1. Download and unzip the OpenFFBoard F407 firmware package.
2. Put the F407 board into bootloader mode per the board instructions.
3. Flash the firmware from the downloaded `Firmware.zip` using your preferred STM32 programmer/tool.
4. Download the ODrive Mini v1.0 firmware file and flash it to the ODrive Mini using the appropriate ODrive firmware tool or built-in recovery method.
5. Reboot both the F407 board and the ODrive Mini after flashing.
6. Verify each board connects successfully to your PC and is recognized by its configuration tool.

## Step 3: Configure the System
1. Install the OpenFFBoard Configurator from the downloaded Windows package.
2. Launch the configurator and connect to the F407 board.
3. Load or create a profile for your wheel setup.
4. Configure motor parameters, steering limits, and sensor inputs.
5. Set the CAN/BLE/USB communication mode if required.
6. Save the configuration and reboot the board.
7. Test wheel movement and force feedback gradually at low torque settings.
8. Adjust damping, friction, and torque curves until the wheel behaves smoothly.

## Notes
- Start with low current limits and work up slowly to avoid hardware damage.
- Always test electrical connections before powering the system.
- Ensure the motor and wheel are fixed securely before applying force feedback.

## Useful Links
- OpenFFBoard releases: https://github.com/Ultrawipf/OpenFFBoard/releases
- MKS ODrive Mini firmware: https://github.com/makerbase-motor/MKS-ODrive

