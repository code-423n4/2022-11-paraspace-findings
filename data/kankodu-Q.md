- Where: [FlashClaimLogic](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L47)

- When a user smart contract calls [executeFlashClaim](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/FlashClaimLogic.sol#L28) the Pool sends them NFTs and then calls `executeOperation` function on the receiver address.
- Usually the receiver smart contract has the check on `executeOperation` to make sure that msg.sender is the pool. Like `OnlyPool` modifier on the sample implementation done by the team. (see [here](https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/flashclaim/AirdropFlashClaimReceiver.sol#L40)) 
- This check is not sufficient as anyone can specify the receiver address and make the receiver contract do unexpected things by specifying malicious `params`. 
- See [here](https://twitter.com/danielvf/status/1580936010556661761) to get an idea of what can happen. 

## Suggestion
- Pass on the initiator address in `executeOperation` as well. see [ERC3156](https://eips.ethereum.org/EIPS/eip-3156#receiver-specification) for reference. 
