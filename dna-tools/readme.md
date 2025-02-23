# DNA Tools

These tools are to help configure a SYZYGY devices DNA information. They take the DNA settings stored in a JSON file (see the `szg-dna` folder) and convert them into into binary data to store on the SYZYGY MCU. 

The updated python tool is an all in one solution for generating SYZYGY DNA and programing peripheral devices with firmware and DNA.

C++ versions for generating DNA and Power Sequence binary data are also included.

# SYZYGY DNA Python Tool

The `syzygy-tools.py` script is an all-in-one tool for programming SYZYGY pMCU (ATTINY44A) firmware as well as parsing and loading SYZYGY DNA JSON config files. See the usage help below for all of the functions.

## Setup

- Install python 3.8+
- Install the required python packages: `pip install -r requirements.txt`
- Install AVRDUDE for programming


### Setup for AVRDUDE in Windows


- Install `AVRDUDE for Windows` https://github.com/mariusgreuel/avrdude
  - AVRDUDE for Windows includes support for ATATMEL-ICE programmer, where as WinAVR does not.
  - Download the release zip and extract avrdude.exe, avrdude.conf and avrdude.pdb to a folder. 
  - Add that folder to the system [PATH](https://helpdeskgeek.com/windows-10/add-windows-path-environment-variable/)

## Running the Tool

**Examples**
- `python .\syzygy-tools.py -d szg-adc-ltc2264.json -f ..\pod-fw\avr-dna-fw-main.hex`
- `python .\syzygy-tools.py -d szg-tst-txr4.json -f ..\pod-fw\avr-dna-fw-test.hex`


### Usage Help
```
usage: syzygy-tools.py [-h] -d DNA [-s SERIAL] [-f FIRMWARE] [-b] [--dna_binary_out DNA_BINARY_OUT] [--sequence_binary_out SEQUENCE_BINARY_OUT] [--firmware_out FIRMWARE_OUT]

optional arguments:
  -h, --help            show this help message and exit

Required Arguments:
  -d DNA, --dna DNA     The DNA JSON file for the target board

Additional Optional Arguments:
  -s SERIAL, --serial SERIAL
                        Set the boards serial. In barcode mode this sets the serial prefix. 
                        Uses a default date format as the prefix if none is supplied.
  -f FIRMWARE, --firmware FIRMWARE
                        The SYZYGY firmware hex file for programming. AVRDUDE is used to program the board.
  -b, --barcode_scan_mode
                        Start barcode scan mode for batch programming. Input the board serial prefix 
                        (datecode) as the --serial argument. Every time a serial sticker on a board is 
                        scanned the serial will be updated and the board will be programmed.
  --dna_binary_out DNA_BINARY_OUT
                        A filename to write out the DNA data
  --sequence_binary_out SEQUENCE_BINARY_OUT
                        A filename to write out the Power Sequence data
  --firmware_out FIRMWARE_OUT
                        A filename to write out the combined firmware hex to, instead of programming a board
```


# C++ DNA Writer Application

The `dna-writer` application can be used to generate a binary DNA blob from a JSON-format input. This application uses the [JSON for Modern C++](https://github.com/nlohmann/json) library. The DNA writer conforms to the SYZYGY DNA Specification v1.1. Please see the [SYZYGY](http://syzygyfpga.io/) website for more information.

Please see the examples in the `pod-dna` folder for peripheral DNA examples.

# C++ Sequence Writer Application

The `sequence-writer` application can be used to generate binary blobs from a JSON-format input. This binary data is stored in the EEPROM and can be used on a peripheral to control power enables and other hardware sequences based on sensed rail voltage levels.

Please see the examples in the `pod-dna` folder for peripheral DNA examples.