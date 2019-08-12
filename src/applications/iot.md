---
description: A DSA based application for the Internet of Things (IoT)
---

# IoT

A DSF application to support IoT devices and data sharing.

## Endpoint Options

DSF-IoT adds a set of options for endpoint description.

| Name | ID | Description | Multiple? | Size |
| :--- | :--- | :--- | :--- | :--- |
| Unit | 0x8001 | \(IoT\) Endpoint Unit | Yes\(\*\) | Variable |
| Kind | 0x8002 | \(IoT\) Endpoint Kind | Yes\(\*\) | Variable |
| Name | 0x8003 | \(IoT\) Endpoint Name | Yes\(\*\) | Variable |

 \* One option per endpoint entry

## Service Pages

IoT service pages are a primary page where data is a list of endpoints, ordered by ID, and each endpoint consists of at least a Name, a Kind, and a Unit option.

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Endpoint ID          |        Endpoint Length        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                        Endpoint Options                       /
/             Variable Length Data (32-bit aligned)             /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Data Blocks

Endpoint data is a set of data objects corresponding to each endpoint, ordered by endpoint ID.

### Data Objects

```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Endpoint ID          |          Value Type           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                             Value                             /
/             Variable Length Data (32-bit aligned)             /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Value Types

| Name | ID | Description | Size |
| :--- | :--- | :--- | :--- |
| Bool | 0x0001 | Boolean value \(encoded in top bit\) | 0 byte |
| Int32 | 0x0002 | 32-bit integer value | 4 byte |
| Float32 | 0x0003 | 32-bit floating point value | 4 byte |
| Float64 | 0x0004 | 64-bit floating point value | 8 byte |

