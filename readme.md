# I2C Scanner
This application scans the I2C bus for attached devices. It sends a I2C message without any data (i.e. stop condition after sending just the address). If there is an ACK for the address, it assumed there is a device present.

There is not a defined way to detect if a I2C device is connected. Some device may not responsed correctly or it can corrupt the I2C communication on the bus. 

## Selecting the I2C bus
The project will use by default the I2C bus as defined on the Arduino header of any development kit. When the arduino I2C bus ``arduino_i2c`` is not defined in the device tree a compile error will occure. In that case the alias ``my-i2c`` must be defined to select the I2C bus. This can be done with a simple overlay file with the name of the build target board. This file must be placed in the project directory. See the bellow example to select the defined I2C bus of the thing:91 in the board device tree file.

**<thingy91_nrf9160_ns.overlay>**
```
/ {
	aliases {
		my-i2c = &i2c2;
	};
};
```
## Setting scan range
The application only scans 7-bit address, 10-bit address are currently not supported. The range for the scan be limited by setting the start en stop value in the kernel config. by default the scan is done from addres 0x08 to 0x7F. You can modify the range in your proj.conf using the bellow settings.

**<proj.conf>**
```
CONFIG_I2C_SCAN_ADDR_START=8
CONFIG_I2C_SCAN_ADDR_STOP=127
```
## Changing the I2C speed
By default it will use the standard defined I2C transfer speed for the target build board. This setting can be overwriten by modifying the device tree overlay file or the board definition. Bellow an example of a overlay file for changing the speed when using the ardiuno header. 

**<nrf52833dk_nrf52833.overlay>**
```
&arduino_i2c {
	clock-frequency = <I2C_BITRATE_STANDARD>; 
};
```

## Overlay file example for setting up I2C

bellow an example of a overlay file with the defintion of the alias and modifying the default values for the I2C bus node used with the scanner.

**<thingy91_nrf9160_ns.overlay>**
```
/ {
	aliases {
		my-i2c = &i2c2;
	};
};

&i2c3 {
	status = "okay";
	compatible = "nordic,nrf-twim";
	sda-pin = <30>;
	scl-pin = <31>;
	clock-frequency = <I2C_BITRATE_FAST>; 
};
```

