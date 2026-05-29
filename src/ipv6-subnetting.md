# IPv6 Subnetting.

Since a v6 address is 128 bits, we need a way to express that many digits with fewer characters.

The IETF settled on hex.

`0000`

Looking at just the first digit, of the first hextet we see this:

| Hex | Binary |
|-----|------- |
| 0   | `0000` |
| 1   | `0001` |
| 2   | `0010` |
| 3   | `0011` |
| 4   | `0100` |
| 5   | `0101` |
| 6   | `0110` |
| 7   | `0111` |
| 8   | `1000` |
| 9   | `1001` |
| A   | `1010` |
| B   | `1011` |
| C   | `1100` |
| D   | `1101` |
| E   | `1110` |
| F   | `1111` |

## Examples

### /40

You're given `3fff::/20`, make some `/40` networks.

A `/40` is two hextets, and two digits worth of bits.

- `3fff:0:0000::/40`
- `3fff:0:0100::/40`
- `3fff:0:0200::/40`
- `3fff:0:0300::/40`
- `3fff:0:0400::/40`
- `3fff:0:0500::/40`

which becomes

- `3fff:0::/44`
- `3fff:0:100::/40`
- `3fff:0:200::/40`
- `3fff:0:300::/40`
- `3fff:0:400::/40`
- `3fff:0:500::/40`

### /44

You're given `3fff::/20`, make some `/44` networks.

A `/44` is two hextets, and three digits worth of bits.

- `3fff:0:0000::/44`
- `3fff:0:0010::/44`
- `3fff:0:0020::/44`
- `3fff:0:0030::/44`
- `3fff:0:0040::/44`
- `3fff:0:0050::/44`

which becomes

- `3fff:0::/44`
- `3fff:0:10::/44`
- `3fff:0:20::/44`
- `3fff:0:30::/44`
- `3fff:0:40::/44`
- `3fff:0:50::/44`

### /48

You're given `3fff::/20`, make some `/48` networks.

A `/48` is three hextets, worth of bits.

- `3fff:0:0::/48`
- `3fff:0:1::/48`
- `3fff:0:2::/48`
- `3fff:0:3::/48`
- `3fff:0:4::/48`

### /52

You're given `3fff::/20`, make some `/52` networks.

A `/52` is three hextets, plus 1 hex digit worth of bits.

- `3fff:0:0:0::/52`
- `3fff:0:0:1000::/52`
- `3fff:0:0:2000::/52`
- `3fff:0:0:3000::/52`
- `3fff:0:0:4000::/52`

## Cursed Subnets

These do not fall on a hex digit boundary.

| Bits Borrowed | Boundaries                                     |
|---------------|------------------------------------------------|
| 1 bit         | 0, 8                                           |
| 2 bits        | 0, 4, 8, C                                     |
| 3 bits        | 0, 2, 4, 6, 8, A, C, E                         |

### /53

You're given `3fff::/20`, make some `/53` networks.

A `/53` is three hextets, plus one extra bit.

- `3fff:0:0:0::/53`
- `3fff:0:0:8000::/53`
- `3fff:0:1:0::/53`
- `3fff:0:1:8000::/53`
- `3fff:0:1:8000::/53`

### /54

You're given `3fff::/20`, make some `/54` networks.

A `/54` is three hextets, plus two extra bits.

- `3fff:0:0:0::/54`
- `3fff:0:0:4000::/54`
- `3fff:0:0:6000::/54`
- `3fff:0:0:8000::/54`
- `3fff:0:0:C000::/54`

### /55

You're given `3fff::/20`, make some `/55` networks.

A `/55` is three hextets, plus three extra bits.

- `3fff:0:0:0::/55`
- `3fff:0:0:2000::/55`
- `3fff:0:0:4000::/55`
- `3fff:0:0:6000::/55`
- `3fff:0:0:8000::/55`

# References

[IPv6 address - Wikipedia](https://en.wikipedia.org/wiki/IPv6_address)
