# Title

## Summary

[summary]: #summary

CEP PR: [casperlabs/ceps#0000](https://github.com/casperlabs/ceps/pull/0000)

This CEP is intended to help guide the development of network support for so-called Non-fungible Tokens (NFTs). It is meant to define a standardized interface for smart contracts to be able to generate, transfer and otherwise manage NFT assets.

## Motivation

[motivation]: #motivation

NFTs represent a rapidly evolving and expanding blockchain industry segment. They have seen widespread adoption in areas such as digital artwork and collectibles, but have applicability in use cases ranging from representation of hard assets such as real estate, to ownership of intellectual property. The landscape of potential use-cases is assumed to continue growing.  

In contrast to native currency tokens (e.g. CSPR as defined by [CEP-0014](https://github.com/casper-network/ceps/blob/master/text/0014-native-currency.md)), or custom token standards (e.g. [CEP-0018](https://github.com/zie1ony/ceps/blob/token/text/0018-token-standard.md)), NFTs by concept are intended to represent assets that are each unique and distinct. They therefore require API functionality beyond that of the native or even custom token standards.

## Guide-level explanation

[guide-level-explanation]: #guide-level-explanation

While this CEP is not intended to be a full technical specification, we will outline here the envisioned core functionalities supporting a generalized and extensible NFT capability.

### Basic concepts

#### Asset Ownership
Core to the NFT functionality is an effective implementation of asset ownership. Again, in contrast to native currency tokens, where 'movement' of tokens from one account to another is mechanically a recalculation of balance amounts between accounts, a NFT must explicitly carry with it a representation of its current 'owner', in a manner that can be updated and queried reliably. Exclusive vs. collective or frational ownership must also be taken into account. 

#### Asset Definition
The asset a NFT claims ownership of must be defined in a standardized manner. The specification must be broad enough to encompass a range of digital and non-digital assets, while specific enough to achieve consistency in implementation.   

#### Ownership Rights
The rights, if any, that are bestowed to the NFT owner must be defined. The scope of use (domains, limitations, exclusions, etc) that are granted as a result of the asset ownership must also be defined. **TBD**

#### Asset Transfer
At a minimum, a standardized NFT interface must provide mechanisms for the legitimate owner to initiate or authorize the transfer of the asset to a new owner.

#### Token Creation & Destruction
The NFT standard must provision for the creation (minting) of NFTs that are uniquely identifiable. It must also establish the mechanism by which they might later be destroyed. 

## Reference-level explanation

[reference-level-explanation]: #reference-level-explanation

### Interface
The token contract should implement following methods:

#### tokenID
returns the token ID

```rust
fn tokenID() ->  URef
```
**TBC:** Best uuid implementation?

#### owner
returns the token owner(s)

```rust
fn owner() -> Address
```

#### transfer
Transfer tokens from the direct function caller to the `recipient`.
```rust
fn transfer(recipient: Address, id: URef)

/// caller must be the current owner
```

#### approve
Allow other address to transfer caller's tokens.

```rust
fn approve(spender: Address, id: URef)
```

#### transfer_from
Transfer tokens from `onwer` address to the `recipient` address if required
amount was approved before to be spend by the direct caller.
```rust
fn transfer_from(owner: Address, recipient: Address, id: URef)
```

## Drawbacks

[drawbacks]: #drawbacks

The ecosystem is still actively engaging with NFT protocols developed on other networks, and implementing a NFT standard immediately may miss opportunities to leverage from the feedback and learning experiences of those projects.

## Rationale and alternatives

[rationale-and-alternatives]: #rationale-and-alternatives

### Rationale
- Current design meets the existing industry standards in a reliable way, and allows for future iterative improvements
- It provides functionality deemed a requirement for a range of active use-cases.

Alternatives would include:
- Do not define a standard and wait for additional market and community feedback

## Prior art

[prior-art]: #prior-art

The CEP leverages the design utilized by Ethereum in its [ERC-20](https://eips.ethereum.org/EIPS/eip-20) and [ERC-721](https://eips.ethereum.org/EIPS/eip-721) standards. However...

- TBC:
  - URef vs u512 id implementation.
  - Account vs Purse model?
  - Underlying asset schema? Centralized vs decentralized URI designation possible?
  - See CEP-0018 discussion of ERC-20 [Approve and Call Problem](https://github.com/zie1ony/ceps/blob/token/text/0018-token-standard.md#approve-and-call-problem), and the differing approach allowed by Casper infrastructure. 

## Unresolved questions

[unresolved-questions]: #unresolved-questions

- Inherit Purse vs. Account model TBC from [CEP-0018](https://github.com/zie1ony/ceps/blob/token/text/0018-token-standard.md#rationale)?
- Support for 'representative' management of NFTs
- Fractional (multi-party) ownership support
- Precise 'scope' of ownership rights
  - to what degree is this implemented into contract standard?
- Required asset metadata?
  - Robust way to verify and record on-chain whether URI is http vs IPFS?

### Out of Scope (Current Iteration)

- NFT Auctions
- NFT leasing / licensing

## Future possibilities

[future-possibilities]: #future-possibilities

- Pending discussion of Unresolved / Out of Scope...
