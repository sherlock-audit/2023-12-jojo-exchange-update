
# JOJO Exchange Update contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
Arbitrum

___

### Q: Which ERC20 tokens do you expect will interact with the smart contracts? 
any
___

### Q: Which ERC721 tokens do you expect will interact with the smart contracts? 
none
___

### Q: Do you plan to support ERC1155?
none
___

### Q: Which ERC777 tokens do you expect will interact with the smart contracts? 
none
___

### Q: Are there any FEE-ON-TRANSFER tokens interacting with the smart contracts?

None
___

### Q: Are there any REBASING tokens interacting with the smart contracts?

None
___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED?
TRUSTED
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
TRUSTED
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
1. multisig. 2. the owner of JOJODealer, JUSD, DepositStableCoinToDealer, JUSDBank, JUSDExchange, FlashloanRepay, FlashloanLiquidate, GeneralRepay and FundingRateArbitrage.  3. It can set some parameters. 4. Steal the user's fund
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
ERC20 for JUSD
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
1. Centralization Risk: admin have excessive authority, FundingRateKeeper role, JOJO operation role, valid orderSender, emergency oracle owner, insurance account. All have used multi-sig.
2. Incompatibility with Deflationary Tokens: primary asset and secondary asset are standard ERC20 token
3. Low-level Call: About the execute operation of Subaccount.
4. unused contract: in the delist, we will replace the oracle to make the price anchored a fixed price
5. Missing Zero Address Validation
6. Removal of `Perpetual`：It is the normal logic, delist the perpetual.
7. Reliability of Price
8. Third Party Dependencies: chainlink Oracle Failed
9. Potential Reentrancy Attack:
    1. _settle() after `IDealer(owner()).requestLiquidation` 
    2. change `secondaryCredit` after  `IERC20(state.primaryAsset).safeTransfer(to, primaryAmount);` in funding.sol
    3. change secondaryAsset after `IDecimalERC20(_secondaryAsset).decimals()` in Operation.sol
10. Open positions are discarded if `Perpetual` is deregistered: only remove when no position
11. Signature can be replayed: backend will add nonce to the order type.
12. Address Poisoning Attack: caused by the user's wrong copy address and team can not do something for help.
13. DOS attack possible for trading:
    1. only valid msg.sender can call **approveTrade.** If there are too many orders and cannot be matched, we will divide them into multiple transactions.
    2. _realizePnl function loops over an unbounded array within the **`openPositions`**
14. setOperator function is missing onlyOwner modifier: this function is aiming to set every user own operator so no need modifier
15. Collateral Token is standard ERC20
16. FlashloanRepay and FlashloanLiquidate is only to implement the function. We only focus the USDC whether left in this two contract. If users want to implement their own function, the need to write their own contract.
17. JUSD system operates under a cross-margin mode, if the users are not safe, Appreciated collateral can be obtained during the liquidation process.
18. Asset losses of less than 1e-2 due to loss of precision are not considered.
___

### Q: Please provide links to previous audits (if any).
https://github.com/JOJOexchange/smart-contract-EVM/tree/main/audit
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?
JOJO uses a hybrid order book mechanism that combines off-chain matching with on-chain confirmation.  
___

### Q: In case of external protocol integrations, are the risks of external contracts pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality.
Yes, that's acceptable. Currently, we are using Chainlink's price feed to support contract prices. If Chainlink suspends service, we have a backup self-built oracle.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
we expect USDC/USDT/ BTC/WSTETH/ETH/ARB/SOL/LINK
___

### Q: Add links to relevant protocol resources
https://about.jojo.exchange/jojo-1/jojo-exchange/overview
https://jojo-docs.netlify.app/
___



# Audit scope


