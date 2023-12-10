# rak-wisblock-io-mbus
RAK WisBlock IO wired M-Bus adapter 

![M-Bus IO Adapter](/docs/MBUSAdapterv30pcb.png)

Das Repository enthält den Schaltplan und das PCB-Layout für einen Wired M-BUS Slave Adapter, der als IO-Modul an einen RAK WisBlock Core angeschlossen werden kann.

Das Modul verwendet den **TI TSS721A Single-chip Meter-bus Transceiver** mit Referenzschaltung auf einem RAK IO-Modul. Der TSS721A wird in der Schaltung vom Wired M-BUS Master versorgt, was den Stromverbrauch über den RAK WisBlock Core auf ein Minimum reduziert (daher habe ich auch auf eine Status-LED verzichtet).

Der Wired M-BUS Adapter wurde nur für den Datenempfang mit dem L&G E450 Smart Meter getestet, sollte aber auch für den Empfang und Versand mit anderen Wired M-BUS Mastern verwendbar sein.

Der Wired M-BUS Adapter kann über den RAK WisBlock Core ein- und ausgeschaltet werden, indem der PIN WB_IO2 auf LOW oder HIGH gesetzt wird:



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


Dies kann nützlich sein, um in Auslesepausen Strom zu sparen oder um den Wired M-BUS Adapter zurückzusetzen.

Der Wired M-BUS Adapter kann direkt über die serielle Schnittstelle der MCU ausgelesen werden, z.B. mit dem RAK Wisblock 4631 und dem L&G E450 (Beispiel):


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


Die Parameter für die serielle Schnittstelle hängen vom Smart Meter ab. Weiterer Code zur Verarbeitung eines CII Push Smart Meters wird in Kürze im Rahmen des Smartspar-Projekts folgen.


