# DVB, ATSC, ISDB-T

DVB (Digital Video Broadcast) is a set of international open standards for digital television [Read more](/en/book/protocols/dvb/)

## Common adapter options

### General options

![General Options](dvb-general.png){: width="400"}

- `Enable` - turn adapter on/off;
- `Name` - adapter name;
- `ID` - unique adapter identifier. For new adapter you may keep this field blank and Astra generates it;
- `SAT>IP` - use adapter on remote server over SAT>IP protocol;
- `Adapter` - select system devices;
- `Type` - select adapter standard.

### Advanced options

![Advanced Options](dvb-advanced.png){: width="400"}

- `Enable monitoring` - enable telemetry sending to Zabbix or InfluxDB;
- `Timeout` - check DVR errors in defined interval. Default: 120;
- `CI` - use external DVB-CI adapter;
- `CI Device` - device number on DVB-CI adapter;
- `CI Bitrate` - define bitrate for DVB-CI adapter;
- `BISS` - descramble an entire transponder;
- `DVB API v3` - use deprecated API to read information from Adapter;
- `Budget Mode` - disable hardware PID filtering. In budget mode Astra receives whole transponder from adapter;
- `CA Delay` - delay between sending control packets to Conditional Access Module (CAM).

## DVB-S/S2

DVB-S/S2 is a standard for Satellite Television.

### DVB-S/S2: General options

![DVB-S/S2 Tuner](dvb-s.png){: width="400"}

- `Frequency` - carrier frequency (950-13250 MHz);
- `Polarization` - signal polarization. For linear polarization: Vertical, Horizontal. For circular polarization: Right, Left;
- `Symbolrate` - symbol rate (1000-50000 Kbaud).

### DVB-S/S2: LNB (Low-Noise Block) options

![DVB-S/S2 LNB](dvb-s-lnb.png){: width="400"}

Default options depends of the adapter frequency:

| Frequency range | LOF1 | LOF2 | SLOF |
| ---: | --- | --- | --- |
| 10700 .. 13250 | 9750 | 10600 | 11700 |
| 4500 .. 4800 | 5950 | 0 | 0 |
| 3400 .. 4200 | 5150 | 0 | 0 |
| 2500 .. 2700 | 3650 | 0 | 0 |
| 950 .. 2150 | 0 | 0 | 0 |

For circular polarized converters (e.g. 36°E, 56°E), set 10750 in LOF1

- `LOF1` - Low sub-band, MHz;
- `LOF2` - High sub-band, MHz;
- `SLOF` - Sub-band range. If adapter frequency more than SLOF, than will be used LOF2 value and sends tone (22 kHz) signal to LNB to switch;
- `Force Tone` - option to send 22 KHz tone signal.
- `LNB Mode` - select additional LNB modes

<div class="tip">

**Voltage 13V/18V** - is not only for LNB power supply but also choosing signal polarization

- 13V is for Vertical/Right polarization. Switching signal in the range of 11-14 Volts
- 18V is for Horizontal/Left polarization. Switching signal in the range of 16-20 Volts

**22kHz** signal is a heterodine switch (LOF1 or LOF2)

</div>

Available LNB modes:

- `LNB Sharing` - in this mode the LNB voltage supply and tone signal are disabled. Allows you to connect several DVB-adapters to single converter through a passive splitter. On the splitter, only one adapter must be active, the any other adapters should be passive. All adapters on same splitter should use same polarization and same sub-band (frequency is greater or less than the value of slof);
- `DiSEqC 1.0` - Digital Satellite Equipment Control is a communication protocol for use between a satellite receiver and a device such as a multi-dish switch or a small dish antenna rotor. Version 1.0 allows switching between up to 4 satellite sources;
- `DiSEqC 1.1` - allows switching between up to 16 satellite sources;
- `DiSEqC Command` - sends raw command to DiSEqC;
- `Tone Burst` - also known as mini DiSEqC. allows switching between 2 satellite sources.
- `Unicable I (EN50494)` - Provides simultaneous access to multiple LNB’s over a single coaxial cable for several receivers. Version I provide access up to 8 satellite sources;
- `Unicable II (EN50607)` - provides access to up to 32 satellite sources.

### DVB-S/S2: Advanced options

![DVB-S/S2 Advanced](dvb-s-advanced.png){: width="400"}

- `Modulation` - signal modulation. Default: QPSK for DVB-S, PSK8 for DVB-S2;
- `FEC` - forward error correction. Default: Auto;
- `Roll-Off` - spectrum efficiency. Only for DVB-S2. Default: 35;
- `Stream ID` - PLP stream ID for multistream transponders. Only for DVB-S2.

## DVB-T/T2

DVB-T/T2 is a standard for the broadcast transmission of digital terrestrial television.

### DVB-T/T2: General options

![DVB-T/T2 Tuner](dvb-t.png){: width="400"}

- `Frequency` - carrier frequency (0-1000 MHz).

### DVB-T/T2: Advanced options

![DVB-T/T2 Advanced](dvb-t-advanced.png){: width="400"}

- `Bandwidth` - Signal bandwidth in kHz. Default: 8000;
- `Guard` - Guard interval;
- `Transmit` - Transmission mode;
- `Hierarchy` -

## DVB-C

DVB-C is a standard for the broadcast transmission of digital television over cable.

### DVB-C: General options

![DVB-C Tuner](dvb-c.png){: width="400"}

- `Frequency` - carrier frequency (80-1000 MHz);
- `Symbolrate` - symbol rate (1000-10000 Kbaud).

### DVB-C: Advanced options

![DVB-C Advanced](dvb-c-advanced.png){: width="400"}

## Scan Adapter

Press "Scan" button to scan an adapter and get available information:

- Signal level and signal quality;
- Transponder information;
- Available channels.

![DVB Scan](dvb-scan.jpg){: width="400"}

Check all channels you want to append and press "Apply" button.

## Address format

```
dvb://adapter-id
dvb://adapter-id#options
```

- `adapter-id` - adapter identifier

Options:

- `pnr=Number` - select program from MPTS (Multi-Program Transport Stream)

## Stream options

In the web interface DVB Input options available in the Stream options. You can write source address directly to the Input address line:

![Input address](input-list.png){: width="400"}

Or click to the gear icon and use an Input configuration form:

![DVB Input options](dvb.png){: width="400"}

## Troubleshooting

<details class="marker">
<summary>failed to open frontend: Device or resource busy</summary>

Adapter is taken by another process. Maybe Astra started twice.

</details>

<details class="marker">
<summary>failed to open frontend: No such file or directory</summary>

DVB Driver is not installed in system.

</details>
