---
description: A DSA based application for the Internet of Things (IoT)
---

# DSD-IoT

A DSD application to support IoT devices and data sharing.

## Options

DSD-IoT adds a set of options for endpoint description.

| Name | ID | Description | Multiple? | Size |
| :--- | :--- | :--- | :--- | :--- |
| [Unit](iot.md#unit) | 0x8001 | (IoT) endpoint unit | Yes | Variable |


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
|          Endpoint ID          |           Data Type           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
/                                                               /
/                           Data Value                          /
/             Variable Length Data (32-bit aligned)             /
/                                                               /
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

### Data Types

| Name | ID | Description | Size |
| :--- | :--- | :--- | :--- |
| Bool    | 0x0001 | Boolean value (encoded in top bit) | 0 byte |
| Int32   | 0x0002 | 32-bit integer value               | 4 byte |
| Float32 | 0x0003 | 32-bit floating point value        | 4 byte |
| Float64 | 0x0004 | 64-bit floating point value        | 8 byte |
