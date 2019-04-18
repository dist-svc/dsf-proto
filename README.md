# Introduction

the Distributed Service Framework (DSF) is an approach to secure, trustworthy and distributed registration and discovery of arbitrary services, based on public key cryptography for identity and authenticity, secret key cryptography for privacy, and utilising a kademlia-derived distributed hash table \(DHT\) as a supporting Database.

This is designed to support the simple and safe development and deployment of distributed applications, without requiring any specific knowledge of distributed systems or cryptography. You can read more about the goals and approach [here](https://drive.google.com/open?id=1-RFdQuVXcE5GmVBgtxzvZzFpKU0OnxE6), and view this documentation rendered [here](https://ryankurte.gitbook.io/dsf).

The use of a DHT allows DSF to operate in a peer-to-peer manner, without requiring common / centralised / trusted infrastructure. The use of public-key cryptography derived identifiers allows services \(and communications\) in DSF to be verified.

Services in DSF consist of a public-private key pair, a _**Service IDentifier \(SID\)**_ derived from this public key, and arbitrary data and metadata to support the service. _**Services**_ _publish_ _**Pages**_ containing relevant information to the _**Database**_ at a given _**SID**_, and can be located using these _**SIDs**_.
consumers can use the information in the pages to interact directly with service or service replicas.

In addition to _Primary_ pages, published by the service directly, _Secondary_ pages can be published to a Service ID by third parties to provide supporting information to a service. For example, providing alternate addresses for service replication.

## Goals

* **Must** provide trustworthy / verifyable / immutable service identities
* **Must** support service mobility and updates to services
* **Must** support registration and discovery of arbitrary services
* **Must** provide mechanisms for private / secure service registration and discovery
* **Must** provide simple / human centric approaches for service registration and discovery
* **Should** use small / efficient encodings for use with resource-constrained and embedded system
  * Except where this would impair protocol compatibility or similar


