#**CS 216: Bitcoin Transaction Lab Assignment**

This repository contains the implementation of the Bitcoin Transaction Lab for CS 216. The project focuses on creating and validating Bitcoin transactions using Legacy (P2PKH) and SegWit (P2SH-P2WPKH) address formats in a regtest environment.

##**Team Information**
•	Shambhavi S Vijay- 240041034
•	Vaishnavi Ventrapragada- 240002078
•	Devanshi Mahto- 240001023
•	Priyanshi Mahto- 240002057

##**Project Structure**
•	legacy.py: Script for P2PKH transaction workflows (A → B → C).
•	segwit.py: Script for P2SH-P2WPKH transaction workflows (A’ → B’ → C’)
•	report.pdf: Technical report including workflows, txids, decoded scripts, and btcdeb analysis.
•	bitcoin.conf: Regtest configuration with specific fee parameters.

##**Setup & Prerequisites**
1.	Bitcoin Core: Ensure bitcoind is running in regtest mode.
2.	Configuration: Use the mandatory fee settings: 
1.	paytxfee=0.0001 
2.	fallbackfee=0.0002 
3.	mintxfee=0.00001
4.	txconfirmtarget=6.
3.	Dependencies: Requires python-bitcoinlib for low-level transaction manipulation.
4.	Tools: Use btcdeb for script debugging and validation.

##**Execution Instructions**
1.	Start the Daemon: Ensure bitcoind is running with the provided bitcoin.conf settings.
2.	Run Part 1- Legacy: Bash:  python Part1_Legacy.py
This will output the TXIDs and raw scripts for the A→B and B→C transactions.
3.	Run Part 2- Segwit: Bash: python Part2_SegWit.py
This will output the TXIDs and witness data for the A'→B' and B'→C' transactions.
4.	Verification: Validation was performed using the btcdeb debugger to verify the integrity of the locking abd unlocking mechanism, the commands used are stated below:
  1.	For legacy: ./btcdeb '[<signature> <publicKey>] [OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG]'
  Above command is used to verify that concatenating the scriptSig and scriptPubKey correctly executes the cryptographic challenge, resulting in a TRUE stack output. The ‘step’ and ‘stack’ command is used to observe the stack at each step.
  2.	For Segwit: ./btcdeb '[<signature> <publicKey>] [OP_DUP OP_HASH160 <pubKeyHash> OP_EQUALVERIFY OP_CHECKSIG]' 
  Validated the transition where the transaction commits to a script hash (P2SH), which then triggers the execution of the Segregated Witness data containing the signatures. The ‘step’ and ‘stack’ command is used to observe the stack at each step.

##**Comparative Analysis**\n
Based on the data gathered during execution:
•	Transaction Size: SegWit transactions utilized virtual bytes (vbytes), resulting in a smaller footprint compared to Legacy P2PKH bytes.
•	Structure: Legacy transactions store signature data within the scriptSig, whereas SegWit separates this into a dedicated witness field, reducing fees and preventing transaction malleability.

##**Conclusion**\n
This lab successfully demonstrated the creation, signing, and broadcasting of transactions across two different Bitcoin address standards. By analyzing the scripts via btcdeb, we confirmed the security of stack-based validation and quantified the efficiency gains provided by SegWit's restructured data format.

