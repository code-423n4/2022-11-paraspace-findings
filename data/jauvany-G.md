EVENT IN APESTAKINGLOGIC.SOL IS NOT INDEXED

IMPACT
An Event in the APESTAKINGLOGIC.SOL contract is not indexed, making off-chain scripts such as front-ends of dApps difficult to filter this event efficiently.

PROOF OF CONCEPT

REFERENCED CODE:
ApeStakingLogic.sol #L29 link: https://rebrand.ly/983b57

RECOMMENDED MITIGATION STEPS
Add the indexed keyword to the event. For example, event ProposalExecuted(uint256 indexed id);