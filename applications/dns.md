---
description: A DSA based Domain Name Service
---

# DSF-DNS

A DSF application to support registration and addressing of arbitrary services in a distributed and/or federated manner

## Approach

DSF-DNS supports two types of registrations. Primary registration supports the discovery of services by DSF-ID, providing a simple IP address analogue. Secondary registration allows users to designate an authoritative service for a given namespace, and supports the registration and resolving of human-friendly names within this namespace.

### Primary Registration

Registration: 

1. A service is created to use for registration, this service ID becomes the public name for use
2. This service publishes a DSF-DNS Entry as a primary page to it's own address, containing DNS records

Resolution:

1. Other peers can lookup this service by ID, retrieving the associated DNS records

### Secondary Registration

Secondary registration uses the concept of namespaces to support decentralisation of control of DNS, where a given service can control a gTLD such as `.google` or `.facebook`. These are not required to be unique, however, trust in these services must be distributed with the DSF-DNS client or established by the user.

Registration:

1. A service is created to provide an authorative registry for a given namespace
2. This service publishes a DSF-DNS Registrar as a primary page to it's own address, containing it's namespace name and other metadata
3. This service publishes DSF-DNS Entry as a secondary pages to the hash of any name within the namespace

Configuration:

1. A client searches for a DSF-DNS service and marks the service as trusted for a given namespace

Resolution:

1. The client then searches the database for an entry using the hash of the entry name
2. If an entry is found, and the service is trusted, the resulting DNS information is returned

## Options

DSF-DNS adds a set of options corresponding to standard DNS entries

| Name | ID | Description | Multiple? | Size |
| :--- | :--- | :--- | :--- | :--- |

**TODO**


## Service Pages

**TODO**

## See also

- https://en.wikipedia.org/wiki/Domain_Name_System


