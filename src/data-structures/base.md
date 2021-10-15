# Base

A common common base structure is used across Pages, Messages, and Data, to simplify encoding and parsing. All base objects contain an ID derived from the service or node public key and a signature, allowing validation prior to further parsing \(so long as the associated key is known\).

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
/                           Header                              /
/          Protocol Defined Header Length (48-bytes)            /
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                             Data                              /
/                Optional, Variable Length Data                 /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                        Secure Options                         /
/                 Optional, Variable Length Data                /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                    Symmetric encryption Tag                   /
/                        Optional, 40 bytes                     /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                        Public Options                         /
/                Optional, Variable Length Data                 /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
/                           Signature                           /
/          Protocol Defined Signature Length (64-bytes)         /
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Header

```text
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Protocol Version = 0x0000   |        Application ID         |  
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|           Object Kind         |             Flags             |         
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Index            |            Data Len           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|       Secure Options Len      |       Public Options Len      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
/                               ID                              /
/             Protocol Defined ID Length (32-bytes)             /
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Object Kind:

```text
 0                   1           
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Kind         |   B   |       
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Flags:

```text
 0                   1           
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|A|         Reserved        |E|S|        
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Header Fields

* **Protocol Version** is the DSF protocol version, this must be 0x0000
* **Flags**
  * Bit 15: Secondary, indicates this is a secondary object
  * Bit 14: Encrypted, indicates data and secure options fields have been encrypted
  * Bit 0: Address Request, messages only, indicates the responder should attach a peer address option to the response \(may be used for address discovery\)
* **Application ID** indicates a specific application in DSF \(0x0000 for DSF core\)
* **Object Kind** indicates protocol-specific page or message kind
  * kinds must be globally unique within DSF
  * To apply for a page kind \(or kinds\) apply for a PR on this repo against the `KINDS.yml` listing
  * For testing purposes and/or private use that does not require registration, an application ID of  `0x0FFF` may be used
  * Bit 15 indicates whether a page is defined by DSF core (0) or the application protocol (1)
    * This is to allow the re-use of standard pages between applications where suitable, and to inform DSF as to whether objects should be parsed by the daemon
  * Bits 14:13 indicate the block base kind
    * 0b00 for pages \(service / peer registration etc.\)
    * 0b01 for request messages \(between peers\)
    * 0b10 for response messages \(between peers\)
    * 0b11 for data \(merkle-tree based data structures\)
* **Index**
  * For pages, this is a _Page Version_, a monotonically increasing counter for page replacement
  * For messages, this is a _Request ID_ to uniquely map request and response objects
  * For data, this field must be 0x0000
* **Data Length**, length in bytes of the variable length data field
* **Secure Options length**, length in bytes of the variable length secure options field
  * This allows options such as IP addresses for service connections to be encrypted alongside service data
* **Public options length**, length in bytes of the variable length public options field
  * These options are used to specify public page information such as Public Keys and Expiry Time
* **ID** is the _Service ID_ for Pages and Data or the _Node ID_ for messages

## Body

* **Data** contains arbitrary data for service pages \(based on the page kind\), or DSF data for messages \(such as pages to be transferred\). The data section may be encrypted.
* **Secure Options** contain options that are encrypted and can thus only be parsed by those with the appropriate keys
* **Public Options** contain public options associated with a page or message

## Signature

* **Signature** is a cryptographic signature across the whole object \(header included\) used to validate the authenticity of pages and messages.

## Encryption

If the encryption flag is set, Data and Secure Options fields are encrypted using the specified algorithm, and must be decrypted before use.
The specified cryptographic algorithm is executed over the body and private options fields, with the tag (Nonce and MAC) injected immediately after.
