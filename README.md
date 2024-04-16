NOTE:	This repository is a work in progress and none of the documents have been ratified by the Lumian Foundation.

# Overview

The Cryptographic Key ID (CKID) is a new ID format that has the following qualities:

* Statistically guaranteed to be unique
* Does not require a centralized entity to be generated- devices can generate their own CKID
* Proof of ownership-  The CKID allows devices to prove ownership of the private key using public key cryptography
* Built-in error detection
* Ciphersuite independent- CKID support any mechanism for generating private keys.

It can be used as a database index for storing data on any electronic object and is the foundation of identity in the Lumian Network.

# Basic Format

The CKID includes three basic elements:

* Type identifier, allowing for variations in the Hash and CRC elements within the CKID.
* Hash output of public-key-material.
* (Optional) CRC check, allowing some basic error detection within the CKID.

The CKID is defined in the following way:

TYPE  = addr-type-identifier 
HASH  = hash-function ( public-key-material )
CHECK = check-sum ( TYPE | HASH )
CKID   = TYPE | HASH | CHECK

## Address Type Identifier  

The addr-type-identifier consists of 2 bytes and identifies the hash function, the check sum function and the public-key-material used to calculate the CKID.  The addr-type-identifier determines the length of the CKID.  For example, all CKIDs with addr-type-identifier of 0x0001 are 22 bytes.

## Hash Function

Using a hash of the public-key-material, rather than the public-key-material itself, creates a fixed-length CKID, independent of the underlying type of public key or the number of keys being used. The principle of the hash ensures that different public keys will result in different hash values, with a very high probability. I.e. devices employing true random key generation will result in unique hash values.

## Check Sum

Including a check-sum within the CKID allows the detection of possible bit errors within the CKID, e.g. in case a CKID is incorrectly copied or entered from a human. The maximum number of detectable bit errors depends on the check-sum being used. The check-sum is calculated over the concatenated TYPE and HASH values:

* TYPE: 	Unsigned short int. 2 Byte. LSB first.
* HASH:	Array of unsigned char.

CKID may use  different check-sum functions, as indicated within the addr-type-identifier.

# Public Key Material

The public-key-material ties the CKID to one or more private/public key pairs. The public-key-material shall include at least one public key. The specific key material of the public-key-material is indicated within the addr-type-identifier.

# CKID Representations

The CKID, as defined above, has a binary form. The following CKID representations are equivalent:

* Binary representation: 	8 bits of CKID / byte.
* Base64 encoded: 	6 bits of CKID / byte.
* Hexadecimal representation: 	4 bits of CKID / byte.

Due to the nature of Base64 encoding, the Base64-encoded CKID is case-sensitive. Use of Base64 encoded CKID is not encouraged. The Hexadecimal representation of the CKID shall be case insensitive. It is recommended to output a hexadecimal representation of the CKID in upper case letters. Use of hexadecimal encoded CKID is encouraged.

# Ethereum Type

For addr-type-identifier of 0x0001, the CKID is identical to the 20 Byte check-summed Ethereum address. In Ethereum Virtual Machine (EVM) smart contracts the 22-byte CKID is prepended by 6 bytes of 0x00 so the CKID can be stored in a standard bytes32 variable type.
