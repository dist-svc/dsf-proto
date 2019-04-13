# Base

A common common base structure is used across both Messages and Pages to simplify encoding and parsing. All pages provide contain an ID derived from the service or node public key and a signature, allowing validation of both Messages and Pages prior to parsing.

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Kind             |              Flags            |           
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|             Version           |            Data Len           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Secure Options Len      |       Public Options Len      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
/                               ID                              /
/             Protocol Defined ID Length (32-bytes)             /
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                             Data                              /
/       Optional, Variable Length Data (4-byte aligned)         /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                        Secure Options                         /
/        Optional, Variable Length Data (4-byte aligned)        /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                        Public Options                         /
/       Optional, Variable Length Data (4-byte aligned)         /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
/                           Signature                           /
/          Protocol Defined Signature Length (64-bytes)         /
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

Kind:

```text
 0                   1           
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Kind         |   B   |       
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

Flags:

```text
 0                   1           
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|A|         Reserved          |E|        
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Header Fields

* **Kind** indicates protocol-specific page or message kind
  * kinds must be globally unique within DSF
  * To apply for a page kind \(or kinds\) apply for a PR on this repo against the `KINDS.yml` listing
  * For testing purposes and/or private use that does not require registration, a page kind of  `0x0FFF` may be used
  * Bit 15 indicates whether the block is core DSF \(0\) or implementation \(1\) defined 
  * Bits 12:14 indicate the block base kind
    * 0bx000 for primary pages \(service / peer registration etc.\)
    * 0bx001 for secondary pages \(replicas, annotations, etc.\)
    * 0bx010 for request messages \(between peers\)
    * 0bx011 for response messages \(between peers\)
    * 0bx100 for data \(merkle-tree based data structures\)
* **Flags**
  * Bit 15: Body Encrypted, indicates data and secure options fields have been symmetrically encrypted
  * Bit 0: Address Request, messages only, indicates the responder should attach a peer address option to the response \(used for address discovery\)
* **Page version**, monotonically increasing counter for page replacement in the database
* **Data Length**, length of the variable length data field
* **Secure Options length**, length of the variable length secure options field
  * This allows options such as IP addresses for service connections to be encrypted alongside service data
* **Public options length**, length of the variable length public options field
  * These options are used to specify public page information such as Public Keys and Expiry Time
* **ID** is the Service ID for Pages or the Node ID for messages

## Body

* **Data** contains arbitrary data for service pages \(based on the page kind\), or DSF data for messages \(such as pages to be transferred\). The data section may be encrypted.
* **Secure Options** contain options that are encrypted and can thus only be parsed by those with the appropriate keys
* **Public Options** contain public options associated with a page or message

## Signature

* **Signature** is a cryptographic signature across the whole object \(header included\) used to validate the authenticity of pages and messages.

## Encryption

If the encryption flag is set, Data and SecureOptions fields are encrypted using the specified algorithm \(see [0x-security.md](https://github.com/ryankurte/DSF-proto/tree/c85420e467a9e6ea2e0c63afe79835a4f4ae71dc/0x-security.md)\), and must be decrypted before use. 
Encrypted fields must include any information required to decrypt them \(ie. Nonces\). In the current scheme, the cryptographic nonce is appended to the end of the field following encryption \(and increasing the field length\), and removed prior to decryption \(decreasing the field length\).

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                             Field                             /
/          Optional, Variable Length (4-byte aligned)           /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```
