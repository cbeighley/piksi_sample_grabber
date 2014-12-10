Binaries:
sample_grabber
--------------
Receives an arbitrary number of raw samples from the Piksi,
using the onboard FT232H in FIFO mode. The FT232H on the Piksi
must be set in FIFO mode before sample_grabber can be used -
run set_fifo_mode to do this. After finishing using
sample_grabber, use set_uart_mode to set the FT232H on the
Piksi in UART mode for normal operation.

set_fifo_mode : Writes settings to EEPROM attached to FT232H for synchronous
                FIFO mode in order to stream raw samples from the RF frontend
                through the FPGA. Must be used before running sample_grabber.

set_uart_mode : Erases EEPROM attached to FT232H to set the FT232H in UART mode
                for normal operation. Should be used after running
                sample_grabber.

Building : to build the programs run "make"

Dependencies : libftdi1 : for sample_grabber
                   http://www.intra2net.com/en/developer/libftdi/download.php
                   libftdi1 has the following dependencies:
                       libusb-1.0-0-dev
                       libconfuse
                   We recommend installing these from source from the link.
               FTDI D2XX drivers : for set_uart_mode and set_fifo_mode
                   http://www.ftdichip.com/Drivers/D2XX.htm

Note : Simultaneous sampling from multiple Piksies

  In order to simultaneously sample from multiple Piksies, they must be
  assigned different USB product ID's. The FTDI custom product ID for Piksi
  is 0x8398, and this is the ID Piksi is assigned by set_fifo_mode by
  default. If you want to simultaneously sample from another Piksi, it must be
  assigned a different product ID. To do this, use the -i option with
  set_fifo_mode, e.g. :

      $ ./set_fifo_mode -v -i 0x8399

  This will assign the Piksi the product ID 0x8399, instead of the default
  0x8398. Note that you'll need to now add the new USB Product ID to your USB
  permissions in /etc/udev/rules.d, or use sudo on the following commands. To
  collect samples from this Piksi, use sample_grabber with the -i option, e.g. :

      $ ./sample_grabber -v -s 100M -i 0x8399 my_sample_file.dat

  After you finish sampling with this Piksi, you'll want to set it back into
  UART mode for normal operation. set_uart_mode looks for a device with our
  custom USB product ID 0x8398 by default, so you'll need to supply the -i
  option to set_uart_mode to reset the Piksi with the modified id of 0x8399,
  e.g. :

      $ ./set_uart_mode -v -i 0x8399
