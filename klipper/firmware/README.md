# Flashforge Creator Pro Klipper firmware

Generated for **Atmel atmega2560**.

Use the following command from the Klipper directory to flash the board:

  ```
  avrdude -v -c stk500v2 -p atmega2560 -P {your PRINTER ID from running ls /dev/serial/by-id/*} -b 57600 -D -U out/klipper.elf.hex
  ```

For example, for my configuration:

  ```
  avrdude -v -c stk500v2 -p atmega2560 -P /dev/serial/by-id/usb-MakerBot_Industries_The_Replicator_85830303639351610272-if00 -b 57600 -D -U out/klipper.elf.hex
  ```

