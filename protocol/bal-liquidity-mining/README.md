# BAL流动性挖矿 - BAL Liquidity Mining

## BAL与Balancer上的流动性成正比 - BAL Distribution Proportional to Liquidity on Balancer <a id="353e"></a>

To make the token distribution as fair as possible, we distribute BAL tokens proportional to the amount of liquidity each address contributed, relative to the total liquidity on Balancer. Since there is liquidity in several different tokens, we use the USD value as the common measure.

为了使代币分配尽可能公平，我们将BAL代币分配成与每个地址贡献的流动性数量（相对于Balancer的总流动性）成比例。由于几种不同的代币都有流动性，因此我们将美元价值作为通用指标。

Head over to [https://claim.balancer.finance/](https://claim.balancer.finance/) to claim your BAL from liquidity mining.

前往[https://claim.balancer.finance/](https://claim.balancer.finance/)要求您从流动性挖矿中获得BAL。

In practice, every week Balancer Labs has to:

实际上，Balancer Labs每周必须：

* Define the starting and ending block of the week. Both are chosen as the block with the closest timestamp to a fixed weekly time \(e.g. Sunday 1:00pm UTC\). For example, the starting block for a given week might be \#10,100,000 and the ending block \#10,140,000. - 定义一周的开始和结束时间。两者都被选为时间戳记最接近固定每周时间\(例如，UTC的星期日1:00 pm\)的块。例如，给定周的开始块可能是\#10,100,000，结束块可能是\#10,140,​​000。
* Define snapshot blocks, every 256 blocks \(roughly hourly\) counting backwards from the ending block until the starting block. For the example above, the snapshot blocks would be \#10,140,000, \#10,139,744, \#10,139,488, and so on. - 定义快照块，每256个块\(大约每小时一次\)从结束块向后计数，直到开始块为止。对于上面的示例，快照块将是\#10,140,​​000，\#10,139,744，\#10,139,488，依此类推。
* For each snapshot block, and for each Balancer pool, get the USD price of the tokens in the pool from [CoinGecko](https://www.coingecko.com/api/documentations/v3#/contract/get_coins__id__contract__contract_address__market_chart_), and calculate the total USD liquidity. - 对于每个快照块和每个Balancer池，请从[CoinGecko](https://www.coingecko.com/api/documentations/v3#/contract/get_coins__id__contract__contract_address__market_chart_)获取池中令牌的美元价格。美元总流动资金。
* Since liquidity in pools that have lower trading fees contribute more to protocol usage than liquidity in pools with higher fees, we multiply the USD pool liquidity by a feeFactor that down-weights pools according to their fee percentage: - 由于交易费用较低的池中的流动性比费用较高的池中的流动性对协议使用的贡献更大，因此我们将美元池的流动性乘以一个费用因子，该因子根据费用的百分比对池进行加权：

![](../../.gitbook/assets/fee_factor_calc.png)

The constant, `k`, was initially set to 0.5, but beginning in week 8 \(July 20th 00:00 UTC\), the community approved a [proposal to to change `k` to 0.25](https://forum.balancer.finance/t/modifying-feefactor-toward-reducing-the-mining-penalty-for-high-fee-pools/103) in order to ease the penalty for higher-fee pools. This creates the following bell-shaped curve for feeFactor, which means, for example, that a pool with a 0.5% fee has a feeFactor of ~0.98, a pool with a 1% fee has a feeFactor of ~0.94, and a pool with a 2% fee has a feeFactor of ~0.78:

常数 `k`最初设置为0.5，但从第8周\(UTC 7月20日00:00\)开始，社区批准了一项[将k更改为0.25的建议](https://forum.balancer.finance/t/modifying-feefactor-toward-reducing-the-mining-penalty-for-high-fee-pools/103) ，以减轻高收费池的罚款。这会为FeeFactor创建以下钟形曲线，这意味着，例如，费用为0.5％的池的FeeFactor为〜0.98，费用为1％的池的FeeFactor为〜0.94，以及2％的手续费会产生一定的影响因素〜0.78：

![](../../.gitbook/assets/fee_factor_plot.png)

* **第2周更新 - UPDATED for week 2** \(starting June 8th 00:00 UTC\), multiply the pool liquidity by a `ratioFactor`. Since pools that are imbalanced contribute less to trading volume \(because the slippage is higher\), the community approved a [proposal to add a ratioFactor](https://forum.balancer.finance/t/introduction-of-a-weight-ratio-factor-in-liquidity-mining/15). This way highly imbalanced pools \(such as those with 98%/2% weights\) have a much lower weight in the final BAL distribution. - 将池流动性乘以`ratioFactor`。由于不平衡的池对交易量的贡献较小（因为滑点较高），因此社区批准了[提议添加ratioFactor的提议](https://forum.balancer.finance/t/introduction-of-a-weight-ratio-factor-in-liquidity-mining/15)。这样，高度不平衡的池\(例如权重为98％/2％的池\)在最终BAL分布中的权重要低得多。
* **第3周更新 - UPDATED for week 3** \(starting June 15th 00:00 UTC\), multiply the pool liquidity by a `wrapFactor`. Since pools containing pairs of tokens that have a hard peg \(e.g. DAI and cDAI\) do not contribute much trading volume \(because traders can wrap DAI for cDAI and vice-versa\), the community approved a [proposal to add a wrapFactor](https://forum.balancer.finance/t/wrapfactor-penalizing-pairs-of-equivalent-tokens-in-liquidity-mining/28/3). This way, liquidity in such pairs \(like cETH/ WETH\) has a 0.1 wrapFactor \(i.e. counts 10 times less than for other regular pairs\), reducing the amount of BAL received by their liquidity providers. In week 8 \(starting July 20th 00:00 UTC\), the community approved a [proposal to add a wrapFactor for soft pegged pairs](https://forum.balancer.finance/t/modifying-wrapfactor-applying-a-0-7-factor-to-soft-pegged-pairs/108). While for hard pegs, the factor is 0.1, for soft pegs it is 0.7 \(much less harsh\). A soft pegged pair is one in which the two assets are not directly convertible, but they do track the same underlying asset's price by design. Examples include a pair of USD stable coins \(e.g. DAI and USDC\) or a synthetic paired with its real-world asset \(e.g. sETH and WETH\). - 
将池流动性乘以`wrapFactor`。由于包含具有稳定\(例如DAI和cDAI\)的成对代币的池不会贡献太多的交易量\(因为交易者可以将DAI换成cDAI，反之亦然\)，因此社区批准了[建议添加wrapFactor](https://forum.balancer.finance/t/wrapfactor-penalizing-pairs-of-equivalent-tokens-in-liquidity-mining/28/3)。这样，此类货币对\(如cETH / WETH \)的流动性为0.1 wrapFactor \(即比其他常规货币对的计数少10倍)，从而减少了其流动性提供者收到的BAL数量。在第8周（从世界标准时间7月20日00:00开始），社区批准了[proposal to add a wrapFactor for soft pegged pairs](https://forum.balancer.finance/t/modifying-wrapfactor-applying-a-0-7-factor-to-soft-pegged-pairs/108)。对于稳定币，系数是0.1，而波动币，系数是0.7\(少得多)。软挂钩货币对是两个资产不能直接转换的货币对，但是它们确实通过设计跟踪了相同基础资产的价格。例子包括一对稳定的美元硬币\(例如DAI和USDC\)或与其实际资产\(例如sETH和WETH \)成对的合成货币。
* **第8周更新 - UPDATED for week 8** \(starting July 20th 00:00 UTC\), multiply the pool liquidity by a `balFactor`. To incentivize liquidity of the native governance token, BAL, the community approved a [proposal to add a balFactor](https://forum.balancer.finance/t/balfactor-incentivizing-bal-liquidity-on-balancer/102/4). Deep liquidity of the BAL token ensures a more equitable distribution of governance power, by allowing new users to buy BAL with lower slippage, and discouraging existing holders from hoarding their tokens. The `balFactor` implementation applies a 2x multiplier to the BAL portion of each trading pair containing both BAL and one of the uncapped tokens \(`ETH, DAI, USDC, WBTC`\). This, combined with `ratioFactor`, creates a maximum possible total factor of ~1.54 for a pool containing 58% BAL and 42% of one of the uncapped tokens. - 将池流动性乘以`balFactor`。为了激励本地治理令牌BAL的流动性，社区批准了[添加balFactor的提案](https://forum.balancer.finance/t/balfactor-incentivizing-bal-liquidity-on-balancer/102/4)。 BAL代币的高流动性允许新用户以较低的滑点购买BAL，并阻止现有持有者ing积其代币，从而确保更公平地分配治理权。 balFactor的实现对每个交易对的BAL部分应用2倍乘数，该交易对既包含BAL又包含未封盖代币\(`ETH, DAI, USDC, WBTC`\).结合 `ratioFactor`，对于包含58％BAL和42％的未封盖令牌的池，创建的最大可能总因子约为1.54.
* **第9周更新 - UPDATED for week 9** \(starting June 29th 00:00 UTC\), after the liquidity of all tokens is adjusted as usual by the currently active factors \(ratio, wrap, fee\), a `capFactor` has [been proposed](https://forum.balancer.finance/t/capfactor-capping-eligible-liquidity-to-10m-per-token/56), which is calculated such that every capped token is **limited to a maximum of $10M in adjusted liquidity**. `capFactor` is then applied to the liquidity of each affected capped token, resulting in an adjusted liquidity for every pool containing those tokens.`ETH, DAI, USDC, WBTC, BAL` is the list of uncapped tokens. Please read the proposal linked above for further details and examples. - 在所有令牌的流动性按照当前活跃因素\(比率，自动换行，费用\)照常进行调整后，`capFactor`[被提出](https://forum.balancer.finance/t/capfactor-capping-eligible-liquidity-to-10m-per-token/56)，其计算为使得每一个封端的令牌 **调整后的流动资金上限为1000万美元**。然后将`capFactor` 应用于每个受影响的有上限代币的流动性，从而对包含这些代币的每个池调整后的流动性.ETH，DAI，USDC，WBTC，BAL是未上限代币的列表。请阅读上面链接的提案，以获取更多详细信息和示例。
* Calculate the proportional, adjusted, and capped \(see `capFactor` above\) liquidity USD value that each liquidity provider has in the pool. The table below shows an example for a pool that has 100$ worth of liquidity \(already adjusted by all factors\):  -  [计算每个流动性提供者在池中拥有的比例\(已调整和有上限)的\(请参见上面的`capFactor`\)。下表显示了一个池的示例，该池具有100美元的流动资金\(已由所有因素调整\)：]

![](https://miro.medium.com/max/1472/1*2EM2KXgvt48qVK8FKQRmcw@2x.png)

* Divide the weekly amount of BALs distributed by the number of snapshot blocks. Considering blocks lasting 15s, a week would have a total of 40,320 blocks \(=7\*24\*60\*60/15\). Of these, there would be 158 snapshot blocks \(=40,320/256\). With 145,000 BAL distributed per week, the number of BAL distributed per snapshot block would be approximately 918 \(=145,000/158\). - 将每周分发的BAL数量除以快照块数量。考虑到持续15 s的块，一周将总共有40,320块 \(=7\*24\*60\*60/15\)。其中，将有158个快照块\(=40,320/256\)。每周分配145,000 BAL，每个快照块分配的BAL数量约为918 \(=145,000/158\)。
* For each snapshot block, calculate the number of BAL tokens allocated to each address. This is calculated for each address proportional to the total liquidity of that account \(considering all pools they've contributed to\), divided by the total protocol liquidity. The table below shows an example of the final distribution for a snapshot block. - [对于每个快照块，计算分配给每个地址的BAL令牌的数量。这是针对每个地址计算的，该地址与该帐户的总流动性\(考虑到它们贡献的所有池\)除以总协议流动性成比例。下表显示了快照块的最终分配示例。]

![](https://miro.medium.com/max/1492/1*MvfWrMI2PovCLJiwaQr6EQ@2x.png)

All the calculations described above depend exclusively on on-chain data and historical token prices openly accessible on CoinGecko. This whole calculation process is fully auditable via [an open source script](https://github.com/balancer-labs), and BAL distribution results will be posted to IPFS and referenced on Balancer’s [twitter account](https://twitter.com/BalancerLabs). 

上述所有计算仅取决于链上数据和CoinGecko上可公开获取的历史代币价格。整个计算过程可以通过[一个开放源代码脚本](https://github.com/balancer-labs)进行完全审计，并且BAL分配结果将发布到IPFS，并在Balancer的[twitter帐户](https://twitter.com/BalancerLabs)中引用.

## 代币白名单和流动性挖掘的资格池 - Token Whitelist and Eligible Pools for Liquidity Mining <a id="84fc"></a>

**第9周更新 - UPDATED for week 9** \(starting June 29th 00:00 UTC\): All tokens present in the whitelist created by the community \(see whitelist proposal\) are eligible for BAL liquidity mining. The [most up to date list](https://github.com/balancer-labs/assets/blob/master/lists/eligible.json) is maintained on Balancer's Github. All tokens listed under the `"homestead"` list in the json file linked are eligible. This list will evolve over time with input from the community. - 社区\(请参阅白名单提案\)创建的白名单中存在的所有代币都有资格进行BAL流动性挖掘。[最新列表](https://github.com/balancer-labs/assets/blob/master/lists/eligible.json)保留在Balancer的Github上。链接的json文件中`"homestead"`列表下列出的所有令牌均符合条件。该列表将随着社区的发展而不断发展。

Only Balancer pools containing two or more whitelisted tokens will be eligible for BAL liquidity mining.

只有包含两个或多个列入白名单的代币的Balancer池才有资格进行BAL流动性挖掘。

{% hint style="danger" %}
Note that if you've created a [Smart Pool](../../smart-contracts/smart-pools/configurable-rights-pool.md) eligible for BAL rewards - without going through our standard factory - you must apply to either redirect them to a regular account \(e.g., for a single-LP, private pool\), or redistribute the rewards directly to LPs. \(See the [CRP Tutorial](../../guides/crp-tutorial.md) for more details.\)
{% endhint %}

{% hint style="danger" %}
请注意，如果您创建了具有BAL奖励资格的[智能池](../../smart-contracts/smart-pools/configurable-rights-pool.md) -无需通过我们的标准工厂-您必须申请将其重定向到常规帐户\(例如，对于单个LP，私人池\)，或将奖励直接重新分配给LP。\(有关更多详细信息，请参见[CRP教程](../../guides/crp-tutorial.md)。)
{% endhint %}