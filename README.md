# Overview

DSD is an approach to secure, trustworthy and distributed registration and discovery of arbitrary services, based on Public Key Cryptography for identity and Authenticity, Secret Key Cryptography for privacy, and utilising a Kademlia-derived Distributed Hash Table (DHT) as a supporting Database.

The use of a DHT allows DSD to operate in a peer-to-peer manner, without requiring common / centralised / trusted infrastructure. The use of public-key cryptography derived identifiers allows services (and communications) in DSD to be verified.

Services in DSD consist of a public-private key pair, and identifier derived from this public key, and arbitrary data and metadata to support the service. ***Services*** *publish* primary pages containing relevant information to the ***Database*** at a ***Service IDentifier (SID)*** derived from their public keys. Services can be located using these ***SIDs***, and consumers can use the information in the pages to interact with the service.

In addition to *Primary* pages, published by the service directly, *Secondary* pages can be published to a Service ID by third parties to provide supporting information to a service. For example, providing alternate addresses for service replication.

## Goals

- **Must** provide trustworthy / verifyable / immutable service identities
- **Must** support service mobility and updates to services
- **Must** support registration and discovery of arbitrary services
- **Must** provide mechanisms for private / secure service registration and discovery
- **Must** provide simple / human centric approaches for service registration and discovery
- **Should** use small / efficient encodings for use with resource-constrained and embedded system
  - Except where this would impair protocol compatibility or similar
