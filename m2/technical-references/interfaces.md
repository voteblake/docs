# Automotive Interfaces

The M2 Inferface Board contains both the power supply circuitry and the automotive interfaces required to communicate with your car. This includes:

- 2x CAN bus
- 1x SWCAN (single wire can)
- 2x LIN/9141
- J1805 VPW/PWM

To find libraries for any M2 interfaces, try the [Macchina Community Showcase] (http://showcase.macchina.cc/libraries.html).

<img src="/images/Interface_0d024.png" width="640" />

In addition, every M2 has a 26 pin "expansion" connector that provides even more connection options. For example: UART, SPI, I2C, general purpose 12V drivers and 12V analog inputs. See the [schematic](https://github.com/macchina/m2-hardware) for actual pinout details.

Refer to the following diagram for pin 1 location.

<img src="/images/26pin_connector.png" width="640" />

## CAN

[Controller Area Network](https://en.wikipedia.org/wiki/CAN_bus)

CAN bus is a vehicle bus standard used in most cars built after 2006. It is a message-based protocol that allows modules within a car to communicate with one another. While the physical layer is understood and open, the actual meaning of the messages sent over the bus are not. While some messages are legislated to be "standard", the majority of CAN messages in your typical car are not well documented.

The M2 has 2 CAN channels (in addition to the single-wire CAN channel) that can interface directly to the CAN bus network of your car. The M2 uses the 2 built-in CAN controllers found in the SAM3X and 2 external TJA1051 transceivers.

Here is the link to the datasheet: http://www.nxp.com/docs/en/data-sheet/TJA1051.pdf

CAN bus connections can be found on either the 16-pin OBD2 connector on the under-the-dash M2 or the 24-pin connector used by the under-the-hood M2.

<!-- TO DO: Discuss termination and how to terminate with solder jumpers on interface board. -->

<!-- TO DO: Add schematic page for CAN bus transceivers -->

## Single-wire CAN

The Macchina M2 provides single-wire CAN support using a MCP2515 CAN controller.

## LIN

[Local Interconnect Network](https://en.wikipedia.org/wiki/Local_Interconnect_Network) bus is an inexpensive, single wire, serial network protocol used in many modern cars. Typically, LIN would be used to control and monitor lower-priority devices such as seat positions, door locks, radio and illumination.

The M2 has 2 LIN channels that can interface directly to the LIN bus network of your car. Your M2 uses 2 external TJA1027 transceivers connected via UART to the processor.

Here is the link to the datasheet: https://www.nxp.com/docs/en/data-sheet/TJA1027.pdf

Note that the TJA1027 transceiver is used for both LIN and ISO9141 (K-LINE/L-LINE) for a total of 2 channels.

LIN bus connections can be found on either the 16-pin OBD2 connector on the under-the-dash M2 or the 24-pin connector used by the under-the-hood M2.

## K-line

ISO9141 (K-Line) interface uses the TJA1021. While this part is designed for LIN, it is also K-line compatible.

Here is the link to the datasheet: https://www.nxp.com/docs/en/data-sheet/TJA1027.pdf

## J1850

J1850 supports multiple/variants modes. To change between the levels required for PWM and VPW variants of J1850, use this signal:

`J1850_PWM_nVPW`

This signal is connected to physical pin 123 (PB8) of the SAM3X.

Make this pin HIGH for PWM and LOW for VPW

The following code that will both turn on power to J1850 circuit AND set level for either PWM or VPW:

```CPP
void setup() {
  pinMode(PS_J1850_9141, OUTPUT);
  pinMode(PWM_nVPW, OUTPUT);

  digitalWrite(PS_J1850_9141, HIGH);  // LOW  = no power at +12V_SW/+5V_SW
                                      // HIGH = power at +12V_SW/+5V_SW

  digitalWrite(PWM_nVPW, HIGH);       // LOW  = ~7.9v (VPW)
                                      // HIGH = ~5.9V (PWM)
}

void loop() {
}
```
