# 流动性引导常见问题 - Liquidity Bootstrapping FAQ

### 我如何启动流动性引导池 - How do I launch a Liquidity Bootstrapping Pool?

Decide on critical parameters, such as sale duration, starting and ending weights, and estimate the demand \(i.e., expected sale rate\), using the [LBP simulator](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit?usp=sharing) to adjust the settings until you are happy with the resulting price curve. \(Best practice is to copy/download it, then customize to your own use case.\)

使用[LBP 仿真器](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit?usp=sharing)确定关键参数，如销售持续时间、起始和结束权重，并估计需求\(即预期销售率\)调整设置，直到您对结果价格曲线满意为止。\(最佳做法是复制/下载，然后根据自己的用例进行自定义。\)

Post on [\#token-requests](https://discord.gg/ARJWaeF) to request eligibility for governance token rewards. \(At least a week’s advance notice is recommended, and BAL rewards require an active CoinGecko price feed.\)

发布在[\＃token-requests](https://discord.gg/ARJWaeF)上，以请求有资格获得治理令牌奖励。\(建议至少提前一周通知，BAL奖励需要有效的CoinGecko价格供稿。\)

If your pool is eligible for weekly BAL rewards, they will be distributed to your LPs automatically. However, to receive BAL rewards yourself, you must redirect them from the smart pool contract to a wallet that can receive them - otherwise they will be locked in the contract and lost. This is done through a pull request to update [this file](https://github.com/balancer-labs/bal-mining-scripts/blob/master/redirect.json) in the mining repository. 

如果您的奖池有资格获得每周BAL奖励，它们将自动分配给您的LP。但是，要自己获得BAL奖励，您必须将其从智能池合同重定​​向到可以接收它们的钱包-否则它们将被锁定在合同中并丢失。这是通过对采矿资料库中的[此文件](https://github.com/balancer-labs/bal-mining-scripts/blob/master/redirect.json)进行更新的拉取请求完成的。

### LBP应该持续多长时间 - How long should an LBP last?

This is a fully customizable parameter that is up to you, based on your objectives. However we can provide some general tips. It’s important to give your potential investors sufficient time to participate and for healthy price discovery to occur. We’ve seen this process work well over a span of 3 days, but as LBPs are a new innovation, we believe there are other lengths of time that could work as well or even better. 

这是一个完全可定制的参数，取决于您的目标。但是，我们可以提供一些一般性提示。重要的是要给您的潜在投资者足够的时间参与并进行健康的价格发现。我们已经看到此过程在3天的时间里效果良好，但是由于LBP是一项新的创新，因此我们认为还有其他时间长度可能会更好甚至更好。

Considering the fast pace and unpredictability of events happening regularly in DeFi, making your LBP too short could allow obstacles that are unrelated to your project \(i.e., a spike in Ethereum network congestion or a new yield farming craze\) to get in the way of a successful sale. We would recommend at least 3 days, but you are certainly free to make it shorter if you believe it would better serve your particular case.

考虑到DeFi中定期发生的事件的快速和不可预测性，将LBP设置得太短可能会使与您的项目无关的障碍（例如，以太坊网络拥挤的峰值或新的农耕热潮\）成为障碍。销售的成功。我们建议至少保留3天，但是如果您认为可以更好地满足您的特定情况，则可以将其缩短。

Note that the default minimum duration is approximately 2 weeks \(and 2 hours for the add token time lock\); using a shorter period will require overriding that default when you create the pool.

### 我应该如何选择初始价 - How should I choose a starting price?

You can think of the starting price of your LBP as the ceiling you’d want to set for the token sale. This may seem counterintuitive, but since LBPs work differently than other token sales, your starting price should be set much higher than what you believe is the fair price. 

您可以将LBP的初始价格视为想要出售token的上限。这看起来似乎违反直觉，但是由于LBP的工作方式不同于其他代币出售，因此您的初始价格应设定为比您认为的合理价格高得多。

This does not mean you’re trying to sell the token above what it’s worth. Setting a high starting price allows the changing pool weights of your LBP to make their full impact, lowering the price progressively until a market equilibrium is reached. Unlike older token sale models such as bonding curves, LBP investors are disincentivized to buy early, and instead benefit from waiting for the price to decrease until it reaches a level they believe is fair.

这并不意味着您试图以高于其价值的价格出售代币。设置较高的初始价格可以使您的LBP池权重不断变化，从而发挥其全部影响，逐步降低价格，直到达到市场平衡为止。与债券曲线等较早的代币销售模型不同，LBP投资者没有提早购买的动机，而是从等待价格下跌直到价格达到他们认为合理的水平而受益。

For example, if you believe the fair price for your token is $2, you may want to consider an opening price that is up to $10.

例如，如果您认为代币的公平价格为2美元，则可能要考虑最高10美元的开盘价。

### LBP的建议开始和结束权重配置是什么？ - What are the recommended starting and ending weight configurations for an LBP?

The general idea is to start with the pool weights skewed towards your token and end with the pool weights skewed towards the reserve asset\(s\). This configuration is designed to make it so that the majority of your tokens end up being exchanged for the reserve asset\(s\) you have chosen.

总体思路是，首先将池权重偏向令牌，然后将池权重偏向储备资产。此配置旨在实现这一目的，以便最终将您的大多数令牌交换为您选择的储备资产。

Be sure to think all the way through your intended sale, since there are some technical subtleties. For instance, if your weights are very close to the min/max weight boundaries, `pokeWeights` could fail in some edge cases where the total would temporarily be exceeded. Unless you need the maximum 98%/2% \(49/1 denorm\) weights, it's best to use weights that sum to a lower total number \(e.g., 38/2\), and put the project token first in the list when you create the pool, so that the weight _decrease_ would be processed first.

由于存在一些技术上的细微差别，因此请务必仔细考虑您打算进行的销售。例如，如果您的权重非常接近最小/最大权重边界，则在某些临时超出总和的极端情况下，`pokeWeights`可能会失败。除非您需要最大权重为 98％/ 2％\(49/1 denorm \)，否则最好使用总和较小的权重\(例如38/2 \)，然后将项目令牌放在创建池时列出，以便首先处理权重_decrease_。

### 我如何使用LBP以最少的种子资金进行代币销售? - How can I use an LBP to conduct a token sale with minimal seed capital?

The lower you set the weight\(s\) of the reserve asset\(s\) in your LBP, the less upfront capital you would need to seed your LBP. In a two-token LBP, the most skewed ratio you can set is 2:98, meaning that the pool composition will be 2% your reserve asset, and 98% the project token. 

您在LBP中设置储备资产的权重越低，则注入LBP所需的前期资本就越少。在两令牌LBP中，您可以设置的最大偏差比率为2:98，这意味着池组成将是您的储备资产的2％和项目令牌的98％。

However, since the value of your project’s tokens is proportional to the value of the reserve assets you provide for the sale, the amount of funds you can raise in the sale is limited by the total value of the reserve assets you provide to the LBP.

但是，由于项目代币的价值与您为出售提供的储备资产的价值成正比，因此您可以在出售中筹集的资金数量受到您向LBP提供的储备资产的总价值的限制。

For example, with an LBP weight ratio of 2:98 \(reserve asset:project token\), if your upfront capital is 50K USDC, and your starting price is 10 USDC, the amount of tokens you can sell is \[\(50K / 0.02\) / 10\] = approx. 250K tokens.

例如，当LBP权重比为2:98 \(保留资产：项目代币\)时，如果您的前期资本为50K USDC，而您的起始价格为10 USDC，则您可以出售的代币数量为 \[\(50K / 0.02\) / 10\] = 大约250K令牌。

In contrast, if your upfront capital is 1M USDC, you’d be able to sell around 5M tokens.

相反，如果您的前期资本是100万USDC，那么您将能够出售大约500万个代币。

### 如何根据种子资本和池权重计算代币销售量的不同方案? - How can I calculate different scenarios for the amount of tokens to sell, based on the amount of seed capital and pool weights?

You can use our [LBP simulator](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit?usp=sharing) to plug in your variables and see the projected results. \(Best practice is to copy it so you have your own version, or even download to Excel.\)

您可以使用我们的[LBP模拟器](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit?usp=sharing)插入变量并查看预期结果。\(最佳做法是复制它，以便您拥有自己的版本，甚至下载到Excel。\)

There's a lot going on there, but a good place to start is the "ad hoc" simulator at the top right. There, you can type in balances, weights, and the swap fee, and see what the initial price would be. Then you can type your starting values into the main interface on the top left, and experiment with different ending weights and sale rates to come up with a reasonable price curve. It will also display the total proceeds and leftover tokens.

那里有很多事情要做，但是一个不错的起点是右上角的“ ad hoc”模拟器。在这里，您可以输入余额，权重和掉期费，并查看初始价格。然后，您可以在左上角的主界面中输入起始值，并尝试使用不同的终止权重和销售率以得出合理的价格曲线。它还将显示总收益和剩余令牌。

You'll see right away that it is very sensitive to the sale rate. The price is derived from two values - balances and weights. And you only control one of them! Your weight "flipping" schedule determines how the weights will change over time, but \(unless you intervene with further issuance or buybacks\), balances only change through trades: mostly people buying your token with the reserve currency.

您会立即看到它对售卖率非常敏感。价格从两个值得出-余额和权重。而您只能控制其中之一！您的权重“翻转”计划决定权重随时间的变化方式，但是\(除非您干预进一步发行或回购\)，余额仅通过交易发生变化：大多数人都是用储备货币购买代币。

If people buy while the weights are constant - or someone places a huge order - the token price may go up sharply. This is how a token sale can get "stuck" on a platform like Uniswap, where the prices are set by balances only. This may be optimal for retail liquidity, but is much less so for a token sale.

如果人们在权重不变的情况下进行购买-或有人下了大笔订单-代币价格可能会急剧上涨。这样，令牌销售就可以像Uniswap这样的平台上“卡住”，该平台的价格仅由余额决定。对于零售流动性而言，这可能是最佳的，但对于代币销售而言，则远非如此。

In an LBP, buyers suffering sticker shock need only wait for the weights to adjust and the price to start dropping. \(If they are impatient, they can call the public `pokeWeights` function themselves to tell the contract to update weights to the proper values at the current block.\)

在LBP中，遭受标签冲击的购买者仅需等待权重调整并且价格开始下跌。\(如果他们不耐烦，他们可以自己调用公共`pokeWeights`函数来告诉合同将权重更新为当前块的适当值。)

In practice, we have seen that the market is efficient. Traders do indeed keep the price within a fairly narrow band around the discovered market value. That is one reason we believe this is the fairest token distribution method yet discovered. And as a bonus, this price curve tends to maximize total proceeds of the sale.

实际上，我们已经看到市场是有效的。交易者确实确实将价格保持在发现的市场价值附近的相当窄的范围内。这就是我们认为这是迄今为止发现的最公平的代币分配方法的原因之一。作为奖励，此价格曲线趋向于使销售的总收益最大化。

Note that it may not always be possible to sell all available tokens in the desired price range with the capital available. In this case, you could either split up the sale into multiple tranches \(either re-using the same pool, or making new pools, using the proceeds of each to fund the next\), or alter the supply or weight function during the sale in various ways, as described below.

请注意，并非总是可以用可用资金以期望的价格范围出售所有可用代币。在这种情况下，您可以将销售分成多个部分（重新使用相同的集合，或者新建一个集合，使用每个集合的收益为下一个集合提供资金），或者在交易过程中更改供应或权重函数出售方式如下所述。

### 在运行LBP时如何影响代币价格? - How can I influence the token price while running an LBP?

Unless you deposit tokens yourself during the sale \(e.g, by adding additional liquidity through joinPool\), the only way to affect prices during an LBP is by setting the target pool weights. Changing the target pool weights influences price, such that increasing the weight of an asset increases its relative price, and vice versa. The change in price gradually incentivizes investors to buy your token in exchange for the reserve asset\(s\), which then changes the balances of assets in the pool.

除非您在销售过程中自己存放令牌（例如，通过joinPool添加额外的流动性），否则在LBP期间影响价格的唯一方法是设置目标池权重。更改目标池权重会影响价格，因此增加资产的权重会增加其相对价格，反之亦然。价格的变化逐渐激励投资者购买您的代币以换取储备资产，然后储备池中的资产余额发生变化。

### LBP直播时如何更改其权重?  - How do I change the weights of my LBP while it’s live?

Use the `updateWeightsGradually` function to put the contract into a state where it will respond to the `pokeWeights` call by setting all the weights according to the point on the "weight curve" corresponding to the current block. `pokeWeights` can be called by you, or anyone, to update the weights according to your LBP’s configuration.

使用`updateWeightsGradually`功能，通过根据与当前块相对应的`pokeWeights`上的点设置所有权重，将合同置于一种状态，以响应`pokeWeights`调用。您或任何人都可以调用`pokeWeights`，以根据您的LBP的配置更新权重。

You can also call `updateWeight` \(as long as a gradual update is not running\) to set weights arbitrarily - but this will not affect the price. It will transfer tokens as required such that the new balances and weights maintain current prices.

您也可以调用`updateWeight` \(只要不运行逐步更新\)来任意设置权重-但这不会影响价格。它将根据需要转移令牌，以使新的余额和权重保持当前价格。

### 我可以把代币卖给什么样的储备资产? - What are the reserve assets I can sell my token for?

You can sell your token for any ERC-20 tokens. You can choose up to 7 other tokens to be used as reserve assets in your LBP token sale. Projects typically sell their tokens for highly liquid stablecoins such as DAI, USDC, or USDT; and/or for WETH.

您可以将您的令牌出售为任何ERC-20令牌。您最多可以选择7个其他代币用作LBP代币售卖中的储备资产。项目通常将其代币出售给高度流动的稳定币，例如DAI，USDC或USDT；和/或WETH。

### 如何在Balancer交易UI上按名称和徽标列出我的令牌？- How do I get my token listed by name and logo on the Balancer exchange UI?

The Balancer team regularly monitors the crypto landscape and adds new tokens to our listings based on internal requirements. Tokens do not need to request to be listed on the exchange, as it is done proactively.

Balancer团队会定期监控加密货币的情况，并根据内部需求将新令牌添加到我们的清单中。令牌不需要主动在交易所列出，因为它是主动完成的。

If you are launching an LBP and want to make sure that your token is listed on the exchange before launch, please [contact the team](mailto:contact@balancer.finance) for assistance.

如果您要启动LBP，并希望在启动前确保令牌已在交易所中列出，请[与团队联系](mailto：contact@balancer.finance)寻求帮助。

### 我如何将我的代币列入BAL采矿奖励白名单? - How do I get my token whitelisted for BAL mining rewards?

[This page](../protocol/bal-liquidity-mining/exchange-and-reward-listing.md) describes the process.

[本页](../protocol/bal-liquidity-mining/exchange-and-reward-listing.md) 描述了此过程。

### 提供启动LBP所需的初始种子资金后，我以后需要存入额外的资金吗？ -  After providing the initial seed capital needed to launch an LBP, do I need to deposit additional capital later?

No, you do not need any additional capital beyond the initial seed amount based on the starting weights you’ve selected for your pool. However, you do have the optional ability to deposit new capital into the pool as a buyback mechanism while the LBP is running.

不，根据您为池选择的起始权重，您不需要除初始种子数量以外的任何其他资本。但是，您确实可以选择在LBP运行时将新资金作为回购机制存入池中。