[smart-contract-EVM @ 4103ea69689b62ea60766314578f410fb364c90f](https://github.com/JOJOexchange/smart-contract-EVM/tree/4103ea69689b62ea60766314578f410fb364c90f)
- [smart-contract-EVM/src/DepositStableCoinToDealer.sol](smart-contract-EVM/src/DepositStableCoinToDealer.sol)
- [smart-contract-EVM/src/FlashLoanLiquidate.sol](smart-contract-EVM/src/FlashLoanLiquidate.sol)
- [smart-contract-EVM/src/FlashLoanRepay.sol](smart-contract-EVM/src/FlashLoanRepay.sol)
- [smart-contract-EVM/src/FundingRateArbitrage.sol](smart-contract-EVM/src/FundingRateArbitrage.sol)
- [smart-contract-EVM/src/FundingRateUpdateLimiter.sol](smart-contract-EVM/src/FundingRateUpdateLimiter.sol)
- [smart-contract-EVM/src/GeneralRepay.sol](smart-contract-EVM/src/GeneralRepay.sol)
- [smart-contract-EVM/src/JOJODealer.sol](smart-contract-EVM/src/JOJODealer.sol)
- [smart-contract-EVM/src/JOJOExternal.sol](smart-contract-EVM/src/JOJOExternal.sol)
- [smart-contract-EVM/src/JOJOOperation.sol](smart-contract-EVM/src/JOJOOperation.sol)
- [smart-contract-EVM/src/JOJOStorage.sol](smart-contract-EVM/src/JOJOStorage.sol)
- [smart-contract-EVM/src/JOJOView.sol](smart-contract-EVM/src/JOJOView.sol)
- [smart-contract-EVM/src/JUSDBank.sol](smart-contract-EVM/src/JUSDBank.sol)
- [smart-contract-EVM/src/JUSDBankStorage.sol](smart-contract-EVM/src/JUSDBankStorage.sol)
- [smart-contract-EVM/src/JUSDExchange.sol](smart-contract-EVM/src/JUSDExchange.sol)
- [smart-contract-EVM/src/JUSDMulticall.sol](smart-contract-EVM/src/JUSDMulticall.sol)
- [smart-contract-EVM/src/JUSDOperation.sol](smart-contract-EVM/src/JUSDOperation.sol)
- [smart-contract-EVM/src/JUSDRepayHelper.sol](smart-contract-EVM/src/JUSDRepayHelper.sol)
- [smart-contract-EVM/src/JUSDView.sol](smart-contract-EVM/src/JUSDView.sol)
- [smart-contract-EVM/src/Perpetual.sol](smart-contract-EVM/src/Perpetual.sol)
- [smart-contract-EVM/src/interfaces/IDealer.sol](smart-contract-EVM/src/interfaces/IDealer.sol)
- [smart-contract-EVM/src/interfaces/IFlashLoanReceive.sol](smart-contract-EVM/src/interfaces/IFlashLoanReceive.sol)
- [smart-contract-EVM/src/interfaces/IJUSDBank.sol](smart-contract-EVM/src/interfaces/IJUSDBank.sol)
- [smart-contract-EVM/src/interfaces/IJUSDExchange.sol](smart-contract-EVM/src/interfaces/IJUSDExchange.sol)
- [smart-contract-EVM/src/interfaces/IPerpetual.sol](smart-contract-EVM/src/interfaces/IPerpetual.sol)
- [smart-contract-EVM/src/interfaces/internal/IChainlink.sol](smart-contract-EVM/src/interfaces/internal/IChainlink.sol)
- [smart-contract-EVM/src/interfaces/internal/IDecimalERC20.sol](smart-contract-EVM/src/interfaces/internal/IDecimalERC20.sol)
- [smart-contract-EVM/src/interfaces/internal/IPriceSource.sol](smart-contract-EVM/src/interfaces/internal/IPriceSource.sol)
- [smart-contract-EVM/src/libraries/EIP712.sol](smart-contract-EVM/src/libraries/EIP712.sol)
- [smart-contract-EVM/src/libraries/Errors.sol](smart-contract-EVM/src/libraries/Errors.sol)
- [smart-contract-EVM/src/libraries/FlashLoanReentrancyGuard.sol](smart-contract-EVM/src/libraries/FlashLoanReentrancyGuard.sol)
- [smart-contract-EVM/src/libraries/Funding.sol](smart-contract-EVM/src/libraries/Funding.sol)
- [smart-contract-EVM/src/libraries/Liquidation.sol](smart-contract-EVM/src/libraries/Liquidation.sol)
- [smart-contract-EVM/src/libraries/Operation.sol](smart-contract-EVM/src/libraries/Operation.sol)
- [smart-contract-EVM/src/libraries/Position.sol](smart-contract-EVM/src/libraries/Position.sol)
- [smart-contract-EVM/src/libraries/SignedDecimalMath.sol](smart-contract-EVM/src/libraries/SignedDecimalMath.sol)
- [smart-contract-EVM/src/libraries/Trading.sol](smart-contract-EVM/src/libraries/Trading.sol)
- [smart-contract-EVM/src/libraries/Types.sol](smart-contract-EVM/src/libraries/Types.sol)
- [smart-contract-EVM/src/oracle/ConstOracle.sol](smart-contract-EVM/src/oracle/ConstOracle.sol)
- [smart-contract-EVM/src/oracle/EmergencyOracle.sol](smart-contract-EVM/src/oracle/EmergencyOracle.sol)
- [smart-contract-EVM/src/oracle/OracleAdaptor.sol](smart-contract-EVM/src/oracle/OracleAdaptor.sol)
- [smart-contract-EVM/src/oracle/OracleAdaptorWstETH.sol](smart-contract-EVM/src/oracle/OracleAdaptorWstETH.sol)
- [smart-contract-EVM/src/oracle/UniswapPriceAdaptor.sol](smart-contract-EVM/src/oracle/UniswapPriceAdaptor.sol)
- [smart-contract-EVM/src/subaccount/BotSubaccount.sol](smart-contract-EVM/src/subaccount/BotSubaccount.sol)
- [smart-contract-EVM/src/subaccount/BotSubaccountFactory.sol](smart-contract-EVM/src/subaccount/BotSubaccountFactory.sol)
- [smart-contract-EVM/src/subaccount/DegenSubaccount.sol](smart-contract-EVM/src/subaccount/DegenSubaccount.sol)
- [smart-contract-EVM/src/subaccount/DegenSubaccountFactory.sol](smart-contract-EVM/src/subaccount/DegenSubaccountFactory.sol)
- [smart-contract-EVM/src/subaccount/Subaccount.sol](smart-contract-EVM/src/subaccount/Subaccount.sol)
- [smart-contract-EVM/src/subaccount/SubaccountFactory.sol](smart-contract-EVM/src/subaccount/SubaccountFactory.sol)
- [smart-contract-EVM/src/token/JUSD.sol](smart-contract-EVM/src/token/JUSD.sol)

