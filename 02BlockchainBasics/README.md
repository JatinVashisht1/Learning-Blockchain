## Before we proceed
- this is not a very detailed explaination of blockchain. This is a high level view about blockchain.
- This is based upon the **blockchain** section of **solidity** docs.
    - click [here](https://docs.soliditylang.org/en/v0.8.15/introduction-to-smart-contracts.html) to goto the original page

## Blockchain 
- Blockchain as a topic is not very hard topic to understand, other complex topics such as mining, hashing, etc. are there to provide features.
    - Once you approach them as features or assets that will eventually make your code/program more secure and functional then you won't fear from them, it could also be possible that you study about them with more interest if you know which topic will help you in which type of conditions.
- Moreover you don't have to study them in very much depth also you just should know how to use them in which condition.

### Transaction

Blockchain is a globally shared database, which means anyone can read the entries just by participating in the network (don't worry about the privacy of data as of now).
When you want to change something then also you have to get approval from all the other nodes of the network.

- To make the change we have to create something called **transaction**, and get it accepted by others.
- Transaction makes sure that the change you want to apply is either applied fully or is not applied at all.
- While a transaction is being applied not other transaction can alter it.

For example if person A is sending money to account of Person B, then transactional nature of database makes sure that amount from Person A is subtracted and is added to account of Person B. If for any reason it is not able to complete it, all the changes made will undo.

- A transaction is always cryptographically signed by the sender, which guards it from any alteration.

### Block
- One problem that you can think of in a blockchain network is, "what is two transactions request to do the same work that can be done once?", for example, a request to empty the account is made by two transactions. But this action can only be performed once.
    - This is actually called `double-spend attack`.
- This problem (at higher level) is solved by a globally accepted order of selection. This make sure that this case does not occur.
- These transactions are then bundled in a so called `block` of transactions.
    - If two transactions contradict each other then the transaction which is identified second by the global order selection mechanism will be rejected.
- These blocks form a linear sequence in time, you can visualise it as a chain of blocks, this is where the word `blockchain` came from.
- Due to the order selection mechanism also called `mining`, it might be possible that a block is reverted time to time, but it is more likely to happen at the tip of the chain.
    - More number of blocks added on top of a block less the chance of its getting reverted.
- This means that more time you wait more the chances of the block to get accepted.
---
### Note
<p>
Transactions are not guaranteed to be included in the next block or any specific future block, since it is not up to the submitter of a transaction, but up to the miners to determine in which block the transaction is included.
</p>

---