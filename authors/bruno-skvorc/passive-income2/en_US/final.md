In the [previous post](https://bitfalls.com/2019/01/18/make-your-crypto-work-for-you-how-to-create-dai/) we learned how to generate the stablecoin DAI from our ether. This is useful if you have a lot of ether lying around but would still like to be able to speculate with its value without actually selling it.

In this post we'll learn how to use the [Compound Finance](https://compound.finance) platform to loan out our DAI and gather interest on these loans - automatically and safely.

## Compound Interest

In finance, compound interest is interest that grows on the previously grown interest. A banal example is: you have 1000 USD which you invest at 10% returns per year. One year later you have 1100 USD for 100 USD interest.

Compound interest comes into play when you don't pull out the 100 USD but instead reinvest it all. That way, next year you'll have 1210 instead of 1200 - you got an extra 10 USD because you compounded the interest.

This doesn't seem like much, but apply it to a non-trivial amount of money for a non-trivial amount of time, and you join [the ranks of kings](https://www.marketwatch.com/story/this-warren-buffett-rule-can-work-wonders-on-your-portfolio-2016-04-26).

For example, $100,000 invested at 5% compounded annually nets you $62000 of interest after 10 years. Hardly revolutionary. But let that sit until retirement, and in 40 years you have $600,000. Add another 10 years and it magically turns into over 1 million USD.

A good idea is putting a significant chunk of your paycheck into such a savings mode so that it continuously increases. This becomes a much more reliable retirement strategy than a state pension because it both compounds and increases principal continuously.

![Warren Buffet](https://bitfalls.com/wp-content/uploads/2019/01/01.png)

## Compound Finance

Compound Finance is a collection of smart contracts and the user interfaces to use them. These contracts let you lend out or borrow cryptocurrency in exchange for collateral - also in cryptocurrency.

Much like the MakerDAO CDP, you put down collateral in one cryptocurrency and get a loan in another. The big difference is that you don't _create_ a currency when taking a loan here - instead, you borrow from an existing supply provided by others. The smaller that supply is, the bigger the interest rate.

Let's dive right in and learn how to make our crypto earn us some money by earning compound interest on the amounts we loan out.

### Setting up a Compound

We'll assume you've created some DAI as per the [previous guide](https://bitfalls.com/2019/01/18/make-your-crypto-work-for-you-how-to-create-dai/). We'll also assume you have some DAI and some Ether in your wallet ([MetaMask](https://bitfalls.com/2018/02/16/metamask-send-receive-ether/) is fine) as both are needed for interacting with the DAI lending at Compound Finance.

Now, let's go to [the Compound.Finance app](https://app.compound.finance).

The first screen lists your general status - how much you've lent out, how much you've borrowed, how much you can extract, and all of this broken down by asset. We're at 0 on this example account.

![Compound Finance App](https://bitfalls.com/wp-content/uploads/2019/01/02.png)

The best current interest rate is on DAI, so let's use that.

Click on DAI.

Like in the previous post about CDPs, we need to allow the smart contract to manage our assets on request by clicking on Enable DAI and issuing this one-time transaction.

![Enable DAI](https://bitfalls.com/wp-content/uploads/2019/01/03-1.png)

Once DAI is enabled, we can click the Supply tab and enter the amount of DAI we want to lend out.

![Supply DAI](https://bitfalls.com/wp-content/uploads/2019/01/04-1.png)

This interface allows us to put the DAI into a big pool from which others can borrow it. All the lenders thus collectively share the interest generated by the loans.

### Interest

At barely 3%, small amounts don't make much sense. We need to take into account the 0.5% annual stability fee of MakerDAO CDP as well, which means that if your DAI is generated and not bought, you are actually looking at 2.5% rather than 3% interest here.

One should also be careful about too frequent transactions - at small amounts of invested money ($5000 and below), the interest is so small that it costs more to issue withdrawal and top-up transactions than one earns. Leaving money in this system is worth it when you can ignore it for a long time.

The interest rate changes all the time based on supply and demand. It is automatically applied to your principal dynamically, and the rate is refreshed every ethereum block - so every ~15 seconds. This literally means `(RATE/BLOCKS_PER_YEAR)` is applied to your principal every 15 seconds.

What's more, you can see this live. Your principal, interest, and your transaction history with this lending contract will be visible right there in the UI. As microcents start rolling in, you'll see them change your balance live, right there.

As you can see in the demo loan below, the interest earned has reset when the balance was topped up to 5868 DAI on January 16th, and since then it earned 3.16 DAI. Hardly a profit - this is why it's important to look at compound interest. By leaving it in and occasionally topping it up, the contract has potential to return much, much higher yields in the long run.

![Demo loan](https://bitfalls.com/wp-content/uploads/2019/01/05-1.png)

When you're ready to stop lending, simply opt for Withdraw instead of Supply and pull all the money out. Just like that, it'll appear in your wallet.

### Alternative UIs

The absolute best feature of this system is the decentralization - it's all in smart contracts, which means anyone can tap into them and you can keep using them even if the Compound.Finance UI disappears. Some alternative implementations have already appeared:

- [Bloqboard](https://app.bloqboard.com) combines Compound and Dharma in a smooth user interface letting you use the same Compound functionality above in addition to borrowing and lending peer to peer. This means you can issue a loan or borrow some cryptocurrency directly to/from other people, with blockchain as the arbiter.
- [MultiSupply](https://multi.supply) is an alternative UI for Compound Finance with more information (i.e. the expected earnings after a year)
- [Curious Giraffe](https://www.curiousgiraffe.io/compound/) is a read only analytics dashboard which outputs information about the current status of Compound (and some other projects), so it's worth keeping an eye on to learn more about the health of the system and details.

Be careful when using custom UIs. Watch which permissions you give them and how much of an interest rate they advertise. Some will skim off the top, others might hijack your transactions if they exceed a certain number. The above three are safe, but if you run into one that's even a little suspicious, ask around before using it.

### Bonus: How to Liquidate Someone

So what happens if someone borrows money and the value of the loan exceeds their collateral allowance of 150%? They can get liquidated. Let's get one!

If you go to [Conlan's Compound Liquidator](https://conlan.github.io/compound-liquidator/), you'll notice outstanding loans ordered by their ratio. Anything under 150% can be liquidated. The lower the ratio, the more you can liquidate someone.

_Note: as a reminder, liquidation is paying back someone's outstanding loan and getting an equal fraction of their collateral in return, at a discount._

Let's take a look at the third loan in this image.

![Loan](https://bitfalls.com/wp-content/uploads/2019/01/06.jpg)

143% is barely defaulting, but it'll do for our example. Clicking on the loan will show more details.

![Loan details](https://bitfalls.com/wp-content/uploads/2019/01/07.jpg)

The green checkmarks note the currencies the smart contract in charge of liquidations has been allowed to manage on our account. Again, we need to activate those we intend to deal with so I activated DAI and REP here because REP was borrowed and DAI was put down as collateral.

_Note: you only need to do this once, not once per loan but once in general._

I then obtained some REP through an exchange like [Coinvendor.io](https://coinvendor.io), and proceeded to pay it back. The process is as follows:

- select the collateralized currency you want to be paid in for this liquidation
- select the loaned currency you want to repay in the name of the borrower
- slide the bottom slider to the max, indicating you want to fully repay the debt and bring the ratio back to 150%
- click repay after making sure the estimated return will be worth it

After the transaction completes, you can inspect what happened.

![Transaction details](https://bitfalls.com/wp-content/uploads/2019/01/08.jpg)

We can see here that we:

- paid 0.0776 REP
- got 0.997634 DAI

At that time, the price of REP was $12.24.

This means that 0.0776 REP could have been sold for 0.949824 DAI on the open market, but we got 0.997634, which is around 5% more. If we had rushed off to buy back 0.0776 REP at market price, we could have earned a few cents on this transaction.

Granted, these amounts are insignificant considering the transaction costs and time it takes to execute them, so actions such as these are usually automated with bots which look at gas costs estimation, profit/loss ratio, and then auto-execute liquidations and re-buys on decentralized exchanges (DEXes).

This is also why you'll rarely find _Unsafe_ loans with non-trivial amounts listed on dashboards such as these - bigger amounts are definitely worth liquidating so they'll be gone soonest, leaving only the loans that were experiments and aren't intended to ever be paid back. Small loans such as these or the P2P ones you can find at Bloqboard are essentially spam attacks on the blockchain and its dapps.

So is liquidating people pointless?

That depends. You might get lucky in a sudden crash, or you might just be able to power up a bot that's better than anyone elses. It all depends on your idea and implementation. One thing is certain - it is this exact incentivization mechanic that keeps the system working and honest. As long as there is money to be earned from "ratting" people out, the system will work.

## Conclusion

Granted, the interest you can earn on Compound.Finance is far from [the power of long term dividends in the stock market](https://www.suredividend.com/snowball-effect/) but this is the blockchain - the main advantage is freedom. Freedom from regulators, from the tax man, from middlemen, from others telling you what you can and cannot invest in.

What are you waiting for? Exit their system. Enter freedom.