# CS 216: Bitcoin Transaction Lab Assignment

This repository contains the implementation of the **Bitcoin Transaction Lab for CS 216**. The project focuses on creating and validating Bitcoin transactions using **Legacy (P2PKH)** and **SegWit (P2SH-P2WPKH)** address formats in a **regtest environment**.

---

# Team Information

* **Shambhavi S Vijay** — 240041034
* **Vaishnavi Ventrapragada** — 240002078
* **Devanshi Mahto** — 240001023
* **Priyanshi Mahto** — 240002057

---

# Project Structure

* **legacy.py**
  Script for **P2PKH transaction workflows (A → B → C)**.

* **segwit.py**
  Script for **P2SH-P2WPKH transaction workflows (A' → B' → C')**.

* **report.pdf**
  Technical report including workflows, TXIDs, decoded scripts, and btcdeb analysis.

* **bitcoin.conf**
  Regtest configuration with specific fee parameters.

---

# Setup & Prerequisites

### 1. Bitcoin Core

Ensure `bitcoind` is running in **regtest mode**.

### 2. Configuration

Use the following mandatory fee settings:

```
paytxfee=0.0001
fallbackfee=0.0002
mintxfee=0.00001
txconfirmtarget=6
```

### 3. Dependencies

Install **python-bitcoinlib** for low-level transaction manipulation.

### 4. Tools

Use **btcdeb** for script debugging and validation.

---

# Execution Instructions

### 1. Start the Daemon

Ensure `bitcoind` is running with the provided `bitcoin.conf` settings.

---

### 2. Run Part 1 — Legacy Transactions

```bash
python Part1_Legacy.py
```

This will output the **TXIDs and raw scripts** for the **A → B** and **B → C** transactions.

---

### 3. Run Part 2 — SegWit Transactions

```bash
python Part2_SegWit.py
```

This will output the **TXIDs and witness data** for the **A' → B'** and **B' → C'** transactions.

---

### 4. Verification

Validation was performed using the **btcdeb debugger** to verify the integrity of the **locking and unlocking mechanisms**.

#### Legacy Transaction Verification

```
./btcdeb '[<signature> <publicKey>] [OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG]'
```

This command verifies that concatenating the **scriptSig** and **scriptPubKey** correctly executes the cryptographic challenge, resulting in a **TRUE stack output**.

The `step` and `stack` commands are used to observe the stack at each step.

---

#### SegWit Transaction Verification

```
./btcdeb '[<signature> <publicKey>] [OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG]'
```

This validates the transition where the transaction commits to a **script hash (P2SH)**, which then triggers the execution of **Segregated Witness data** containing the signatures.

The `step` and `stack` commands are used to observe the stack during execution.

---

# Comparative Analysis

Based on the data gathered during execution:

### Transaction Size

SegWit transactions use **virtual bytes (vbytes)**, resulting in a smaller footprint compared to **Legacy P2PKH transactions**, which are measured in bytes.

### Structure

* **Legacy Transactions:** Signature data is stored within the **scriptSig**.
* **SegWit Transactions:** Signature data is separated into a dedicated **witness field**, reducing fees and preventing **transaction malleability**.

---

# Conclusion

This lab successfully demonstrated the **creation, signing, and broadcasting of transactions** across two different Bitcoin address standards.

By analyzing the scripts using **btcdeb**, we confirmed the correctness of the **stack-based validation mechanism** and observed the **efficiency improvements introduced by SegWit’s transaction structure**. try madi

