# Blockchain ✨

![forthebadge](https://forthebadge.com/images/badges/built-with-love.svg)
![forthebadge](https://forthebadge.com/images/badges/for-you.svg)
![forthebadge](https://forthebadge.com/images/badges/powered-by-coffee.svg)

![](https://img.shields.io/badge/Excitement-High-red)
![](https://img.shields.io/badge/Maintained-Yes-indigo)
![](https://img.shields.io/badge/Pull_Requests-Accepting-yellow)
![](https://img.shields.io/github/forks/krvaibhaw/blockchain)
![](https://img.shields.io/github/contributors/krvaibhaw/blockchain)
![](https://img.shields.io/github/issues/krvaibhaw/blockchain)
![](https://img.shields.io/github/stars/krvaibhaw/blockchain)

![](https://img.shields.io/badge/Contributions-Accepting-pink)
![](https://img.shields.io/github/license/krvaibhaw/blockchain)
[![](https://img.shields.io/badge/By_Me_A_Coffee-Paypal-skyblue)](https://www.paypal.com/paypalme/krvaibhaw/100)

![](https://img.shields.io/badge/Python-blue)
![](https://img.shields.io/badge/HTML-blueviolet)



# Learn Blockchains by Building One Yourself


<p align="center">
<img src="/preview/preview.png">
</p>


## Installation

1. Make sure [Python 3.6+](https://www.python.org/downloads/) is installed.
2. Install [Flask Web Framework](https://flask.palletsprojects.com/en/2.0.x/).
3. Clone this repository  
```
    $ git clone https://github.com/krvaibhaw/blockchain.git
```
4. Change Directory
```
    $ cd blockchain
``` 
5. Install requirements  
```
    $ pip install requirements.txt
``` 
6. Run the server:
```
    $ python blockchain.py 
```
7. Head to the Web browser and visit
```
    http://127.0.0.1:5000/
```

## Introduction

Blockchain is a specific type of database. It differs from a typical database in the way it stores information; blockchains store data in blocks that are then chained together. As new data comes in it is entered into a fresh block. Once the block is filled with data it is chained onto the previous block, which makes the data chained together in chronological order. Different types of information can be stored on a blockchain but the most common use so far has been as a ledger for transactions. 


## What is Blockchain?

A blockchain is essentially a digital ledger of transactions that is duplicated and distributed across the entire network of computer systems on the blockchain. It is a growing list of records, called blocks, that are linked together using cryptography. Each block contains a cryptographic hash of the previous block, a timestamp, and transaction data (generally represented as a Merkle tree). The timestamp proves that the transaction data existed when the block was published in order to get into its hash.

As blocks each contain information about the block previous to it (by cryptographic hash of the previous block), they form a chain, with each additional block reinforcing the ones before it. Therefore, blockchains are resistant to modification of their data because once recorded, the data in any given block cannot be altered retroactively without altering all subsequent blocks.

## How does it works?

Blockchains are typically managed by a peer-to-peer network for use as a publicly distributed ledger, where nodes collectively adhere to a protocol to communicate and validate new blocks. Although blockchain records are not unalterable as forks are possible, blockchains may be considered secure by design and exemplify a distributed computing system with high Byzantine fault tolerance.

## Why Blockchain?

* **Immutable :** Blockchains are resistant to modification of their data because once recorded, the data in any given block cannot be altered retroactively without altering all subsequent blocks.

* **Decentralized :** It doesn’t have any governing authority or a single person looking after the framework. Rather a group of nodes maintains the network making it decentralized. It means :
        
        -> Transparency
        -> User Control
        -> Less Prone to Breakdown
        -> Less chance of Failure.
        -> No Third-Party


* **Enhanced Security :** If someone wants to corrupt the network, he/she would have to alter every data stored on every node in the network. There could be millions and millions of people, where everyone has the same copy of the ledger.

* **Distributed Ledgers :** The ledger on the network is maintained by all other users on the system. This distributed computational power across the computers to ensure a better outcome. It ensures : 
    
        -> No Malicious Changes
        -> Ownership of Verification
        -> Quick Response
        -> Managership
        -> No Extra Favors

* **Consensus :** The architecture is cleverly designed, and consensus algorithms are at the core of this architecture. The consensus is a decision-making process for the group of nodes active on the network. The consensus is responsible for the network being trustless. Nodes might not trust each other, but they can trust the algorithms that run at the core of it. That’s why every decision on the network is a winning scenario for the blockchain.

* **True Traceability :** With blockchain, the supply chain becomes more transparent than ever, as compared to traditional supply chain, where it is hard to trace items that can lead to multiple problems, including theft, counterfeit, and loss of goods.

## Understanding the Program

Firstly, we defined the structure of our block, which contains, block index, timestamp of when it has been created, proof of work, along with previous hash i.e., the hash of previous block. In real case seanario along with these there are other contents such as a body or transaction list, etc.

```python
    def createblock(self, proof, prevhash):
        
        # Defining the structure of our block
        block = {'index': len(self.chain) + 1,
                 'timestamp': str(datetime.datetime.now()),
                 'proof': proof,
                 'prevhash': prevhash}

        # Establishing a cryptographic link
        self.chain.append(block)
        return block
```

The genesis block is the first block in any blockchain-based protocol. It is the basis on which additional blocks are added to form a chain of blocks, hence the term blockchain. This block is sometimes referred to Block 0. Every block in a blockchain stores a reference to the previous block. In the case of Genesis Block, there is no previous block for reference.

```python
    def __init__(self):
        
        self.chain = []
        
        # Creating the Genesis Block
        self.createblock(proof = 1, prevhash = "0")
```

Proof of Work (PoW) is the original consensus algorithm in a blockchain network. The algorithm is used to confirm the transaction and creates a new block to the chain. In this algorithm, minors (a group of people) compete against each other to complete the transaction on the network. The process of competing against each other is called mining. As soon as miners successfully created a valid block, he gets rewarded.

```python
    def proofofwork(self, prevproof):
        newproof = 1
        checkproof = False

        # Defining crypto puzzle for the miners and iterating until able to mine it 
        while checkproof is False:
            op = hashlib.sha256(str(newproof**2 - prevproof**5).encode()).hexdigest()
            
            if op[:5] == "00000":
                checkproof = True
            else:
                newproof += 1
        
        return newproof
```

Chain validation is an important part of the blockchain, it is used to validate weather tha blockchain is valid or not. There are two checks performed:

* **First check :** For each block check if the previous hash field is equal to the hash of the previous block i.e. to verify the cryptographic link. 

* **Second check :** To check if the proof of work for each block is valid according to problem defined in proofofwork( ) function i.e. to check if the correct block is mined or not.

```python
    def ischainvalid(self, chain):
        prevblock = chain[0]   # Initilized to Genesis block
        blockindex = 1         # Initilized to Next block

        while blockindex < len(chain):

            # First Check : To verify the cryptographic link
            
            currentblock = chain[blockindex]
            if currentblock['prevhash'] != self.hash(prevblock):
                return False

            # Second Check : To check if the correct block is mined or not

            prevproof = prevblock['proof']
            currentproof = currentblock['proof']
            op = hashlib.sha256(str(currentproof**2 - prevproof**5).encode()).hexdigest()
            
            if op[:5] != "00000":
                return True

            prevblock = currentblock
            blockindex += 1

        return True
```

<br>
Feel free to follow along the code provided along with mentioned comments for 
<br>better understanding of the project, if any issues feel free to reach me out.

## Contributing

Contributions are welcome!
<br>Please feel free to submit a Pull Request.
