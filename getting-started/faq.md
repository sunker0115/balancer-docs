# 常见问题 - FAQ

## 基础 - Basics

### 什么是Balancer协议？ - What is the Balancer Protocol?

Balancer is a protocol for multi-token [automated market-making](../protocol/background.md). It enables portfolio owners to create Balancer Pools, and traders to trade against them. Balancer Pools contain two or more tokens, each with an independent weight representing its proportion of the total pool value. The pools provide the Balancer Protocol with liquidity, and charge traders a fee for access to it. Pools can be considered automated market-makers, since anyone can swap any two tokens, in any pool.

Balancer是用于多令牌[自动做市](../protocol/background.md)的协议。它使投资组合所有者可以创建均衡器池，而交易者可以与其进行交易。平衡器池包含两个或多个令牌，每个令牌具有独立的权重，代表其在池总值中所占的比例。这些池向“平衡器协议”提供流动性，并向交易商收取使用该协议的费用。可以将池视为自动做市商，因为任何人都可以在任何池中交换任何两个令牌。

### 平衡器协议有什么用？ - How is the Balancer Protocol useful?

There are two categories of users who can benefit from the Balancer Protocol: liquidity providers - who own Balancer Pools or participate in shared pools, and traders - who buy or sell the underlying pool assets on the open market.

可以从“平衡器协议”中受益的用户有两类：流动性提供者-拥有“平衡器池”或参与共享池，以及交易者-在公开市场上买卖基础池资产。

Anyone with two or more ERC20 tokens can be a liquidity provider. For example:

任何拥有两个或更多ERC20代币的人都可以成为流动性提供者。例如：

* Portfolio managers, who want to have controlled exposure to different assets without complicated and expensive rebalancing 
* Investors who have ERC20 tokens sitting idly in a wallet, and would like to put them to work earning passive income from fees 


* 投资组合经理，他们希望控制不同资产的敞口，而无需进行复杂而昂贵的重新平衡
* 拥有ERC20代币闲置放在钱包中并希望将其投入使用以从收费中获得被动收入的投资者

Traders can choose from a diverse set of pools, each presenting a unique set of investment opportunities and challenges through its particular configuration of tokens, weights, and fees. The interplay between these settings, pool volume, and external prices generates market forces which incentivize traders to maintain stable token ratios, thereby preserving asset value for liquidity providers.

交易者可以从不同的集合中进行选择，每个集合都通过其令牌，权重和费用的特定配置来呈现独特的投资机会和挑战。这些设置，池数量和外部价格之间的相互作用会产生市场力量，从而激励交易者保持稳定的代币比率，从而为流动性提供者保留资产价值。

There are three main categories:

主要分为三类：

* "Retail" traders seeking to exchange tokens with low slippage at favorable rates
* Arbitrageurs seeking profit through leveling market inefficiencies between DEXs or CEXs 
* Ethereum smart contracts seeking liquidity for a variety of reasons, such as liquidating positions on other protocols, trading on behalf of users, etc. 

* “零售”交易者，希望以低廉的价格交换低滑点的代币
* 套利者通过平息DEX或CEX之间的市场低效率来寻求利润
* 以太坊智能合约出于各种原因寻求流动性，例如清算其他协议的头寸，代表用户进行交易等。

### Balancer协议是否完全未经许可？ - Is Balancer Protocol fully permissionless?

Yes. Balancer Pools cannot be censored or whitelisted. Traders cannot be censored or whitelisted. Balancer Labs does not have the power to halt or edit the smart contracts in any way after they’ve been deployed. The contracts are not upgradeable, and there is no admin functionality or "backdoor" present in the code.

是的。Balancer池不能被审查或列入白名单。交易者不能被审查或列入白名单。部署智能合约后，Balancer Labs无权暂停或编辑它们。合同不可升级，并且代码中没有管理功能或“后门”。

Of course, Balancer has no control over the contracts of ERC20 tokens placed in Balancer pools. If a centralized token \(e.g., USDC\) were to blacklist an address or freeze all transfers, that would affect all USDC tokens everywhere, including those in Balancer Pools.

