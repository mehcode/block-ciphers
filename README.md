# RustCrypto: block ciphers [![Build Status](https://travis-ci.org/RustCrypto/block-ciphers.svg?branch=master)](https://travis-ci.org/RustCrypto/block-ciphers)
Collection of block ciphers and block modes written in pure Rust.

## Warnings

Currently only AES implementations currently provide constant-time implementation.
If you do not really know what you are doing it's generally recommended not to
use other cipher implementations in this repository.

Additionally crates in this repository have not yet received any formal
cryptographic and security reviews.

**USE AT YOUR OWN RISK.**

## Supported algorithms
| Name     | Alt name   | Crates.io  | Documentation  |
| ------------- |:-------------:| :-----:| :-----:|
| [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) | Rijndael | [![crates.io](https://img.shields.io/crates/v/aes.svg)](https://crates.io/crates/aes) | [![Documentation](https://docs.rs/aes/badge.svg)](https://docs.rs/aes) |
| [Blowfish](https://en.wikipedia.org/wiki/Blowfish_(cipher)) |   | [![crates.io](https://img.shields.io/crates/v/blowfish.svg)](https://crates.io/crates/blowfish) | [![Documentation](https://docs.rs/blowfish/badge.svg)](https://docs.rs/blowfish) |
| [DES](https://en.wikipedia.org/wiki/Data_Encryption_Standard) |   | [![crates.io](https://img.shields.io/crates/v/des.svg)](https://crates.io/crates/des) | [![Documentation](https://docs.rs/des/badge.svg)](https://docs.rs/des) |
| [Kuznyechik](https://en.wikipedia.org/wiki/Kuznyechik) |  GOST R 34.12-2015  | [![crates.io](https://img.shields.io/crates/v/kuznyechik.svg)](https://crates.io/crates/kuznyechik) | [![Documentation](https://docs.rs/kuznyechik/badge.svg)](https://docs.rs/kuznyechik) |
| [Magma](https://en.wikipedia.org/wiki/GOST_(block_cipher)) | GOST 28147-89 and GOST R 34.12-2015 | [![crates.io](https://img.shields.io/crates/v/magma.svg)](https://crates.io/crates/magma) | [![Documentation](https://docs.rs/magma/badge.svg)](https://docs.rs/magma) |
| [RC2](https://en.wikipedia.org/wiki/RC2) |  ARC2  | [![crates.io](https://img.shields.io/crates/v/rc2.svg)](https://crates.io/crates/rc2) | [![Documentation](https://docs.rs/rc2/badge.svg)](https://docs.rs/rc2) |
| [Twofish](https://en.wikipedia.org/wiki/Twofish) | | [![crates.io](https://img.shields.io/crates/v/twofish.svg)](https://crates.io/crates/twofish) | [![Documentation](https://docs.rs/twofish/badge.svg)](https://docs.rs/twofish) |

### Minimum Rust version
All crates in this repository support Rust 1.22 or higher. (except `aesni` and
`aes` crates, which require Rust 1.27) In future minimum supported Rust version
can be changed, but it will be done with the minor version bump.

## Usage
Block cipher crates provide only bare block cipher implementations. For most
applications you will need to use some [block cipher mode of operation](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation)
which are generically implemented in the [`block-modes`](https://docs.rs/block-modes/) crate.

Lets use AES128-CBC with PKCS7 padding to show an example:

```Rust
extern crate aes_soft as aes;
extern crate block_modes;

use block_modes::{BlockMode, BlockModeIv, Cbc};
use block_modes::block_padding::Pkcs7;
use aes::Aes128;

// create an alias for convinience
type Aes128Cbc = Cbc<Aes128, Pkcs7>;

let mut cipher = Aes128Cbc::new_varkey(key, iv).unwrap();
// buffer must have enough space for message+padding
let mut buffer = [0u8; 32];
// copy message to the buffer
buffer[..msg_len].copy_from_slice(msg);
let encrypted_msg = cipher.encrypt_pad(&mut buffer, msg_len).unwrap();

let mut cipher = Aes128Cbc::new_varkey(key, iv).unwrap();
let decrypted_msg = cipher.decrypt_pad(encrypted_data).unwrap();
```

Note that this example does not use any authentification which can lead to serious
vulnarabilities! For Message Authentication Code implementations take a look at
[RustCrypto/MACs](https://github.com/RustCrypto/MACs) repository.

For CTR mode which transforms block cipher into synchronous stream cipher see
[`ctr`](https://docs.rs/ctr/) crate from
[RustCrypto/stream-ciphers](https://github.com/RustCrypto/stream-ciphers)
repository.

## License

All crates licensed under either of

 * [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0)
 * [MIT license](http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
