# UPI Offline Mesh

A proof-of-concept system that demonstrates how UPI-style payments can be initiated completely offline and propagated through a Bluetooth-style mesh network until a connected device uploads them for settlement.

The project explores a practical solution for digital payments in environments with poor or no internet connectivity, such as underground stations, remote villages, disaster zones, crowded events, and emergency situations.

## Overview

Traditional UPI transactions require both parties to have internet connectivity. This project introduces an alternative approach where payment instructions are encrypted on the sender's device and distributed through nearby devices using a mesh communication model.

Each participating device acts as a relay node. Payment packets hop across devices until a bridge node gains internet access and forwards the transaction to the backend for verification and settlement.

The backend is responsible for:

* Secure decryption of payment instructions
* Duplicate transaction prevention
* Replay attack protection
* Transaction settlement
* Ledger management

The entire workflow can be simulated on a single machine without requiring actual Bluetooth hardware.

---

## Key Features

### Offline Payment Propagation

* Payments can be initiated without internet connectivity.
* Transactions travel across multiple intermediary devices.
* Delivery occurs when any node reaches network connectivity.

### End-to-End Encryption

* RSA-OAEP for key exchange.
* AES-256-GCM for payload encryption.
* Authentication tags prevent packet tampering.

### Idempotent Settlement

* Duplicate packets are detected and rejected.
* Multiple bridge nodes can upload the same transaction safely.
* Guarantees exactly-once settlement.

### Replay Protection

* Time-based freshness validation.
* Unique transaction nonce generation.
* Expired packets are automatically rejected.

### Mesh Network Simulation

* Virtual devices emulate Bluetooth mesh behavior.
* Gossip-based packet propagation.
* Configurable hop count and packet TTL.

### Secure Transaction Processing

* Atomic account debit and credit operations.
* Optimistic locking using JPA versioning.
* Transaction ledger persistence.

---

## System Architecture

Sender Device

↓

Encrypted Payment Packet

↓

Mesh Network Propagation

↓

Bridge Device

↓

Spring Boot Backend

↓

Validation & Decryption

↓

Duplicate Detection

↓

Settlement Engine

↓

Transaction Ledger

---

## Technology Stack

### Backend

* Java 17
* Spring Boot 3
* Spring Data JPA
* Spring Web
* H2 Database

### Security

* RSA-2048
* RSA-OAEP
* AES-256-GCM
* SHA-256

### Concurrency

* ConcurrentHashMap
* Optimistic Locking
* Transactional Processing

### Testing

* JUnit
* Spring Boot Test

---

## Core Components

### Mesh Simulator

Simulates a decentralized Bluetooth-style network consisting of multiple virtual devices. Packets are propagated using a gossip-based communication model.

### Bridge Ingestion Service

Acts as the entry point for online bridge nodes. Responsible for validation, decryption, freshness verification, and settlement orchestration.

### Hybrid Crypto Service

Provides encryption and decryption functionality using a combination of RSA and AES cryptography.

### Idempotency Service

Prevents duplicate transaction processing through packet fingerprinting and atomic claim operations.

### Settlement Service

Performs transactional account updates and records settlement information in the ledger.

---

## Security Considerations

The system addresses several important challenges associated with offline financial transactions:

### Duplicate Delivery

Multiple bridge nodes may upload the same packet simultaneously. Duplicate settlements are prevented using packet hashing and idempotency checks.

### Packet Tampering

AES-GCM authentication tags ensure that any modification to the encrypted payload results in rejection.

### Replay Attacks

Transactions include timestamps and unique nonces that prevent previously captured packets from being reused.

### Concurrent Processing

Optimistic locking and transactional settlement logic ensure data consistency under concurrent uploads.

---

## Demonstrated Use Cases

* Rural digital payments
* Disaster recovery scenarios
* Emergency communication environments
* Underground transportation systems
* Large-scale public events
* Low-connectivity regions

---

## Future Enhancements

* Real Bluetooth Low Energy (BLE) communication
* Redis-based distributed idempotency layer
* PostgreSQL persistence
* Digital signature verification
* Multi-region deployment
* Mobile Android implementation
* Real-time monitoring dashboard
* Integration with payment networks

---


