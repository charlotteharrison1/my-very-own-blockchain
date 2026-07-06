following this fun tutorial - https://hackernoon.com/learn-blockchains-by-building-one-117428612f46



blockchain is an immutable, sequential chain of records called `blocks`

the blocks contain transactions, files, or other data

they're chained together using *hashes*.

### *Sidebar: What's a hash function?*

A hash function is a deterministic function. They are generally irreversible (so you can't figure out the input if you only know the output)

It takes in an input and outputs a hash
A hash is usually displayed as a hexidecimal number

Hash function can be used for proving that something is the same as something else, without revealing the information beforehand - i.e. two people with the same result can prove the result is the same by showing that it produces the same hash, but you cannot simply look at the hash without hashing your original result. Hashes are therefore often used to verify information without revealing the answer to the party that needs verification!

# What does a block look like?
Simple example of a single block:
```
block = {
    'index': 1,
    'timestamp': 1506057125.900785,
    'transactions': [
        {
            'sender': "8527147fe1f5426f9dd545de4b27ee00",
            'recipient': "a77f5cdfa2934df3954a5c7c7da5df1f",
            'amount': 5,
        }
    ],
    'proof': 324984774000,
    'previous_hash': "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824"
}
```

From this we see the concept of a chain; each new block contains the hash of the previous block within itself.


This is what allows blockchains to be *immutable*; if a hacker corrupts an earlier block in the chain then all subsequent blocks will contain incorrect hashes.


Add transcations to the block using `new_transaction()` method.


# What is proof of work?
Proof of work is how new blocks are created or mined on the blockchain. 

The goal of PoW is to discover a number which solves a problem. The number must be difficult to find but easy to verify by anyone on the network. This is the core idea behind Proof of Work.


Example:
Decide that the hash of some integer x multiplied by another y must end in 0
So 
```
hash(x*y) = ac23dc....0
```

Fix x=5

Implementing in Python:
```python
from hashlib import sha256
x = 5
y = 0  # We don't know what y should be yet...
while sha256(f'{x*y}'.encode()).hexdigest()[-1] != "0":
    y += 1
print(f'The solution is y = {y}')
```
The solution is y=21

*The proof of work algorithm is called hashcash*

Hashcash is the algorithm used by miners in order to create a new block.
The difficulty is generally determined by the number of characters searched for in a string.
The miners are rewarded for their solution by receiving a coin.

# Our blockchain as an API
We will use Python Flask, which is a micro-framework which makes it easy to map endpoints to Python functions. This allows us to interact with the blockchain over the web using HTTP requests

We will create three methods"
```
/transactions/new <-- creates a new transaction to a block
```
```
/mine <-- tells our servers to mine a new block
```
```
/chain <-- returns the full blockchain
```

## Setting up Flask
our server is a single node in the blockchain network