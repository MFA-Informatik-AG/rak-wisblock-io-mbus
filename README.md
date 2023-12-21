# rak-wisblock-io-mbus
RAK WisBlock IO wired M-Bus adapter 

![M-Bus IO Adapter](/docs/MBUSAdapterv30pcb.png)

The repository contains the schematic and PCB layout for a Wired M-BUS Slave Adapter that can be connected to a RAK WisBlock Core as an IO module.

The module uses the **TI TSS721A single-chip meter-bus transceiver** with reference circuitry on a RAK IO module. The TSS721A is powered by the Wired M-BUS Master in the circuit, which reduces the power consumption via the RAK WisBlock Core to a minimum (which is why I have also dispensed with a status LED).

The Wired M-BUS Adapter was only tested for receiving data with the L&G E450 Smart Meter, but should also be usable for receiving and sending data with other Wired M-BUS Masters.

The Wired M-BUS Adapter can be switched on and off via the RAK WisBlock Core by setting the PIN WB_IO2 to LOW or HIGH:


	void mbusAdapterOff()
	{
		pinMode(WB_IO2, OUTPUT);
		digitalWrite(WB_IO2, LOW);
	}

	void mbusAdapterOn()
	{
		pinMode(WB_IO2, OUTPUT);
		digitalWrite(WB_IO2, HIGH);
	}


This can be useful to save power during readout pauses or to reset the Wired M-BUS Adapter.

The Wired M-BUS Adapter can be read out directly via the serial interface of the MCU, e.g. with the RAK Wisblock 4631 and the L&G E450 (example):


    Serial1.begin(2400, SERIAL_8E1);

	while(true)
	{
		if(Serial1.available() > 0)
		{
			int number = Serial1.read();

			if(number >=0)
			{
				int newState = !digitalRead(LED_BUILTIN);   

				digitalWrite(LED_BUILTIN, newState);
			}
		}
		else
		{
			digitalWrite(LED_BUILTIN, LOW);

			delay(100);
		}
	}


The parameters for the serial interface depend on the smart meter. Further code for processing a CII Push Smart Meter will follow shortly as part of the Smartspar project.


