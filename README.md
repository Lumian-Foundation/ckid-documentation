NOTE:	This repository is a work in progress and none of the documents have been ratified by the Lumian Foundation.

# Overview

The Cryptographic Key ID (CKID) is a new ID format that extends the concept of blockchain addresses to any object capable of generating and using a cryptographic private key, such as an IoT device or cloud server.  CKIDs have the following qualities:

* Unique- CKIDs are statistically unique if they are generated correctly.
* Decentralized- CKIDs can be generated by any device that needs a new CKID.  
* Provable-  a CKID allows a device to prove ownership of the CKID using public key cryptography.
* Error Detection- the CKID format supports multiple types of error detection.
* Ciphersuite Flexibility- a CKID is derived from a cryptographic key and supports commonly used mechanisms for generating such keys.

The CKID can be used as an index for storing data on electronic objects and is the foundation of identity in the Lumian Network.

# Basic Format

The CKID includes three basic elements:

* Type identifier, allowing for variations in the Hash and CRC elements of a CKID.
* Hash output of *public-key-material*.
* (Optional) CRC check, allowing some basic error detection within the CKID.

The CKID is defined in the following way:

TYPE  = addr-type-identifier 

HASH  = hash-function ( public-key-material )

CHECK = check-sum ( TYPE | HASH )

CKID   = TYPE | HASH | CHECK

## Address Type Identifier  

The *addr-type-identifier* consists of 2 bytes and identifies the *hash function*, the *check sum* function and the *public-key-material* used to calculate the CKID.  The *addr-type-identifier* determines the length of the CKID.  For example, all CKIDs with *addr-type-identifier* of 0x0001 are 22 bytes (an Ethereum address is 20 bytes).

## Hash Function

Using a hash of the *public-key-material*, rather than the *public-key-material* itself, creates a fixed-length CKID, independent of the underlying type of public key or the number of keys being used. The principle of the hash ensures that different public keys will result in different hash values, with a very high probability, i.e. devices employing true random key generation will result in unique hash values.  Note that some CKIDs do not use a hash.  The *addr-type-identifier* of 0x0001 (Ethereum address) is an example.

## Check Sum

Including a *check-sum* within the CKID allows the detection of possible bit errors within the CKID, e.g. in case a CKID is incorrectly copied or entered from a human. The maximum number of detectable bit errors depends on the check-sum being used. The check-sum is calculated over the concatenated TYPE and HASH values:

* TYPE: 	Unsigned short int. 2 Byte. LSB first.
* HASH:	Array of unsigned char.

CKID may use  different check-sum functions, as indicated within the *addr-type-identifier*.

## Public Key Material

The *public-key-material* ties the CKID to one or more private/public key pairs. The *public-key-material* shall include at least one public key. The specific key material of the *public-key-material* is indicated by the *addr-type-identifier*.

# CKID Representations

The CKID, as defined above, has a binary form. The following CKID representations are equivalent:

* Binary representation: 	8 bits of CKID / byte.
* Base64 encoded: 	6 bits of CKID / byte.
* Hexadecimal representation: 	4 bits of CKID / byte.

Due to the nature of Base64 encoding, the Base64-encoded CKID is case-sensitive. Use of Base64 encoded CKID is not encouraged. The Hexadecimal representation of the CKID shall be case insensitive. It is recommended to output a hexadecimal representation of the CKID in upper case letters. Use of hexadecimal encoded CKID is encouraged.

# Ethereum CKID

For *addr-type-identifier* of 0x0001, the CKID is identical to the 20 Byte check-summed Ethereum address. In Ethereum Virtual Machine (EVM) smart contracts the 22-byte CKID is prepended by 6 bytes of 0x00 so the CKID can be stored in a standard Solidity bytes32 variable type.

# Integration with TLS

The CKID can be derived after a [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) handshake.  TLS is a commonly used protocol to secure communications over public networks and is required by technical standards such as IEEE 2030.5.  This is the procedure a cloud server could use to get information on a client during a client request:

* Enforce mutual authentication TLS on a connection request.
* Obtain the client’s X.509 certificate once the TLS handshake completes.
* Extract the public key from the certificate.
* Construct the CKID from the public key in accordance with the appropriate CKID *addr-type-identifier*.
* Use the CKID to lookup data on the client.

# Future Work

This repository will be the location for all public Lumian Foundation documentation related to CKID, including specifications and code snippets.
