[Financial Options](https://github.com/magnum31415/wiki/blob/main/options.md)

# Table of Contents

* [Naked Call Break-even Point](#naked-call-break-even-point)
* [Short Straddle Construction](#short-straddle-construction)
* [Long Put Break-even Profitability](#long-put-break-even-profitability)
* [Reasons to Buy a Call Option](#reasons-to-buy-a-call-option)
* [Long Put Profit per Share](#long-put-profit-per-share)
* [Covered Call Assignment at Expiration](#covered-call-assignment-at-expiration)
* [Iron Condor](#iron-condor)
* [Covered Call Maximum Profit](#covered-call-maximum-profit)
* [Naked Call Risk](#naked-call-risk)
* [Long Call Maximum Loss](#long-call-maximum-loss)
* [Long Put Profit Calculation](#long-put-profit-calculation)
* [Option Premium](#option-premium)

---

# Naked Call Break-even Point

## Question

At what price does the break-even point occur for a naked call sold with a strike price of 50 GBP and a premium of 3 GBP?

### Possible Answers

- 47 GBP
- 50 GBP
- 53 GBP
- 55 GBP
- I am not sure

### Correct Answer

✅ 53 GBP

## Theory

A Naked Call is created when an investor sells a call option without owning the underlying shares.

The seller receives a premium upfront.

The break-even point is reached when losses from the increasing stock price exactly offset the premium received.

### Formula

Break-even Price:

```text
Strike Price + Premium Received
```

### Calculation

```text
50 + 3 = 53 GBP
```

Above 53 GBP the seller begins losing money.

## Why the Other Answers Are Incorrect

### 47 GBP

❌ Incorrect.

This would be calculated by subtracting the premium rather than adding it.

### 50 GBP

❌ Incorrect.

This is only the strike price and ignores the premium received.

### 55 GBP

❌ Incorrect.

The break-even point occurs earlier, at 53 GBP.

### I am not sure

❌ Incorrect.

The break-even point can be calculated precisely.

---

# Short Straddle Construction

## Question

To create a straddle using a short call with a strike price of 35 GBP, what should you do?

### Possible Answers

- Buy a 35 GBP put.
- Sell a 35 GBP put.
- Buy a 40 GBP call.
- Sell another 35 GBP call.
- I am not sure.

### Correct Answer

✅ Sell a 35 GBP put.

## Theory

A Short Straddle consists of:

- Selling one Call
- Selling one Put

Both options must have:

- The same strike price.
- The same expiration date.

The strategy profits when the underlying asset remains close to the strike price and both options lose value through time decay.

### Example

```text
Sell Call 35
Sell Put 35
```

This creates a Short Straddle.

## Why the Other Answers Are Incorrect

### Buy a 35 GBP put

❌ Incorrect.

This would create a synthetic position, not a Short Straddle.

### Buy a 40 GBP call

❌ Incorrect.

The strike price differs and does not create a Straddle.

### Sell another 35 GBP call

❌ Incorrect.

This would simply double the short call exposure.

### I am not sure

❌ Incorrect.

A Straddle always requires one Call and one Put.

---

# Long Put Break-even Profitability

## Question

Long put holders make a profit when the stock price is where relative to the strike price minus the premium?

### Possible Answers

- Higher.
- Lower.
- Exactly at break-even.
- At the strike price.
- I am not sure.

### Correct Answer

✅ Lower.

## Theory

The break-even point of a Long Put is:

```text
Strike Price - Premium Paid
```

A Long Put becomes profitable only when the stock price falls below this level.

### Example

```text
Strike = 50 GBP
Premium = 2 GBP

Break-even = 48 GBP
```

If the stock finishes at:

```text
45 GBP
```

The investor earns a profit.

## Why the Other Answers Are Incorrect

### Higher

❌ Incorrect.

A Long Put profits from falling prices, not rising prices.

### Exactly at break-even

❌ Incorrect.

At break-even there is neither profit nor loss.

### At the strike price

❌ Incorrect.

The premium paid must also be recovered.

### I am not sure

❌ Incorrect.

Profitability is determined by a simple break-even calculation.

---

# Reasons to Buy a Call Option

## Question

Someone might buy a call option for all of the following reasons EXCEPT:

### Possible Answers

- Speculate on rising prices.
- Delay purchasing a stock.
- Hedge against falling prices.
- Portfolio diversification.
- I am not sure.

### Correct Answer

✅ Hedge against falling prices.

## Theory

A Call Option gives the right to buy a stock at a fixed price.

Investors typically buy Calls to:

- Speculate on rising prices.
- Gain leverage.
- Delay buying shares while maintaining upside exposure.

Protection against falling prices is generally achieved with Put Options.

## Why the Other Answers Are Incorrect

### Speculate on rising prices

❌ Incorrect.

This is one of the most common reasons to buy Calls.

### Delay purchasing a stock

❌ Incorrect.

A Call can provide exposure while postponing a full stock purchase.

### Portfolio diversification

❌ Incorrect.

Options can be used as part of a diversification strategy.

### I am not sure

❌ Incorrect.

The incorrect statement is the use of Calls for downside protection.

---

# Long Put Profit per Share

## Question

If an investor buys a put option with a strike price of 50 GBP for 2 GBP and the stock falls to 40 GBP, what is the profit per share (excluding commissions)?

### Possible Answers

- 2 GBP
- 10 GBP
- 12 GBP
- 8 GBP
- I am not sure.

### Correct Answer

✅ 8 GBP

## Theory

A Put Option increases in value as the stock price falls below the strike price.

### Calculation

Intrinsic Value:

```text
50 - 40 = 10 GBP
```

Net Profit:

```text
10 - 2 = 8 GBP
```

## Why the Other Answers Are Incorrect

### 2 GBP

❌ Incorrect.

This is the premium paid, not the profit.

### 10 GBP

❌ Incorrect.

This ignores the premium cost.

### 12 GBP

❌ Incorrect.

The profit cannot exceed the intrinsic value plus premium adjustment.

### I am not sure

❌ Incorrect.

The calculation is straightforward.

---

# Covered Call Assignment at Expiration

## Question

An investor owns shares purchased at 40 GBP and sells a 45 GBP call for a premium of 3 GBP. If the stock rises to 50 GBP, what happens at expiration?

### Possible Answers

- The investor keeps the shares and the 3 GBP premium.
- The investor keeps the 3 GBP premium but must sell the shares.
- The investor earns 8 GBP per share.
- The investor must buy additional shares at 45 GBP.
- I am not sure.

### Correct Answer

✅ The investor earns 8 GBP per share.

## Theory

A Covered Call consists of:

- Owning shares.
- Selling a Call against those shares.

When the stock price rises above the strike price, assignment is likely.

The investor must sell the shares at the strike price.

### Calculation

Capital Gain:

```text
45 - 40 = 5 GBP
```

Premium Received:

```text
3 GBP
```

Total Profit:

```text
5 + 3 = 8 GBP per share
```

## Why the Other Answers Are Incorrect

### The investor keeps the shares and the 3 GBP premium

❌ Incorrect.

The option will likely be exercised because it is in-the-money.

### The investor keeps the 3 GBP premium but must sell the shares

❌ Partially true but incomplete.

It does not include the capital gain and therefore is not the best answer.

### The investor must buy additional shares at 45 GBP

❌ Incorrect.

The investor already owns the shares.

### I am not sure

❌ Incorrect.

The outcome can be calculated precisely.

# Iron Condor

## Question

An Iron Condor requires managing four different option contracts. What makes this challenging for new investors?

### Possible Answers

- High commission costs.
- The strategy only works in volatile markets.
- The strategy requires a professional certification.
- Tracking multiple break-even points.
- I am not sure.

### Correct Answer

✅ Tracking multiple break-even points.

## Theory

An Iron Condor is an advanced options strategy that combines:

- A Bull Put Spread
- A Bear Call Spread

This results in four option contracts:

1. Buy one Put
2. Sell one Put
3. Sell one Call
4. Buy one Call

The strategy profits when the underlying asset remains within a predefined price range.

Because multiple strike prices are involved, investors must monitor:

- Maximum profit
- Maximum loss
- Lower break-even point
- Upper break-even point

For beginners, understanding and tracking all these levels can be difficult.

## Why the Other Answers Are Incorrect

### High commission costs

❌ Incorrect.

Although commissions may be higher because four contracts are involved, this is not the main challenge of understanding the strategy.

### The strategy only works in volatile markets

❌ Incorrect.

Iron Condors are generally used when low volatility or stable prices are expected.

### The strategy requires a professional certification

❌ Incorrect.

No professional certification is required.

### I am not sure

❌ Incorrect.

The challenge is specifically the management of multiple strike prices and break-even points.

---

# Covered Call Maximum Profit

## Question

An investor buys 100 shares of ABC at 50 GBP and sells a call option with a strike price of 55 GBP for a premium of 2 GBP. What is the maximum profit per contract?

### Possible Answers

- Total profit of 700 GBP.
- Unlimited profit potential.
- Premium only of 200 GBP.
- Capital gain of 100 GBP.
- I am not sure.

### Correct Answer

✅ Total profit of 700 GBP.

## Theory

A Covered Call consists of:

- Owning the shares.
- Selling a call option against those shares.

The seller receives a premium immediately.

### Calculation

Stock purchase price:

50 GBP

Strike price:

55 GBP

Capital gain per share:

55 - 50 = 5 GBP

For 100 shares:

5 × 100 = 500 GBP

Premium received:

2 × 100 = 200 GBP

Total maximum profit:

500 + 200 = 700 GBP

## Why the Other Answers Are Incorrect

### Unlimited profit potential

❌ Incorrect.

The sold call limits upside potential at the strike price.

### Premium only of 200 GBP

❌ Incorrect.

This ignores the capital gain from the shares.

### Capital gain of 100 GBP

❌ Incorrect.

The gain is 500 GBP, not 100 GBP.

### I am not sure

❌ Incorrect.

The maximum profit can be calculated precisely.

---

# Naked Call Risk

## Question

What is the maximum risk faced by a seller of uncovered calls (naked calls)?

### Possible Answers

- The seller only receives the premium.
- The seller receives the strike price minus the premium.
- The seller has unlimited loss potential.
- The seller receives double the premium.
- I am not sure.

### Correct Answer

✅ The seller has unlimited loss potential.

## Theory

A Naked Call is created when an investor sells a call option without owning the underlying shares.

If the stock rises sharply:

- The option buyer exercises the call.
- The seller must provide the shares.
- The seller may have to purchase those shares at much higher market prices.

Since stock prices theoretically have no upper limit, losses can become extremely large.

### Example

Strike price:

50 GBP

Stock rises to:

300 GBP

The seller may be forced to buy shares at 300 GBP and sell them for only 50 GBP.

## Why the Other Answers Are Incorrect

### The seller only receives the premium

❌ Incorrect.

This describes the income received, not the risk.

### The seller receives the strike price minus the premium

❌ Incorrect.

This is not a risk calculation.

### The seller receives double the premium

❌ Incorrect.

Premium income does not automatically double.

### I am not sure

❌ Incorrect.

The maximum risk is theoretically unlimited.

---

# Long Call Maximum Loss

## Question

An investor buys 10 call contracts at 0.50 GBP each (500 GBP total). The stock expires below the strike price. What is the loss, excluding commissions?

### Possible Answers

- 50 GBP
- 500 GBP
- 5000 GBP
- Unlimited
- I am not sure

### Correct Answer

✅ 500 GBP

## Theory

When buying options:

- The maximum loss is limited to the premium paid.
- The buyer is never obligated to exercise the option.

If the option expires out-of-the-money:

- It becomes worthless.
- The entire premium is lost.

### Calculation

Total premium paid:

500 GBP

Option expires worthless:

Loss = 500 GBP

## Why the Other Answers Are Incorrect

### 50 GBP

❌ Incorrect.

This does not match the total premium paid.

### 5000 GBP

❌ Incorrect.

The buyer cannot lose more than the premium.

### Unlimited

❌ Incorrect.

Unlimited risk applies to some option sellers, not option buyers.

### I am not sure

❌ Incorrect.

The maximum loss is clearly defined.

---

# Long Put Profit Calculation

## Question

A put option with a strike price of 40 GBP costs 3 GBP. If the underlying stock falls to 30 GBP at expiration, what is the profit for the put owner?

### Possible Answers

- Profit of 7 GBP.
- Profit of 10 GBP.
- Profit of 13 GBP.
- Loss of 3 GBP.
- I am not sure.

### Correct Answer

✅ Profit of 7 GBP.

## Theory

A Put Option gives the holder the right to sell shares at the strike price.

When the stock price falls below the strike price, the put gains value.

### Calculation

Strike price:

40 GBP

Stock price:

30 GBP

Intrinsic value:

40 - 30 = 10 GBP

Premium paid:

3 GBP

Net profit:

10 - 3 = 7 GBP

## Why the Other Answers Are Incorrect

### Profit of 10 GBP

❌ Incorrect.

This ignores the premium paid.

### Profit of 13 GBP

❌ Incorrect.

This exceeds the intrinsic value.

### Loss of 3 GBP

❌ Incorrect.

The option is profitable because it finishes in-the-money.

### I am not sure

❌ Incorrect.

The profit can be calculated directly.

---

# Option Premium

## Question

Which statement best describes an option premium?

### Possible Answers

- The amount collected by the seller when the option expires.
- The price paid by the buyer to the seller when the option is purchased.
- The commission charged by the broker.
- The difference between strike prices.
- I am not sure.

### Correct Answer

✅ The price paid by the buyer to the seller when the option is purchased.

## Theory

The premium is the market price of the option.

It is:

- Paid by the buyer.
- Received by the seller.
- Paid when the trade is executed.

The premium consists of:

- Intrinsic value
- Time value

## Why the Other Answers Are Incorrect

### The amount collected by the seller when the option expires

❌ Incorrect.

The premium is received when the option is sold, not when it expires.

### The commission charged by the broker

❌ Incorrect.

Broker commissions are separate from the option premium.

### The difference between strike prices

❌ Incorrect.

This relates to option spreads, not premiums.

### I am not sure

❌ Incorrect.

The premium has a precise definition.
