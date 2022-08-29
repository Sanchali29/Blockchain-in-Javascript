# Building my own Blockchain in Javascript
With all the hype about blockchain and cryptocurrencies, I decided to learn more about it. Now what is thte better way to learn than to try to build it yourself?
I have tried to mention all the steps involved in building blockchain from basic principles. 

## Step 1: What exactly is Blockchain?

Let us see what blockchain means, the fundamentals of blockchain technology are straightforward. Any given blockchain comprises a single chain of discrete blocks of information, arranged chronologically.
A common misconception is that a blockchain is a single chain of blocks, when in reality, it's more like a tree of blocks. So at any given time, there are multiple chains for blocks by pointing to their respective parent. The pointing happens via hashes which are calculated based upon the data inside the block (i.e. hash of the parent, transaction data and other important stuff)

By pointing via hashes of blocks, we can enforce a specific order of blocks. I.e given a chain of blocks, you can't just take a block in the middle of the chain and change its data, since that would change its hash and subsequently also all blocks that are descendents of the block in question.

```javascript
class Block {
  constructor(blockchain, parentHash, nonce = sha256(new Date().getTime().toString()).toString()) {
    this.blockchain = blockchain;
    this.nonce = nonce;
    this.parentHash = parentHash;
    this.hash = sha256(this.nonce + this.parentHash).toString()
  }
```
By definition, THE blockchain is just the longest chain available in the tree. So at one point, a chain can be the longest one, but then get superseeded by another. Let's visualize the longest chain in the tree.

```javascript
class Blockchain {
  longestChain() {
    const blocks = values(this.blocks)
    const maxByHeight = maxBy(prop('height'))
    const maxHeightBlock = reduce(maxByHeight, blocks[0], blocks)
    const getParent = (x) => {
      if (x === undefined) {
        return false
      }

      return [x, this.blocks[x.parentHash]]
    }
    return reverse(unfold(getParent, maxHeightBlock))
  }
}
```
