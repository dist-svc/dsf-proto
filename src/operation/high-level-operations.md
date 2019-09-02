# High Level Operations

This section covers the high-level processes for interacting with DSF, this is supported by lower level operations that enable storage and querying using the DHT and subscription and publication of data to services.
It is intended this will be expanded to become the source of truth for all DSF operations, however, currently this documentation lags [dsf-impl]().

## Connecting to the network

1. Generate a new keypair and DatabaseID for the new node \(this may be persisted as desired\)
2. Connect to a "bootstrap" node \(address only\) that is already a member of the network
3. Generate a new Primary page containing the node information
4. Publish the new primary page to DSF at the Node DatabaseID

## Publishing a service

1. Generate a new keypair and Database ID for the new service \(this should be stored\)
2. Generate a new Primary page containing the service information
3. Publish the new primary page to DSF at the Service DatabaseID

## Updating a service

1. Update the service information / definition
2. Generate an updated Primary page containing the service updated information
3. Publish the updated primary page to DSF at the Service DatabaseID

## Locating a service

1. Search for pages at a given ID
2. Parse and Reduce returned pages (one primary, N secondary)
3. Check Key / Signature against pinned values (if available)
4. Return service

## Subscribing to a service

1. Search for pages at a given ID
2. Parse and Reduce returned pages (filter for replicas)
3. Select a subset of replicas based on QoS / availability
4. Issue SubscribeRequest messages to replica subset to request future updates

## Querying for service data

1. Search for pages at a given ID
2. Parse and Reduce returned pages (filter for replicas)
3. Select a subset of replicas based on QoS / availability
4. Issue Query messages to replica subset to request data

