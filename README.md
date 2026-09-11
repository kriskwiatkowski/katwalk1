# katwalk

This repository has been archived. New version of the tools is available [here](https://github.com/kriskwiatkowski/katwalk2)

Utility to iterate over NIST Known Answer Tests vectors from CAVP. It
allows to bind an action for each test vector supplied by calling code. 

## Supported schemes:
| Algorithm | NIST Specification name     |
|-----------|----------------------------|
| SHA2      | FIPS-180-4 |
| SHA3      | FIPS-202 |
| SHAKE     | FIPS-202 |
| HMAC      | FIPS-198 |
| Diffie-Hellman | SP 800-56A |
| KDF | SP 800-108 |
| DRBG      | SP 800-90A (without Prediction Resistance) |
| NIST PQC  | All KEM & Signature schemes  |

## Example
Here below an example of usage for one vector for SHA3 KAT (FIPS 202).
```
// Vector copy pasted from NIST specs
let ex = "
Len = 0
Msg = 00
MD = 6b4e03423667dbb73b6e15454f0eb1abd4597f9a1b078e3f5b5a6bc7";

    // Some variables
    let mut count = 0;
    // Create an iterator for HASH algorithm
		let r = KatReader::new(
			std::io::BufReader::new(Cursor::new(ex)),
			AlgType::AlgHash, 1);
		
    // Iterate over all KATS. The ``el`` will contain fields
    // parsed from KAT files. Those fields are used as input
    // to cryptographic implementation and expected output.
		for el in r {
			assert_eq!(el.hash.md.len(), 28);
			assert_eq!(el.hash.len, 0);
			assert_eq!(el.hash.msg, [0x00]);
			assert_eq!(el.hash.md[0..5], [0x6B, 0x4E, 0x03, 0x42, 0x36]);
			count+=1;
		}
		assert_eq!(count, 1);
```

## Used by
It is used by PQC library ([here](https://github.com/kriskwiatkowski/pqc/blob/main/test/katrunner/src/main.rs)) for functional testing.

## Status
Algorithms are added on "as needed" bases. Don't judge implementation, it needs major rewrite.
