# Auge - Sumodd VL53LX Daughterboard

This repo contains the kicad PCB files, as well as libraries for footprints and symbols, for my
[VL53L4CD][https://www.st.com/en/imaging-and-photonics-solutions/vl53l4cd.html] daughterboard. It
is also compatible with VL53L0X. It is largely designed to work with my
[Sumodd motherboard](https://github.com/oddgrd/sumodd), but it could be used independently with
some [modification](#independent-use).

<details>
<summary><strong>Schematic</strong></summary>

![Schematic](image.png)

</details>

## Board setup

The board is two layers, 1mm thick, with a standard 1oz copper on each side:

- Top layer with the sensor and power indicator LED, as well as a largely uninterrupted ground pour.
- Bottom layer with the sensor support components and a ground pour, as well as the JST-GH 6 pin
connector.

Two tooling holes were added to opposite corners to meet JLCPCB assembly requirements. The
schematic includes fields for LCSC part numbers, which are used in JLCPCB assembly.

## Components

All components are laid out as documented in the [VL53L4CD datasheet][VL53L4CD], section 1.4. All
SMD support components are 0603 imperial, except the 4.7uF cap, which is 0805. I could have gone
smaller, but want to be able to make changes easily, when experimenting with I2C pullups and series
resistors.

- VL53L4CD: modern ToF sensor, offering accurate measurements up to two meters at 100Hz.
- JST-GH 6 pin connector. 4 pins for I2C and power, one pin to enable changing I2C addresses for
multiple devices on the same bus, and GPIO1 pin for data ready interrupts.
- Pull-ups:
    - XSHUT and DRDY, 10k, as recommended in datasheet.
    - SCL and SDA, DNP in the schematic. The datasheet specifies that these should be placed once 
    per bus, and I will have at least three of these daughterboards connected to my motherboard.
    Therefore, I will place them once on the motherboard. The daughterboard has footprints for
    them, for flexibility, in case the sensor should be used with a different
    motherboard/independently. The datasheet documents that we should have 1.5k pull up resistors
    for 1MHz I2C, if the I2C bus capacitance is below 90 pF. I assume that is the case, but I need
    to confirm.
- I2C series resistors, 100 ohms, which is recommended for 1MHz I2C with a <= 90 pF bus capacitance.
- Decoupling caps on [VL53L4CD][VL53L4CD] input, placed as close to the IC as possible, on the back
layer. 0.1uF closest, and 4.7 uF right next to it.
- Green LED that lights when power is connected.

The area around the sensor on the top side is left clear, to make room for a potential cover glass
in the future.

## Independent use

The board is designed to work with my [Sumodd motherboard](https://github.com/oddgrd/sumodd), so
while it has I2C pullup footprints, the resistors are DNP. If you want to use this board, you should
either place 1.5k pullups on the board itself, or on the I2C bus of the board you connect it to.
You may want to change the I2C series resistors too, depending on desired I2C speed and bus
capacitance. See the [VL53L4CD datasheet][VL53L4CD] for resistor values.

[VL53L4CD]: https://www.mouser.com/datasheet/2/389/vl53l4cd-2907214.pdf