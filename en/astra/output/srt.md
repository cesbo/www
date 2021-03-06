# SRT

SRT (Secure Reliable Transport) is an UDP based protocol with high stability and security.
[Read more](/en/book/protocols/srt/)

!!! note
    Version: 2021-12-21 or later

## Caller address format

In the caller mode transmitter sends request to the receiver.

```
srt://host:port#options
```

- `host` – remote server address
- `port` – remote port

Options:

- `streamid=ID` – stream identifier
- `passphrase=Pass` – password for the encrypted transmission. Password length should be in range 10 .. 79 characters
- `pbkeylen=N` – crypto key length in bytes. Possible values: 16, 24, 32
- `tsbpdmode` – timestamp-based packet delivery mode
- `packetfilter` - injecting extra processing instructions at the beginning and/or end of a transmission to implement Forward Error Correction (FEC). [Read more](https://github.com/Haivision/srt/blob/master/docs/features/packet-filtering-and-fec.md)

## Listener address format

In the listener mode transmitter awaits requests from the receivers.

```
srt://@:port
srt://interface@:port#options
```

- `interface` – name of the local interface. If interface is not defined Astra accept requests from any interface
- `port` – local port

Options:

- `tsbpdmode` – timestamp-based packet delivery mode

## Web Interface

In the web interface SRT Output options available in the Stream options. You can write destination address directly to the Output address line:

![Output address](output-list.png){: width="400"}

Or click to the gear icon and use an Output configuration form. SRT Caller options:

![SRT Caller Output options](srt-caller.png){: width="400"}

SRT Listener options:

![SRT Listener Output options](srt-listener.png){: width="400"}
