# Smart Treasury

This is an emerging use case, based in large part on [this article](https://www.placeholder.vc/blog/2020/9/17/stop-burning-tokens-buyback-and-make-instead) by Placeholder \(see image below, from that article, and the related [simulator](https://drive.google.com/file/d/1xTyQAQgTyl6Ajkl7AYzA6yB_X2H-5RVn/view).\)

这是一个新兴的用例，大部分基于[本文](https://www.placeholder.vc/blog/2020/9/17/stop-burning-tokens-buyback-and-make-instead)\(请参见该文章的下图，以及相关的[模拟器](https://drive.google.com/file/d/1xTyQAQgTyl6Ajkl7AYzA6yB_X2H-5RVn/view).\)

![Automatic buyback machine, token issuance pool, and liquidity provider \(9/17/20; Joel Monegro\)](../../.gitbook/assets/buyback+and+make+placeholder.jpeg)

Here we have a network of producers and consumers, where users pay for services with a fee token, and that income is used to pay producers in project tokens. Since the treasury is a Smart Pool, excess ETH would raise the price and cause the market to swap in project tokens for ETH until it the treasury is rebalanced - effectively an automatic buyback.

在这里，我们有一个生产者和消费者网络，用户使用收费令牌支付服务费用，而收入用于以项目令牌向生产者付款。由于国库是一个智能池，因此多余的以太坊会抬高价格并导致市场将项目代币交换为以太坊，直到金库重新平衡为止-有效地是自动回购。

The treasury can also buyback on a schedule by adding liquidity \(e.g., depositing reserve currency income\), and replenishes the market supply through issuance to producers. Maintaining the treasury as a Balancer pool means the market has guaranteed liquidity through the protocol itself.

国库还可以通过增加流动性\(例如，存放储备货币收入\)来按计划进行回购，并通过发行给生产者来补充市场供应。保持国库作为Balancer池意味着市场通过协议本身保证了流动性。

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* _canPauseSwapping_
* _canChangeSwapFee_
* ~~canChangeWeights~~
* ~~canAddRemoveTokens~~
* **canWhitelistLPs**
* ~~canChangeCap~~

The Smart Pool Treasury is by definition the only liquidity provider, so the only right you really need is whitelisting \(only the controller would be on the whitelist, enabling buybacks\). Change swap fee could be used to modulate trading to some degree if necessary. If you want to be able to prevent trading against the pool altogether, you could enable pause swapping \(though since this is the mechanism of automatic buyback, it should be used sparingly\).

从定义上讲，Smart Pool财政部是唯一的流动性提供者，因此，您真正需要的唯一权利是将白名单\(仅将控制者列入白名单，从而启用回购\)。如有必要，可以使用变更掉期费在某种程度上调节交易。如果您希望能够完全防止在池中进行交易，则可以启用暂停交换\(尽管由于这是自动回购的机制，所以应谨慎使用\)。

