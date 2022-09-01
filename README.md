# Defi Borrowing and AAVE
In this lesson, we are going to talk about the fundamentals of Defi borrowing, both from a lending and borrowing perspective. You will also learn about AAVE, one of the top platform that enables lending and borrowing across multiple blockchains, including Ethereum, Polygon and many L2 Platforms.

## How Are Traditional Loans and Defi Loans Different?
To start this lesson, let's begin with a quick breakdown of how a traditional loan works.

### Traditional Loans
Let's say that you're looking to build a small business that requires capital to get it off the ground. You decide to get a modest loan from your local bank and submit and application. 

They will do their own due diligence, such your history of repaying loans, the risk of the venture and other concerns. Once they complete their due diligence (assuming they approve the loan), you will then provide you with an interest rate for the loan, a payment schedule and then ask for collateral on that loan. 

If you default on that loan, the bank can then take that collateral. However, if you pay back the loan in full, plus interest, your loan is paid off and you are free of any obligations.

The process can be demonstrated as such:

![Trad Loan](documents/trad%20loan.drawio.png)

This process should look familiar to anyone who has taken out a car or mortgage loan. In these cases, the car or the house acts as the collateral for the loan, but the process works the same.

### Defi Loans
Defi loans act quite similar to traditional loans in a number of ways:

- Borrowers ask for a loan from a lender
- An interest rate on the loan is assessed
- Collateral for the loan is held for the borrower
- If the loan is not repaid, the collateral is collected by the lender
- If the loan is paid back in full, the collateral does not go back to the lender

However, there are a few crucial difference between a traditional loan and defi loan:

- The platform allowing for the loan is a decentralized, autonomous service, not a trusted third party. Lenders and Borrowers can use the platform to allow for liquidity, but the platform itself is only a facilitator of the transaction.
- Traditional loans can take weeks or months to approve. Defi loans are near instantaneous.
- Traditional loans can take any sort of asset as collateral. Defi loans can only take decentralized assets as collateral. By the same token, defi loans can only loan out decentralized assets.

In practice, let's see how this works.

### Defi Lending on AAVE

Every protocol needs to start with the creation of a pool. This would will allow both lenders and borrowers to share assets in a decentralized, trustless matter. When lending or borrowing on the platform, there will be several key numbers associated with the assets:

- The interest rate to be paid on the loan (Annual Percentage Yield, or APY)
- The borrowing power of the underlying asset (Max LTV)
- The liquidation percentage (or threshold)
- The liquidation penalty

