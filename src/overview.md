# Overview

Services in DSF consist of a public-private key pair, and identifier derived from this public key, and arbitrary data and metadata to support the service. **Services** _publish_ **primary pages** containing relevant information to the **Database** at a **Service IDentifier \(SID\)** derived from their public keys. Services can be located using these **SIDs**, and consumers can use the information in the pages to interact with the service. DSF also supports **secondary pages** that can be published to a given **SID** by a third party, this supports service annotation and replication.

## Goals

* **Must** provide trustworthy / verifyable / immutable service identities
* **Must** support service mobility and updates to services
* **Must** support registration and discovery of arbitrary services
* **Must** provide mechanisms for private / secure service registration and discovery
* **Must** provide simple / human centric approaches for service registration and discovery
* **Should** use small / efficient encodings for use with resource-constrained and embedded system
  * Except where this would impair protocol compatibility or similar
