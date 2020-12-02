# Swing Trading Pool

Recall the basic design of investment pools: traders profit through arbitrage opportunities, created when "internal" prices diverge from token market values, and pay fees for the privilege, which accrue to the liquidity providers.

回想一下投资池的基本设计：交易者通过套利机会获利，当“内部”价格与代币市场价值背离时，套利机会就产生了，并为流动性提供者支付了特权费用。

There is a natural trade-off here. Low fees and high trade volume leads to impermanent loss for liquidity providers; the arbers extract more value than they're adding in fees. On the other hand, high fees discourage trading altogether, so lower rates might actually generate more total revenue.

这里有一个自然的权衡。低收费和高交易量导致流动性提供者无常损失；仲裁人获得的价值大于他们所增加的费用。另一方面，高额费用完全不鼓励交易，因此较低的费率实际上可能产生更多的总收入。

One strategy to limit impermanent loss and maximize return is to create a pool that acts more like an index fund in traditional finance. You want something immune to day-to-day price fluctuations - so during "quiet" periods is equivalent to HODLing, since the balances don't change without trading. However, when the external market shifts significantly, the portfolio should "rebalance" through arbitrage in a manner favorable to both liquidity providers and traders. Here's a [paper](https://medium.com/balancer-protocol/high-fee-balancer-pools-for-swing-trading-8bc1c169a4c2) describing the concept.

限制永久损失和最大化回报的一种策略是创建一个池，其行为更像传统金融中的指数基金。您需要不受每日价格波动影响的商品-因此在“安静”时期相当于HODLing，因为余额没有交易就不会发生变化。但是，当外部市场发生重大变化时，投资组合应通过套利“重新平衡”，以有利于流动性提供者和交易者的方式进行。这是描述此概念的[论文](https://medium.com/balancer-protocol/high-fee-balancer-pools-for-swing-trading-8bc1c169a4c2)。

Say you have a volatile asset \(e.g., a basket of different types of wrapped BTC\), which can swing 4-5% on a boring day at the office, but makes big 10%+ moves relatively rarely. If you created a pool of such tokens with a swap fee above the "froth" \(e.g., 7%, 8%, even the maximum 10%\), it would be static most of the time. Yet on really big moves, the pool would be guaranteed to sell high and buy low - outperforming HODLing.

假设您有一个动荡的资产\(例如，一篮不同类型的包裹BTC \)，在无聊的一天里可以摆动4-5％，但10％以上的大幅波动相对很少。如果您创建的此类令牌池的交换费高于“泡沫” \(例如7％，8％，甚至最高10％\)，则大多数情况下它将是静态的。但是，如果采取真正的重大举措，则可以保证该池高价出售和低价购买-优于HODLing。

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* ~~canPauseSwapping~~
* _canChangeSwapFee_
* ~~canChangeWeights~~
* ~~canAddRemoveTokens~~
* ~~canWhitelistLPs~~
* ~~canChangeCap~~

You could implement this pool with no rights enabled at all \(= no trust required\). In which case you could just use a traditional BPool, and don't need a Smart Pool at all. However, you might want to change the swap fee to adjust to volatility \(e.g., if volatility increases to where daily fluctuations are triggering lots of trades\), you might want to raise the fee. Conversely, if volatility is lower than expected, you might want to lower it.

您可以完全不启用任何权限来实现该池(=无需信任\)。在这种情况下，您可以只使用传统的BPool，而根本不需要智能池。但是，您可能需要更改掉期费以适应波动性\(例如，如果波动性增加到每日波动触发大量交易的位置\)，则可能需要提高费用。相反，如果波动率低于预期，则可能需要降低波动率。

You might create an off-chain monitor that would update the fee dynamically in response to a volatility measure. Or the fee could be controlled through a DAO or less formal community voting mechanism.

您可以创建一个链下监视器，该监视器将根据波动性指标动态更新费用。或者可以通过DAO或不太正式的社区投票机制来控制费用。