(We haven't discussed that last three points yet, but we will get to it soon.)

Lenders will supply assets onto the platform and get a return on their asset (the APY of the asset). Borrows can then decide they want to borrow against this supply (also for a particular interest rate). In order to borrow, they must do the following:

- Provide at least one type of token as the underlying asset (in this example, let's say we only lend DAI)
- After giving approval and supplying the token to the platform, they will then go to borrow another asset (ETH)
- They can then borrow up to the total borrowing power allowed by their asset
- Once they borrowed, they then have access to the new assets in their wallet and use them as they wish
- Once they are done with the token, they then need to repay the loan on the platform, which will unlock their original collateral and return it to their address.

The process would look something like this:

![Defi Loan](documents/defin%20loan.drawio.png)

On the one side, we have a pure lender (someone who is not borrowing) that is simply there accruing interest.

On the other side, we have a lender who intends to become a borrower. Both the lender and the borrower lend their tokens to the platform.

However, the Borrower then takes an action to borrow funds against the DAI they provided, and takes out a loan for ETH. But then can only borrow up to a percentage of their collateral (For 100% DAI, they may only be able to take out 80%). At this time, their DAI becomes locked in the platform and the ETH becomes available for them to use. The DAI then becomes the collateral.

After some time, the loan needs to be repaid, plus interest. Hopefully whatever activity the borrower had in mind was successful and they are successfully able to repay the loan plus the interest. At that time, the ETH goes back to the total supply of the pool, and the DAI becomes unlocked for the user.

### Collateral Percentage
How much borrowing power do your collateralized assets have? This is decided by the 'Max LTV' of the assets. On AAVE, here is how that information is presented:

![AAVE Loan](./documents/Liquidation%20perc%20and%20max%20ltv.png)

Most defi loans are what you would call *overcollateralized* loans. This means that for every asset you provide as collateral, you can only get a lesser percentage of some other asset. Let's go through an example.

Let's say I have 100 TUSD (another Stable Token) and want to borrow DAI. The Max LTV (borrowing power) of TUSD is 75%, which means we can only borrow up to 75% of the USD value of that asset. So the most I could borrow in DAI is, at most, $75. That's calculate by:

```
CUSD = Total Collateral (in USD)
LTV = Borrowing Power of the Collateral
BUSD = USD value of the borrowed token

(CUSD * LTV) / BUSD = Total Borrowed Tokens
```

For our above transaction, it works out as follows:

```
CUSD = $100
LTV = .75
BUSD = $1 (1 DAI per 1 USD)

($100 * .75) / $1 = 75 DAI
```

As you move to other currencies, the math will work out similarly so that you can only borrow up to the USD value. If ETH is at $2,000 and I want to borrow against my TUSD, the math looks as follows.

```
CUSD = $100
LTV = .75
BUSD = $2000 (1 ETH per $2000 USD)

($100 * .75) / $2000 = .0375 ETH
```

Keep in mind that loans will be accruing interest over time, so the value that needs to be paid back will rise.

### Liquidation Percentage and Penalty
[Loom Video](https://www.loom.com/share/aa59b13f8ab14b17ba502887d1230fee)

So far, we've just talked about full loan repayments without penalties. But there are many cases when a loan is not repaid. Here's how the AAVE platform handles this scenario.

For every token that you borrow against, there are two other numbers that are given:

- Liquidation Threshold
- Liquidation Penalty

To understand how this works, let's go through an example.

Let's say you deposit \$1,000 DAI (LTV of 75%) and thus receive \$750 dollars worth of ETH. Your balance is as follows:

```
DAI = $1000 DAI
ETH = $750 (Debt)
Debt to Collateral Ratio  = 75%
```

The borrowing values for ETH look as follows:

![weth](./documents/Weth%20Liquidation.png)

- Liquidation Threshold - 82.5%
- Liquidation Penalty - 5%

Let's say that the value of your ETH rose from \$750 to \$825, and your DAI collateral had not increased. Your new balance is as follows:

```
DAI = $1000 DAI
ETH = $825 (Debt)
Debt to Collateral Ratio  = 82.5%
```

Your loan is then subject to a liquidation, because your asset is no longer considered properly collateralized. At this point, any actor (now called a liquidator) on the network can choose to liquidate your loan (specifically by calling [`liquidationCall`](https://docs.aave.com/developers/core-contracts/pool#liquidationcall)). Let's say someone then decides to do this. Here's what happens:

1. The liquidator will then repay 50% of the ETH borrow (\$825 * .5 = \$412.5). This 50% is defined by the [AAVE protocol](https://docs.aave.com/faq/liquidations). Once this 50% is repaid, that half of the debt is considered paid for and will no longer be considered your debt.
2. In exchange for paying for 50% of the ETH, the liquidator can now claim 100% of what they repaid, plus a 5% Liquidation penalty (\$412.5 * 1.05 = \$433.13). This gets paid out of the $1,000 worth of DAI used as collateral and will be deducted from your funds.

After this is all said and done, here is what you, the lender, are left with:

```
DAI = $566.87
ETH = $412.5
Debt to Collateral Ratio = 72.77%
```

This liquidation event still leaves you with collateral and an outstanding debt, but you're now under the liquidation threshold of 82.5%. However, you are worse off because you had to pay out additional DAI due to the liquidation, leaving the liquidator with a small profit and you with a loss. 

This is why it's important to never run into a liquidation event if you can avoid it.

## Walkthrough: AAVE Staging Environment
[Loom Video](https://www.loom.com/share/d1423296954b478d9aece0cbe87cf4a7)

To help solidify this understanding, let's go through a walkthrough to actually lend and borrow on the AAVE staging platform.

In this walkthrough, we're going to use the [AAVE staging website](https://staging.aave.com/) to go through the process of lending and borrowing assets. This takes place on the Kovan ETH test network, and has an easy to use faucet to allow for transaction, so no real money will be needed. Please use your test Metamask account for this activity.

In this activity, we will do the following:

- Add test KovanETH to our account
- Use the faucet to provide asset to our test account (three assets in total)
- Lend one asset with the intention of only accruing interest
- Lend one asset with the intention of borrowing
- Borrow another asset, let it accrue interest, then pay it back

### Interesting Architectural Points To Consider
Here are some interesting points you might consider about AAVE as you go through the above walkthrough:

- We have to first approve our tokens before we can supply it. This tells us something about the way AAVE works... that it requires approvals before taking any action with your tokens.
- When you deposit into AAVE as a lender, your assets are automatically collateralized and ready to be used in borrowing.
- The platform is able to not only track how much you have supplied to the platform, but how much yield you accrued during that time. So the platform must have some way of being able to track this so that users are able to get their tokens and interest back. How might this be accomplished?

## Architecture Overview: How Does It Work?
So, this is one of those cases where the underlying logic to make AAVE work is quite complex, often handling a lot of edge cases and gas minimization in order to make the protocol as efficient as possible. 

However, we can look at the activities we did and then find out exactly what handles that logic in order to understand what's going on 'under the hood'. We will just consider supply and withdraws, but you can use the same method to understand any of process that makes AAVE operate.

### Overall Design
The AAVE protocol has two main repositories that handles it's logic. Those include:

- [aave-v3-core](https://github.com/aave/aave-v3-core)
- [aave-v3-periphery](https://github.com/aave/aave-v3-core)

Most of the logic we care about is in `core`, so that's where we will focus our time.

In the `aave-v3-core` repository, there is a folder called [`contract/protocol`](https://github.com/aave/aave-v3-core/tree/master/contracts/protocol), which contains all of the activities we just did in the walkthrough. To help assist your understanding, you can view the [Contract Overview here](https://docs.aave.com/developers/getting-started/contracts-overview).

Overall, AAVE has three main vehicles it uses to maintain borrowing and lending in the AAVE protocol:

- Pools
- ATokens
- DebtTokens

The Pools are the different assets that you can lend and borrow against. New pools get added by the [Pool Address Provider Registry](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/configuration/PoolAddressesProviderRegistry.sol), which allows different types of markets to be added to the contract. 'ETH' is one market. 'DAI' is another. The owner of this contract has the ability to add and remove pools from the AAVE contract.

When you supply a token to the pool, it does more than just keep track of your deposit. You will be minted a new asset called an `AToken`. This is a yield-generating token that is minted and burned when you supply and withdraw your assets from the AAVE pool. 

This is how AAVE keeps track of your owed tokens and yields within the platform. This is treated as a simple EIP20 token, which allows you to do most of the activities you can do with an ERC20 token. Owning this asset is what 'gives' you a yield on the platform. When you burn the asset, you are then given your original asset plus whatever interest you accrued.

`DebtTokens` work in a similar manner, except they are *non-transferable*, mostly-EIP20 compliant tokens that tracks your principle and interest payment. When you burn a debtToken, you need to also provide the subsequent amount of the underlying asset to 'pay off' your debt. There are two variations of `DebtTokens`, stable and variable. Stable tokens have a fixed interest rate associate with them and variable tokens have a variable rate associated with them. We saw this when we looked to borrow a token at a fixed or variable rate:

![variable or stable](./documents/fixed%20vs%20variable%20rate%20loans.png)

In the next part, we will see how AAVE has separated out all of the logic for their contracts in order to make it easier to maintain.

### Walkthrough: Supplying and Withdrawing from a Pool
[Loom Video](https://www.loom.com/share/d83fcefb20324770ad69784c0871af01)

Let's look at what gets called when we supply to a particular pool.

In the staging.aave.com interface, we see that when we hit [this contract endpoint](https://kovan.etherscan.io/address/0xE0fBa4Fc209b4948668006B2bE61711b7f465bAe).

If you look at the code, you will notice that the code is quite minimal, only six files in total:

![AAVE Code](./documents/AAVE%20Source%20Code.png)

This is because the contract is a proxy, which means that the logic of the contract is stored elsewhere. Fortunately, Etherscan has a nice new feature that allows us to read/write to the actual functions:

![AAVE write instruction](./documents/AAVE%20Proxy%20Write%20Instruction.png)

We can also see that because AAVE used the [EIP-1967 transparent proxy pattern](https://eips.ethereum.org/EIPS/eip-1967), we are able to view the real contract [located here](https://kovan.etherscan.io/address/0x2646fcf7f0abb1ff279ed9845ade04019c907ebe#code). This one is 78 files long, so you know we found the right area.

Going back to the proxy, we can see a function called `deposit`, which is what's being called when see click `supply` in the interface:

![Supply on AAVE Interface](./documents/Supply%20on%20AAVE%20interface.png)

If we look at the AAVE v3 Core Repository, we eventually find the `Pool` contract, which indeed has a [supply function](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/Pool.sol#L144). However, this is not where the logic for the `supply` function is stored. Instead, we make a call to an inherited contract called `Supplylogic.executeSupply`.

Let's dive into the logic.

### Supply Logic
By digging further into the repository, we find that all of the 'core' logic that makes up these basic interface tasks are in a repository called [logic](https://github.com/aave/aave-v3-core/tree/master/contracts/protocol/libraries/logic). We can find the specific supply functions we call here: [SupplyLogic.sol contract](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol).

At the top of the contract, we see additional imports, but one that I want to draw your attention to is [IAtoken](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol#L6), which is the *interface* for ATokens. We will see how this is used in a moment.

Let's now look at the [executeSupply function](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol#L52).

(As this file is quite large, it's best to open it in another screen to follow along.)

To start, we see this data in the function:

```
    DataTypes.ReserveData storage reserve = reservesData[params.asset];
    DataTypes.ReserveCache memory reserveCache = reserve.cache();

    reserve.updateState(reserveCache);

    ValidationLogic.validateSupply(reserveCache, params.amount);

    reserve.updateInterestRates(reserveCache, params.asset, params.amount, 0);
```

 Here, the function grabs the `reserveData`, which is the data on the market for the asset provided. This is how the contract knows how much supply is available and then adjusts the interest rates on a variable basis, based on the total reserve amount. Since we are adding to the supply, the interest rate will need to be updated.

After these tasks are done, the collateral asset gets 'locked' and aTokens gets created:

```
    IERC20(params.asset).safeTransferFrom(msg.sender, reserveCache.aTokenAddress, params.amount);

    bool isFirstSupply = IAToken(reserveCache.aTokenAddress).mint(
      msg.sender,
      params.onBehalfOf,
      params.amount,
      reserveCache.nextLiquidityIndex
    );
```

In this transaction, we get the following changes:

- The collateral asset for the supply is being sent to the reserve of other tokens, which is defined by the aTokenAddress
- Then, an aToken is then minted to the sender. This token gets a particular interest rate (`reserveCache.nextLiquidityIndex`), which is associated with the token.

You'll also notice that the function uses a [safeTransferFrom](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol#L67), which is why we needed to give approval to the contract before it could use our tokens.

In the next section, some validation logic is provided, with the user automatically setting the user to use these tokens as collateral:

```
    if (isFirstSupply) {
      if (
        ValidationLogic.validateUseAsCollateral(
          reservesData,
          reservesList,
          userConfig,
          reserveCache.reserveConfiguration
        )
      ) {
        userConfig.setUsingAsCollateral(reserve.id, true);
        emit ReserveUsedAsCollateralEnabled(params.asset, params.onBehalfOf);
      }
    }
```

By the end of this transaction, the user no longer holds their original asset, but instead holds the aToken equivalent, which can then be redeemed in the future.

### Withdraw Logic
[Loom Video](https://www.loom.com/share/464c5d6cfe8046ee8fa594ad35b7ce0d)

Withdrawing works in a similar manner, but there's a slight difference in the way the function transfers tokens.

We can see the [withdraw function in the pool](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/pool/Pool.sol#L197), called `executeWithdraw`, where the user provides their aTokens, and then [burns the aToken](https://github.com/aave/aave-v3-core/blob/master/contracts/protocol/libraries/logic/SupplyLogic.sol#L131) from the total aToken supply. We can also see that the index is updated to re-balance the pool and figure out the next interest rate.

However, we do not see a `safeTransfer` function here, and you won't find it in this contract. This is because the logic to transfer tokens is stored in the aToken itself (`reserveCache.aTokenAddress`). The logic has always been available to the contract, but in order to see it, we have to find the source code. That is stored in a folder called [tokenization](https://github.com/aave/aave-v3-core/tree/master/contracts/protocol/tokenization), where the [aToken functionality](https://github.com/aave/aave-v3-core/blob/1aae831f9c25f6dbfe30770b69fbe74366bdbb32/contracts/protocol/tokenization/AToken.sol#L97) is located.

If you open this file and look at the burn function, you'll see this logic:

```
  function burn(
    address from,
    address receiverOfUnderlying,
    uint256 amount,
    uint256 index
  ) external virtual override onlyPool {
    _burnScaled(from, receiverOfUnderlying, amount, index);
    if (receiverOfUnderlying != address(this)) {
      IERC20(_underlyingAsset).safeTransfer(receiverOfUnderlying, amount);
    }
  }
```

We can then see that the `receiverOfUnderlying` is the address being transferred to. This is the same user who executed the withdraw. So when the token gets burned, the transfer of the underlying asset to the user is done in one single burn transaction. This was likely done at the token level so that no matter what, burning aTokens always in a transfer of assets, with no assets getting locked. There does seem to be a case where if the address itself is executing the burn, assets don't get transferred, likely to help with re-balancing the pool of assets for maintenance.

Through this process, then, we can see how the supply and the withdraws are executed at a foundational level.

## Conclusion
As you can tell, there are a lot of details that go into making this contract work, with a lot of additional levels of complexity related to how the interest rates get set, who gets to set them and how pools operate. If you are planning on moving into defi, I highly encourage you to study this contract further for more insights. However, just this knowledge alone will help you begin to use AAVE borrowing as part of your smart contract development.