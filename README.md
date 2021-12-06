#	base.js

Fast base encoding / decoding of any given digits map.

**WARNING:** This module is **NOT RFC3548** compliant, it cannot be used for base16 (hex), base32, or base64 encoding in a standards compliant manner.


##	Example

base58

``` javascript
var BASE58 = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
var bs58 = require("base-x")(BASE58)

var decoded = bs58.decode("5Kd3NBUAdUnhyzenEwVLy9pBKxSwXvE9FMPyR4UKZvpe6E3AgLr")

console.log(decoded)
// => Uint8Array(33) [
//    128, 237, 219, 220,  17, 104, 241, 218,
//    234, 219, 211, 228,  76,  30,  63, 143,
//     90,  40,  76,  32,  41, 247, 138, 210,
//    106, 249, 133, 131, 164, 153, 222,  91,
//     25
// ]

console.log(bs58.encode(decoded))
// => 5Kd3NBUAdUnhyzenEwVLy9pBKxSwXvE9FMPyR4UKZvpe6E3AgLr
```


##	Some digits map

| Base | Digits                                                                |
| ---- | --------------------------------------------------------------------- |
| 2    | `01`                                                                  |
| 8    | `01234567`                                                            |
| 11   | `0123456789a`                                                         |
| 16   | `0123456789abcdef`                                                    |
| 32   | `0123456789ABCDEFGHJKMNPQRSTVWXYZ`                                    |
| 32   | `ybndrfg8ejkmcpqxot1uwisza345h769` (z-base-32)                        |
| 36   | `0123456789abcdefghijklmnopqrstuvwxyz`                                |
| 58   | `123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz`          |
| 62   | `0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz`      |
| 64   | `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_`    |
| 67   | `ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789-_.!~` |


##	How it works

It encodes octet arrays by doing long divisions on all significant digits in the array, creating a representation of that number in the new base. Then for every leading zero in the input (not significant as a number) it will encode as a single leader character. This is the first in the digits map and will decode as 8 bits. The other characters depend upon the base. For example, base58 packs roughly 5.858 bits per character.

This means the encoded string `000f` (base16, i.e., 0-f) will actually decode to 4 bytes unlike a canonical hex encoding which uniformly packs 4 bits into each character.

While unusual, this does mean that no padding is required and it works for bases like 43.


##	To use in browser

```shell
browserify -r ./src/index:base -o base.js && minify -o base.min.js base.js && rm base.js
```


##	Source
A direct derivation of the base58 implementation from [`bitcoin/bitcoin`](https://github.com/bitcoin/bitcoin/blob/f1e2f2a85962c1664e4e55471061af0eaa798d40/src/base58.cpp), generalized for variable length digits map.

