Warning: this troubleshooting guide has not been vetted by the DieBieMS community and is subject to errors.

# USB port not visible from DBMS tool

If the CP2104 (IC5) USB transceiver chip isn't visible in DieBieMS-Tool, these are some possibilities:

- The CP2014 chip is damaged
- The computer is missing the drivers for CP2104 communication

You can bypass the CP2014 chip by using the UART port.
If you can communicate directly over the UART (using an FTDI or similar device) you can establish that the firmware and the MCU are running correctly.

# USB port visible in DBMS tool but unable to connect

When a connection attempt is made, the DBMS tool requests the firmware ID from the MCU.
If the DBMS tool doesn't get this code, the connection attempt fails.
Since the USB CP2104 chip is powered by the USB cable, it will show up even if the MCU is down.

These are some possibilities:

- The MCU isn't running because of insufficient power
- The firmware on the MCU is corrupted
- The MCU is damaged

A likely cause of insufficient power is damage to the R55 resistor.
If the Power LED is on, the 3.3V supply is available.
In
[this blog post](https://www.electric-skateboard.builders/t/diy-6s-to-12s-bms-with-can-diebiems/2639/989)
@jtag explains that if the USB port is connected without a pack present, R55 burns out.
This means you should suppress your (very understandable) urge to connect USB to your DBMS without a battery or power supply of at least 12V on the B+ and B- terminals.
Here is another
[related blog post](https://www.electric-skateboard.builders/t/diy-6s-to-12s-bms-with-can-diebiems/2639/819).
If R55 fails, the power circuit does not get Vin from the battery pack and the 3.3 V rail from the LM5165 (IC3) chip for the device won't work and the MCU won't power up rendering communication impossible.
You can workaround with a 15 ohm through-hole resistor or if you have hot-air rework, replace the resistor.

If the Power LED is lit but the Status LED isn't flashing, the MCU firmware may not be running or may be corrupted.
If the MCU firmware is corrupted, you'll need to reflash the firmware without the bootloader over the debug port on the underside of the board.

# DBMS turns on briefly and then shuts down

If a cell voltage is reading below the minimum cell voltage, the MCU will shut down.
A cell voltage that expected by the firmware but not connected or configured properly will cause a zero reading.
If the balance connector is not connected, we'd expect this to happen.

This [post](https://www.electric-skateboard.builders/t/diy-6s-to-12s-bms-with-can-diebiems/2639/497)
and this [post](https://www.electric-skateboard.builders/t/diy-6s-to-12s-bms-with-can-diebiems/2639/576)
are both examples of the issue.
