# Transcendent Bloom
![transcendent bloom](https://github.com/0xambr0sia/transcendentbloom/assets/147129231/9204206a-3dda-4366-8bd4-016f018cb7d2)


A performance of two jellies - inspired by natural processes of moon and ghost jellies.

_A singular Jelly token autonomously mints a counterpart, and both hold the potential for a lifelong connection. Within 24 hours, the original owner must transfer one of the tokens to a chosen partner. If successful, their destinies intertwine, but any attempt to transfer ownership after this period triggers an automatic burn. In this digital ballet of love, the unchosen token gains the power to respawn, emerging as a Ghost Jelly—a spectral testament to the transient nature of opportunities in the decentralized sea._

Let's break down how the minting, regeneration, and special token behaviors work in the provided `Jellies` contract:

### Minting (`mint` Function):

1. **Marketplace Setup:**
   - Before minting, the contract must have a valid marketplace address set using the `setMarketplace` function.

2. **Initial Token URI Setup:**
   - The initial token URI is set using the `setInitialTokenURI` function.

3. **Minting Function (`mint`):**
   - The `mint` function is an administrative function (`adminRequired` modifier) that allows the contract owner to mint tokens.
   - It checks whether a marketplace address is set (`_marketplace` must not be the zero address) and ensures that minting has not occurred before (`_hasMinted` must be false).
   - After these checks, the `_mint` function is called to mint a new token with ID 1 and assign it to the caller (contract owner).

### Regeneration (`createNew` Function):

1. **Unbound and Respawn Checks:**
   - Before regeneration, the contract checks that regeneration is allowed (`_hasUnbound` must be true, and `_hasRespawned` must be false).
   - Only certain addresses (owners of tokens with IDs 1 and 2) are allowed to trigger regeneration using the `createNew` function.

2. **Minting Ghost Token:**
   - When regeneration occurs, a new token with ID 4 is minted and assigned to the specified receiver address (`_mint(receiver, 4)`).
   - The `_hasRespawned` flag is set to true, indicating that regeneration has occurred.

### Special Token Behaviors (Time and Ownership):

1. **Locked Time:**
   - The contract has a mechanism based on time (`_lockedTime`) that triggers specific actions.
   - If `_lockedTime` is set, it means that a time-based event is pending.
   - When the specified time (`_lockedTime`) is reached, and `_hasUnbound` is false, the contract performs the following actions:
     - Burns token ID 1 (if the recipient is not the zero address).
     - Mints a ghost token with ID 3 and assigns it to the original owner of token ID 1.

2. **Paired Token Minting:**
   - When tokens are transferred from the marketplace (`_marketplace`) and token ID 2 does not exist, a new token with ID 2 is minted and assigned to the recipient.
   - `_lockedTime` is set to the current block timestamp plus 86400 seconds (24 hours) to trigger the time-based event.

### Royalties:

1. **Royalty Setup and Updates:**
   - The contract supports royalties on token transfers.
   - The `updateRoyalties` function allows the contract owner to update the royalty recipient and basis points.

2. **Royalty Queries:**
   - Functions like `getRoyalties`, `getFeeRecipients`, `getFeeBps`, and `royaltyInfo` provide information about royalties.

### Base URIs:

1. **Base URI Functions:**
   - The contract allows the owner to set base URIs for different types of tokens using functions like `setInitialTokenURI`, `setSplitTokenURI`, and `setGhostURI`.
   - The `tokenURI` function uses these base URIs to construct the complete URI for each token.

In summary, the contract provides flexibility in minting, regeneration, and special token behaviors based on time and ownership. It also supports royalties and allows the owner to customize token metadata URIs. The functions are designed to be called by the contract owner and specific addresses, ensuring controlled access to critical functionalities.
