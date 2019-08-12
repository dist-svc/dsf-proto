# Low Level Operations

This page describes low-level operations that must be implemented by DSF clients to support the defined higher level operations.

## Messaging

### Receiving a message

1. Locate the public key associated with the peer \(from the database or local cache\)
2. Validate the message signature against discovered ID
3. Add public key to local cache
4. Handle message and reply

### Sending a message

## Storing and retrieving pages

### Storing a Primary Page

1. Locate the public key associated with the page ID \(included in page, from the database, from the local cache\)
2. Validate the ID matches the public key
3. Validate the signature
4. Parse the options
5. Check required options exist
6. Store page

### Storing a Secondary Page

1. Find the public key associated with the peer ID \(from the database or local cache\)

### Receiving a \(Requested\) Page

1. Find the public key associated with the page ID \(included in page, from the database, from the local cache\)
2. Validate the message signature against discovered ID
3. Add public key to local cache
4. Return page

## Requesting and Sending Data

## Handling failures

On a signature verification failure

* increment peer failure count
* if failure count &gt; max failures, add ID / Address to block list

