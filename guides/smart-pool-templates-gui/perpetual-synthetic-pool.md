# 合成永动池 - Perpetual Synthetic Pool

Synthetic tokens, through the efforts of [UMA](https://umaproject.org/) and others, are coming into their own, grabbing the spotlight and bringing a type of derivative familiar from the traditional financial markets to crypto.

通过[UMA](https://umaproject.org/)和其他公司的努力，Synthetic代币开始崭露头角，引起了人们的关注，并将传统金融市场上熟悉的一种衍生产品引入了加密货币。

There are already balancer pools featuring synthetics \(I would link to one, but it would be out-of-date in a month.\) One [UMA synthetic token](https://medium.com/uma-project/priceless-synthetic-tokens-f28e6452c18b) current as of this writing is yUSD-OCT20.

已经有带有合成器的平衡池\(我会链接到一个，但是一个月后会过时。\)一个[UMA合成令牌](https://medium.com/uma-project/priceless-synthetic-tokens-f28e6452c18b)当前为yUSD-OCT20。

Note that when UMA supports perpetual synthetics natively - tokens with no expiration date - this gets easier. At that point, you would not need to add or remove tokens monthly, and could simply hold them in a pool, perhaps rebalancing periodically.

请注意，当UMA原生支持synthetics合成时-没有到期日期的token-这会变得更加容易。到时，您无需每月添加或删除token，只需将它们保存在一个池中，也许定期进行重新平衡即可。

Current priceless synthetics have an expiration date, and the most common implementations expire monthly. It is now common practice to create a standard BPool with a synthetic expiring a month or two in the future, and a collateral token like USDC. When the synthetic expires, people generally withdraw liquidity from the current month's pool, and add it to the next month. One of the issues with this is the gas costs incurred by all these rollovers. Your investment would need to be big enough to still profit, after fees.

当前的无价synthetics有到期日期，最常见的实现每月到期。现在，通常的做法是创建一个标准BPool，该标准BPool的合成到期日为一两个月，并附带USDC之类的抵押代币。当合成货币到期时，人们通常会从当月的资金池中提取流动资金，并将其添加到下个月。问题之一是所有这些延期产生的gas成本。您的投资需要足够大才能在扣除费用后仍然获利。

One solution would be a "rolling synthetic" Smart Pool. There are many possible implementations. One is a 2-token pool with the current synthetic and a collateral coin. Near the end of the month, the pool operator would add the next month's token, and then, some time after expiration, remove the previous token.

一种解决方案是“滚动synthetic”智能池。有许多可能的实现。一个是带有当前synthetic货币和抵押币的2令牌池。在月底附近，池操作员将添加下个月的令牌，然后在到期后的某个时间删除前一个令牌。

Note that the pool creator would need to have enough pool tokens to be able to call `removeToken` \(which burns pool tokens\). The bigger the total supply, they more they need, so if a lot of other LPs joined, the creator might need to deposit more liquidity in order to swap out synthetics.

请注意，池创建者将需要有足够的池令牌才能调用`removeToken` \(将刻录池令牌\)。总供应量越大，他们需要的越多，因此，如果加入许多其他的LP，创建者可能需要存入更多的流动性以换出synthetics。

Alternatively, a pool operator could create a pool with two synthetics - the current and next month's tokens. Initially the weights would favor the current month, but over the course of the month \(tracking "implied volatility"\), the weights would "flip," so that the situation was reversed near the end of the month, with next month's token weighted highly, and the current month's heavily discounted.

或者，池操作员可以使用两个synthetics对象（当前和下个月的令牌）创建一个池。最初，权重将偏向当前月份，但是在该月的过程中 (跟踪“隐含波动率”\)，权重将“翻转”，因此情况在月底附近被反转，带有下个月的标记权重很高，而本月的折扣很大。

These could even be combined, adding the future month's synthetic a bit in advance, and removing the expired month's synthetic after a redemption period.

甚至可以将它们组合在一起，提前添加未来月份的综合信息，并在赎回期后删除过期月份的综合信息。

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* **canPauseSwapping**
* ~~canChangeSwapFee~~
* **canChangeWeights**
* _canAddRemoveTokens_
* ~~canWhitelistLPs~~
* ~~canChangeCap~~

You need to change weights, and probably also add/remove tokens, unless you make a new pool each month. If you're adding and removing tokens, you would probably want to be able to pause swapping during these operations, or during redemption periods.

您需要更改权重，并且可能还需要添加/删除令牌，除非您每个月都新建一个池。如果要添加和删除令牌，则可能希望能够在这些操作期间或兑换期间暂停交换。