当然，Balancer无法控制放置在Balancer池中的ERC20代币的合约。如果集中式令牌\(e.g., USDC\)将地址列入黑名单或冻结所有转账，这将影响到所有USDC令牌，包括Balancer池中的令牌。

### 发展路线图是什么？ - What is the development roadmap?

We are working on putting together a more detailed roadmap. The [bronze release](https://github.com/balancer-labs/balancer-core/releases/tag/v1.0.0) went live on February 26, 2020. Silver is currently in a design phase and will likely be released in late 2020.

我们正在努力制定更详细的路线图。 [Bronze发布](https://github.com/balancer-labs/balancer-core/releases/tag/v1.0.0) 于2020年2月26日上线。Silver目前处于设计阶段，可能会发布在2020年末。

Bronze, Silver, and eventually Gold releases refer to the "base" Balancer Pool contract - which actually holds the assets.

Bronze，Silver以及最终的Gold发行是指“基本”的“Balancer池”合约-实际上持有资产。

### Balancer协议在协议层收取任何费用吗？ - Does Balancer Protocol charge any fees at the protocol layer?

No. There is a placeholder for an`EXIT_FEE` in the code, but it is currently set to zero - and for technical reasons, this is very unlikely to change. \(In fact, because it is zero, tokens that do not allow zero-value transfers cannot be held in Balancer pools.\)

不。代码中有一个EXIT_FEE占位符，但当前设置为零-出于技术原因，这不太可能更改。\(实际上，因为它为零，所以不允许零值转移的令牌不能保存在Balancer池中。\)

### 有Balancer协议令牌吗？ - Is there a Balancer Protocol token?

Not currently. A token will never be required to trade or interact with the protocol. Any protocol upgrade in that direction would be discussed with the community well in advance.

当前不是。永远不需要令牌来与协议进行交易或交互。在此方向上的任何协议升级都将提前与社区进行讨论。

### 是否有Balancer治理令牌？  - Is there a Balancer Governance Token?

Yes, Balancer Governance Token, [BAL](https://github.com/balancer-labs/docs/tree/1d12c9a19497529cf518328e06d801f8f1c89d7f/protocol/bal-balancer-governance-token/README.md), can be used to vote on proposals and steer the direction of the protocol. Every week 145,000 BALs, or approximately 7.5M per year, are distributed to liquidity providers. They are typically distributed directly to liquidity providers on Tuesdays at 2300 UTC.

是的，可以使用Balancer Governance Token [BAL](https://github.com/balancer-labs/docs/tree/1d12c9a19497529cf518328e06d801f8f1c89d7f/protocol/bal-balancer-governance-token/README.md) 投票并指导协议的方向。每周向流动性提供者分发145,000个BAL，或每年约750万个。通常在周二2300 UTC将它们直接分发给流动性提供者。

* 25M BAL tokens were initially allocated to founders, stock options, advisors and investors, all subject to vesting periods.
* 5M were allocated for the Balancer Ecosystem Fund. This fund will be deployed to attract and incentivize strategic partners that will help the Balancer ecosystem grow and thrive. BAL holders will ultimately decide how this fund is used over the coming years.
* 5M were allocated for the Fundraising Fund. Balancer Labs raised a pre-seed and seed round. This fund will be used for future fundraising rounds to support Balancer Labs' operations and growth. BAL tokens will never be sold to retail investors.
* The remaining 65M tokens are intended to be mostly distributed to liquidity providers in the coming years.

* 最初有2500万枚BAL代币分配给创始人，股票期权，顾问和投资者，所有这些都必须归属。
* 500万分配给了均衡器生态系统基金。该基金将用于吸引和激励战略合作伙伴，这将有助于Balancer生态系统的发展和繁荣。 BAL持有人将最终决定在未来几年中如何使用这笔资金。
* 500万分配给了筹款基金。平衡器实验室提出了种子前和种子轮。该资金将用于未来的筹款活动，以支持Balancer Labs的运营和成长。 BAL代币将永远不会出售给散户投资者。
*剩余的6500万令牌将在未来几年主要分配给流动性提供商。

## Balancer池 - Balancer Pools

### 什么是Balancer池 - What is a Balancer Pool?

The fundamental building block of the Balancer Protocol is the Balancer Pool. Pools are smart contracts that implement the Balancer Protocol, and hold value in two or more ERC20 tokens.


Balancer协议的基本构建块是Balancer池。Balancer池是实现Balancer协议的智能合约，并在两个或多个ERC20代币中持有价值。

You can think of a Balancer Pool as an automated, market-making portfolio. Each token asset has an independent weight, and can be traded against any other token in the pool. For example, you could have a pool with three tokens in the following proportions 50% WETH, 25% MKR and 25% DAI.

您可以将Balancer Pool视为自动化的做市组合。每个令牌资产都具有独立的权重，可以与池中的任何其他令牌进行交易。例如，您可能有一个包含三个令牌的池，令牌的比例为50％WETH，25％MKR和25％DAI。

The value proposition of Balancer flows from two main features:

Balancer的价值主张源于两个主要特征：

1. Even as the relative unit prices of the tokens vary, the pool as a whole is continuously rebalanced \(in an efficient market\) to maintain each token's proportion of the total value. 
2. Each trade that takes place in a Balancer Pool generates a fee for the pool owner. The fee is a percentage of the trading volume, and is customizable by the pool owner when the pool is created.


1. 即使令牌的相对单位价格有所变化，池的整体也会（在有效市场中）不断重新平衡，以维持每个令牌在总价值中所占的比例。
2. 在Balancer池中进行的每笔交易都会为池所有者产生费用。费用是交易量的百分比，由池所有者在创建池时可以自定义。

Thus the incentives of both participants are aligned. Liquidity providers earn trading fees, while the overall value of their portfolio is preserved through continuous rebalancing. Traders pay these fees for the opportunity to either swap tokens with low slippage, or profit from arbitrage opportunities between pools and the open market.

因此，两个参与者的动机是一致的。流动性提供者赚取交易费，而其投资组合的整体价值则通过持续的再平衡得以保留。交易者支付这些费用是为了有机会交换低滑点的代币，或从池和开放市场之间的套利机会中获利。

### 设置Balancer池是否有限制 - Are there constraints for setting up a Balancer Pool?

Only a few. Balancer Protocol limits pools in the following ways:

只有一点。Balancer协议通过以下方式限制池：

* Number of tokens: pools must contain at least two, and may contain up to eight tokens.
* Swap fee: the fee must be between 0.0001% and 10% 
* ERC20 compliance: pool tokens must be ERC20 compliant. Bronze does not support ERC20 tokens that do not return `bools` for `transfer` and `transferFrom`. Future releases may allow non-standard ERC20's.
* There are a few additional ratio and balance constraints that can be found at [Limitations](../protocol/limitations.md)


* 令牌数：池必须至少包含两个，并且最多可以包含八个令牌。
* swap费：费用必须在0.0001％到10％之间
* 符合ERC20：池令牌必须符合ERC20。 Bronze不支持ERC20令牌，这些令牌不会为transfer和transferFrom返回布尔值。将来的版本可能允许使用非标准的ERC20。
* 在[限制](../protocol/limitations.md)中可以找到一些其他比率和余额约束。

### Balancer池如何不断进行重新平衡 - How are Balancer Pools continuously rebalanced?

We believe this is the main innovation introduced by the Balancer Protocol. Pools are efficiently rebalanced through a multi-dimensional invariant function used to continuously define swap prices between any two tokens in a pool. Essentially, it is an n-dimensional generalization of Uniswap's x \* y = k formula.

我们认为这是Balancer Protocol引入的主要创新。通过多维不变函数有效地重新平衡池，该函数用于连续定义池中任何两个代币之间的掉期价格。本质上，它是Uniswap的x \* y = k公式的n维推广。

Imagine a portfolio that contains a certain proportion of token A, and its external market price increases. In a conventional rebalancing process, the portfolio manager would need to take action - in this case, pay a trading fee to sell token A \(and probably pay another fee to buy something else to replace it\).

想象一个包含一定比例代币A的投资组合，其外部市场价格上涨。在传统的重新平衡过程中，投资组合经理将需要采取行动-在这种情况下，支付交易费用来出售代币A \(并可能还支付另一笔费用来购买其他代币A \)。

In contrast, Balancer Protocol lets rational market actors actively buy token A from the pool, presumably to sell elsewhere on the open market at a profit. The portfolio manager \(Balancer Pool creator in this context\) does nothing but collect the fees.


相比之下，平衡器协议让理性的市场参与者主动从集合中购买代币A，大概是在公开市场上的其他地方获利出售。投资组合经理\(在此情况下为Balancer Pool创建者\)除了收取费用外什么也不做。

So instead of doing work and _paying_ fees to rebalance their portfolio, Balancer Pool creators _earn_ fees while traders do the rebalancing work for them. Conversely, traders benefit in two ways - high liquidity allows low slippage on "retail trades," and arbitrageurs can directly profit from swings in external market prices.

因此，Balancer池创建者不必做工作和支付费用来重新平衡他们的投资组合，而是赚取费用，而交易员则为他们进行重新平衡工作。相反，交易者从两种方式中受益-高流动性可以降低“零售交易”的滑点，套利者可以直接从外部市场价格波动中获利。

For further technical details, please refer to our [white paper](https://balancer.finance/whitepaper.html).

有关更多技术细节，请参考我们的[白皮书](https://balancer.finance/whitepaper.html)。

### Balancer池如何收取费用是多少？ - How do Balancer Pools charge fees and how much are they?

Balancer pools charge a percentage of the input amount traded for each trade. The fee goes entirely to the Balancer Pool liquidity providers.

Balancer池对每笔交易收取一定百分比的交易额。该费用将全部支付给Balancer Pool流动性提供商

### 有哪些类型的平衡器池？ - What types of Balancer pools are there?

Core Balancer Pools can be controlled or finalized. Essentially, finalized pools are "public" - their parameters are fixed, and anyone can add/remove liquidity and swap tokens. Controlled pools are "private" - their parameters are not fixed, but only the pool creator can add liquidity. See more details in [Core Concepts](../protocol/concepts.md).

核心Balancer池可以控制或最终确定。从本质上讲，最终池是“公共”的，其参数是固定的，任何人都可以添加/删除流动性和交换令牌。受控池是“专用”的-其参数不是固定的，但是只有池创建者才能增加流动性。请参阅[核心概念](../ protocol / concepts.md)中的更多详细信息。

There are also various kinds of Smart Pools - Core Balancer Pools controlled by a smart contract. Balancer is designed to be easily extensible; the Core Concepts page referenced above has some suggestions for Smart Pool designs.

还有各种各样的Balancer池-由智能合约控制的核心Balancer池。Balancer设计为易于扩展；上面引用的“核心概念”页面为智能池设计提供了一些建议。

Some Smart Pool concepts have already been implemented, and many more are in development. The first implementation was actually [PieDAO](https://piedao.org/) - they created a series of "++" tokens \(e.g., BTC++, USD++\): Balancer Pool tokens representing a basket of cryptocurrencies, chosen by the community through a DAO mechanism. Their smart pools control the underlying Balancer Pool, and implement features like value caps, pausing trading, etc.

一些智能池概念已经实现，还有更多正在开发中。第一个实现实际上是[PieDAO](https://piedao.org/)-他们创建了一系列“ ++”令牌\(例如BTC ++，USD ++ \)：表示一篮子加密货币的Balancer Pool令牌，由社区通过DAO机制。他们的智能池控制底层的“Balancer池”，并实现诸如价值上限，暂停交易等功能。

Since Balancer Pool tokens are also compliant ERC20 tokens, they can also be composed into "meta" pools, which also exist \(e.g., [BTC++/USD++](https://pools.balancer.exchange/#/pool/0x7d2f4bcb767eb190aed0f10713fe4d9c07079ee8/)\).

由于Balancer Pool令牌也是兼容的ERC20令牌，因此它们也可以组成“元”池，也存在\(例如[BTC ++ / USD ++](https://pools.balancer.exchange/#/pool/0x7d2f4bcb767eb190aed0f10713fe4d9c07079ee8/)\)。

Balancer Labs will soon release a home-grown Smart Pool called the Configurable Rights Pool. Since Smart Pool creators can \(by definition\) alter the parameters of a "live" Balancer Pool, they are less trustless than Core Balancer Pools. To mitigate this, CRP creators can choose exactly which parameters can be changed, at create time. As the name implies, this is done by granting the Smart Pool contract one or more "Rights" - the right to change one of the parameters. \(A CRP with no rights would be equivalent to a Core Balancer Pool.\)

Balancer Labs即将发布自己开发的智能池，称为可配置权限池。由于智能池创建者可以\(按定义\)更改“实时”平衡器池的参数，因此它们比核心平衡器池更不受信任。为了减轻这种情况，CRP创建者可以在创建时准确选择可以更改的参数。顾名思义，这是通过授予Smart Pool合同一项或多项“权利”（更改参数之一的权利）来完成的。 \(没有权限的CRP等同于Core Balancer Pool。\)

[Ampleforth](https://www.ampleforth.org/) is another example of a smart contract system in development. There is also plenty of innovation in the design of tokens. AMPL is an "Elastic Supply" token - instead of a fixed supply, like Bitcoin, and a volatile price -- AMPL is a kind of stable coin where the peg is maintained by adjusting the supply based on demand. These tokenomics can then interact with Balancer Pool settings in creative ways.


[Ampleforth](https://www.ampleforth.org/)是开发中的智能合约系统的另一个示例。令牌的设计也有很多创新。 AMPL是一种“弹性供应”代币-而不是像比特币这样的固定供应和波动的价格-AMPL是一种稳定的代币，通过根据需求调整供应来维持挂钩。然后，这些代币经济学可以创造性地与Balancer Pool设置进行交互。

## 使用Balancer协议 - Using Balancer Protocol

### 如何使用Balancer作为交易者 - How can I use Balancer as a trader?

There are two ways a trader can interact with Balancer Protocol:

交易者可以通过两种方式与Balancer协议进行交互：

* Through our [Exchange](https://balancer.exchange/#/swap) front-end
* Directly through our smart contracts on Ethereum Mainnet


* 通过我们的 [Exchange](https://balancer.exchange/#/swap) 前端
* 直接通过我们在以太坊主网上的智能合约

### 如何将Balancer用作流动性提供者或投资组合经理？ - How can I use Balancer as a liquidity provider or portfolio manager?

There are two main ways a liquidity provider or portfolio manager can interact with Balancer Protocol:

流动性提供者或投资组合经理可以通过两种主要方式与Balancer Protocol进行交互：

* Through our [Pool Manager](https://pools.balancer.exchange/#/) front-end \(available shortly\)
* Directly through our smart contracts on Ethereum Mainnet


* 通过我们的[Pool Manager](https://pools.balancer.exchange/#/) 前端\(不久即可使用\)
* 直接通过我们在以太坊主网上的智能合约

Please [contact us](mailto:contact@balancer.finance) if you have a portfolio worth more than 100,000 USD and need special assistance.

如果您的投资组合价值超过100,000美元，并且需要特殊帮助，请[联系我们](mailto:contact@balancer.finance) 

### 我如何申请BAL代币作为流动性提供者? - How can I claim BAL tokens as a Liquidity Provider?

Head over to [https://claim.balancer.finance/](https://claim.balancer.finance/) to claim your BAL from liquidity mining.

前往 [https://claim.balancer.finance/](https://claim.balancer.finance/) 要求您从流动性开采中获得BAL。

BAL tokens for liquidity mining of any given week will be available to be claimed the following Tuesday evening \(EST time\).

在任何给定的一周中进行流动性开采的BAL代币将在下个星期二晚上\(美国东部时间\)获得。

This claiming mechanism will allow you to choose if you want to claim on a weekly basis or accrue and claim at a later date. At this time, BAL earned through liquidity mining does not expire, so you can take your time to claim them. If for some reason this changes in the future, everyone will be notified well in advance.

通过这种声明机制，您可以选择是否要每周声明一次，还是要在以后的某个日期进行累计和声明。目前，通过流动性挖掘获得的BAL不会过期，因此您可以花些时间要求它们。如果将来由于某种原因这种情况发生变化，则会提前通知所有人。

### Balancer可以用作指数基金吗? - Can Balancer be used as an Index Fund?

Balancer Protocol is ideal for Index Funds, as it takes away all the hassle that managers have regarding rebalancing - and on top of that generates fees from trading.

Balancer协议是指数基金的理想之选，因为它消除了管理人员在重新平衡方面的所有麻烦，并且最重要的是从交易中产生了费用。

## 杂项 - Miscellaneous

### 使用Balancer协议是否会产生应税事件么？ - Does using Balancer Protocol generate taxable events?

We cannot provide tax or accounting advice. Tax regulations are specific to jurisdiction where you or your company reside. For any legal or tax matters we recommend consulting your own attorney.

我们无法提供税务或会计建议。税收法规特定于您或您公司所在的司法管辖区。对于任何法律或税务问题，我们建议您咨询自己的律师。

### 使用Balancer协议是否有风险？ -  Are there risks in using Balancer Protocol?

Balancer Protocol smart contracts have been designed with security as a top priority. The core protocol code has been reviewed by Consensys Diligence, and formally audited by both [Trail of Bits](https://github.com/balancer-labs/balancer-core/blob/master/Trail%20of%20Bits%20Full%20Audit.pdf) and [Open Zeppelin](https://blog.openzeppelin.com/balancer-contracts-audit/). \(Of course, we cannot guarantee that bugs won’t be found in the future.\)

Balancer协议智能合约的设计以安全性为重中之重。核心协议代码已由Consensys Diligence审查，并由[Trail of Bits](https://github.com/balancer-labs/balancer-core/blob/master/Trail%20of%20Bits%20Full%20Audit.pdf)和[打开Zeppelin](https://blog.openzeppelin.com/balancer-contracts-audit/)正式审核。 \(当然，我们不能保证以后不会发现错误。\)

Balancer Pools are not [upgradeable](https://medium.com/consensys-diligence/upgradeability-is-a-bug-dba0203152ce) \(though other third-party Smart Pool implementations might be\), and there are no admin keys or backdoors. 

Balancer池不[可升级](https://medium.com/consensys-diligence/upgradeability-is-a-bug-dba0203152ce)\(尽管其他第三方智能池实现可能\)，并且没有管理员钥匙或后门。

Remember that the tokens held in Balancer Pools are also smart contracts - not controlled by Balancer - and may have their own risks. Balancer does not support non-ERC20-conforming tokens, but pools may have been created that use them anyway.

请记住，Balancer池中持有的代币也是智能合约，不受Balancer控制，并且可能存在风险。 Balancer不支持不符合ERC20的令牌，但是可能已经创建了使用它们的池。

The CRP contains safeguards to prevent categories of tokens with known issues from being used in pools \(e.g., non ERC20-conforming tokens, and tokens that disallow zero-transfers\), and further safeguards to allow other kinds of tokens to interact safely with the protocol \(e.g., tokens with callbacks, or those which require 0 prior spend approval\).

CRP包含防止在池中使用具有已知问题的令牌类别的保护措施\(例如，不符合ERC20的令牌和不允许零转移的令牌\)，还包括允许其他类型的令牌与之安全交互的保护措施。协议\(例如，带有回调的令牌，或需要0个预先支出批准的令牌\).

Since Smart Pool operators can, by definition, alter the parameters of the pool during active trading, all require some level of trust in the pool creators \(beyond the general smart contract risk\) - the more parameters they can change, the more trust is required.

根据定义，由于智能池操作员可以在活跃交易期间更改池的参数，因此所有人都需要对池创建者具有一定程度的信任（超出了智能合约的一般风险）-他们可以更改的参数越多，信任度就越高。是必须的。

As a liquidity provider, there is also the financial risk of [Impermanent Loss](https://medium.com/balancer-protocol/calculating-value-impermanent-loss-and-slippage-for-balancer-pools-4371a21f1a86).

作为流动性提供者，还存在[永久损失](https://medium.com/balancer-protocol/calculating-value-impermanent-loss-and-slippage-for-balancer-pools-4371a21f1a86)的财务风险.

