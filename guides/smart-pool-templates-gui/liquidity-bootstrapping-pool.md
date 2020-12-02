# 流动性引导池 - Liquidity Bootstrapping Pool

Here again is the [article](https://balancer.finance/2020/03/04/building-liquidity-into-token-distribution/) describing this common case - basically a sustainable 2020 reboot of the 2017 "ICO" - minus the legal and regulatory issues.

[文章](https://balancer.finance/2020/03/04/building-liquidity-into-token-distribution/)再次描述了这种常见情况-基本上是2017年“ ICO”的2020年可持续重启-减去法律和法规问题。

The idea is to launch a token with low capital requirements, by setting up a two-token pool with a project and a collateral token. The weights are initially set heavily in favor of the project token, then gradually "flip" to favor the collateral coin by the end of the sale. The sale can be calibrated to keep the price more or less steady \(maximizing revenue\), or declining to a desired minimum \(e.g., the initial offering price\).

这个想法是通过建立一个带有项目和抵押代币的两个代币池来启动具有较低资本要求的代币。权重最初设置为偏重于项目代币，然后逐渐“翻转”以在销售结束时偏向抵押代币。可以对销售进行校准，以使价格或多或少保持稳定\(使收益最大化\)，或降低到所需的最低价格\(例如初始发行价格\).

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* _canPauseSwapping_
* ~~canChangeSwapFee~~
* **canChangeWeights**
* _canAddRemoveTokens_
* **canWhitelistLPs**
* ~~canChangeCap~~

Pausing swapping is an optional feature here. You might want to halt trading for various reasons \(e.g., stronger than expected demand is driving up the price, and people are selling tokens back to the pool for profit instead of buying them\). There isn't much of a trust issue here - the purpose is to sell tokens, so the pool operator has an incentive to leave swapping enabled.

暂停交换是此处的可选功能。您可能出于各种原因而暂停交易(例如，比预期强的需求推动了价格上涨，人们将代币出售回池以获取利润而不是购买代币\)。这里没有太多的信任问题-目的是出售代币，因此，池操作员有动机启用交换功能。

In a token sale, you want to keep fees low to encourage trading; there's really no reason to change it. So best practice would be set it to the minimum on creation and don't enable changing it. You want to enable as few rights as possible. More rights = more possible manipulation from the pool operator = more trust required.

在代币销售中，您希望保持较低的费用以鼓励交易。确实没有理由更改它。因此，最佳做法是在创建时将其设置为最小，并且不要启用对其进行更改。您想要启用尽可能少的权限。更多的权限=来自池运算符的更多可能的操作=需要更多的信任。

Since the core of the strategy is changing weights, you absolutely need to enable that right.

由于该策略的核心是改变权重，因此您绝对需要启用该权利。

The add/remove tokens right depends on your future intentions, since there are two ways to remove your liquidity at the conclusion of the sale. If this is a one-off auction, you can enable the right, and redeem through two "removeToken" calls. This is simple, but destroys the pool.

添加/删除代币的权利取决于您未来的意图，因为有两种方法可以在交易结束时删除您的流动性。如果这是一次性拍卖，则可以启用权利，并通过两个“ removeToken”调用进行赎回。这很简单，但是会破坏池。

If you want to re-use the pool \(e.g., there are "phases" or multiple releases\), you can leave this right disabled, and retrieve the proceeds through repeated calls to exitPool \(1/3 at a time, due to token ratio constraints\). To add more project tokens, you would need to add yourself to the whitelist, and joinPool.

如果要重新使用池\(例如，存在“阶段”或多个版本\)，则可以禁用此权限，并通过重复调用exitPool \(每次1/3来检索收益）令牌比例约束\)。要添加更多项目令牌，您需要将自己添加到白名单中，然后加入joinPool。

In a token sale \(vs an investment pool\), you don't want anyone else providing liquidity. Only the pool creator should be issued pool tokens, and only they should be able to redeem the proceeds at the end. This is most easily accomplished by using the whitelist. If you enable this right and don't add anyone to the whitelist, no one can "join" the pool.

在代币销售\(相对于投资池\)中，您不希望其他任何人提供流动性。仅向池创建者发放池令牌，并且只有它们才能在最后赎回收益。使用白名单最容易做到这一点。如果启用此权限并且不将任何人添加到白名单，则没有人可以“加入”该池。

You could also prevent others from adding liquidity through the cap right. If you enable it, the cap will be set to the initial supply, which has the same effect as the whitelist. However, you will need to be careful when you redeem the pool tokens at the end of the sale, since any token redemption would reduce the supply below the cap and present a window for others to join the pool. To prevent this, you could set the cap to 0 before withdrawing the proceeds.

您还可以防止其他人通过上限权利增加流动性。如果启用它，则上限将设置为初始供应，其作用与白名单相同。但是，在销售结束时赎回泳池代币时，您将需要小心，因为任何代币兑换都会将供应减少到上限以下，并为其他人加入泳池提供一个窗口。为避免这种情况，您可以在提取收益之前将上限设置为0。

Watch out for some subtleties here! Some LBP owners might want to allow public LPs; Uniswap does, after all. If you don't have the whitelist right or the cap right, anyone can add liquidity. This means the LBP controller is not the sole "owner" of the pool, since others also have pool tokens. Consequently, if the controller tries to destroy the pool after the sale with calls to removeToken -- it won't work, because they won't have enough pool tokens. This makes sense - otherwise, pool owners could "drain the pool" just by calling `removeToken`!

注意这里的一些细微之处！一些LBP所有者可能希望允许公共LP。毕竟，Uniswap可以。如果您没有白名单权限或上限权限，那么任何人都可以增加流动性。这意味着LBP控制器不是池的唯一“所有者”，因为其他人也具有池令牌。因此，如果控制器在销售后试图通过调用removeToken销毁池，则它将无法工作，因为它们将没有足够的池令牌。这很有道理-否则，池所有者可以仅通过调用`removeToken`来“耗尽池”！

Depending on the balances, removing one of the tokens "might" work. For instance, say you're at the end of the sale, so you have a small balance of your project token, and a large balance of DAI. You would probably have enough pool tokens to remove the project token. At that point, you would have a 1-token pool of DAI, and -- unless all the other LPs exit the pool - you would not be able to call `removeToken` on DAI. You could only exitPool with whatever pool tokens you had - and the remaining DAI balance would represent the amount of liquidity added by the other LPs.

根据余额，删除令牌之一可能“起作用”。例如，假设您已经售完了，那么您的项目代币余额很小，而DAI余额很大。您可能有足够的池令牌来删除项目令牌。那时，您将拥有一个1令牌的DAI池，并且-除非所有其他LP都退出该池-您将无法在DAI上调用`removeToken`。您只能使用拥有的任何池令牌退出ExitPool-剩余的DAI余额将代表其他LP所增加的流动性。

So the upshot of all this is if you intend to allow public LPs on your LBP, maybe you don't need the add/remove token right. \(This would make your pool more trustless.\)

因此，所有这一切的结果是，如果您打算在LBP上允许公共LP，则也许您不需要添加/删除令牌权限。 \(这会使您的池更加不可信。\)

It could also happen that you don't have enough to call `removeToken` on either. For instance, if a purchaser sold a large amount of your project token back into the pool, or someone added a large amount of liquidity. If you need to call `removeToken` to reuse the pool, remember you can always "buy out" the other LPs. If you're 250 pool tokens short of what you need to remove the project token, you can join with 250 pool tokens' worth of DAI, after which `removeToken` will succeed. All the other LPs wishing to withdraw their liquidity would get their proceeds entirely in DAI.

您也可能没有足够的电话来调用`removeToken`。例如，如果购买者将大量的项目代币卖回了资金池，或者某人增加了大量的流动性。如果您需要调用`removeToken`来重用池，请记住您可以随时“买断”其他LP。如果您缺少删除项目令牌所需的250个池令牌，则可以加入250个池令牌的DAI，之后`removeToken`将成功。所有其他希望撤回其流动资金的有限合伙人，其收益将全部来自DAI。

This is something to keep in mind when investing in Balancer Pools in general - when you withdraw liquidity, you will get the token\(s\) in whatever proportion they are in at the time of withdrawal. If the pool controller has the add/remove token right, they might not even be the same tokens. \(This would surely be the case for "perpetual synthetic" pools that add and remove synthetics as they are minted and expire.\)

一般而言，在平衡池中进行投资时要记住这一点-撤回流动性时，您将获得撤回时所占比例的代币。如果池控制器具有添加/删除令牌权限，则它们甚至可能不是相同的令牌。 \(“永久性合成”池肯定会出现这种情况，因为它们在合成物铸造和过期时会添加和删除合成物。\)

One other important consideration is the "block time" settings - the minimum time between weight updates, and the minimum duration of each gradual update. These times default to 2 hours and 2 weeks, meaning the weights cannot change faster than every 2 hours, and once you start a gradual update, you cannot add a token or do a manual update for 2 weeks.

另一个重要的考虑因素是“阻滞时间”设置-两次体重更新之间的最短时间，以及每次逐步更新的最短持续时间。这些时间默认为2小时2周，这意味着权重变化不能快于每2小时一次，并且一旦开始逐步更新，就无法添加令牌或手动进行2周的更新。

You can make these periods arbitrarily short \(even zero\), but then the pool operators have greater control, and more trust is required.

您可以使这些时间段任意短\(甚至为零\)，但是池操作员将拥有更大的控制权，并且需要更多的信任。

Also keep in mind that contracts cannot change state by themselves - weights only change when someone calls `pokeWeights`. A common mechanism is for the pool operator to set up something like a cron job to call it automatically, though it is a public function that anyone can call.

同样要记住，合同不能自行改变状态-权重仅在有人调用`pokeWeights`时改变。池操作员的通用机制是设置诸如cron作业之类的内容以自动调用它，尽管它是任何人都可以调用的公共函数。

Note that `pokeWeights` changes weights, but not balances - therefore, it can change the price \(albeit only along the trajectory set by the last call the updateWeightsGradually\).

请注意，`pokeWeights`会更改权重，但不会更改余额-因此，它可以更改价格\(尽管仅沿最后一次调用updateWeightsGradually 设置的轨迹移动)。

The controller function `updateWeight` changes weights directly - but not the price. This means it must also adjust the balances at the same time \(i.e., transfer tokens\). So the controller can set weights arbitrarily -- but not while an `updateWeightsGradually` is running. And they must have the tokens available to do it.

控制器函数`updateWeight`直接更改权重-但不更改价格。这意味着它还必须同时\(即转移令牌\)调整余额。因此，控制器可以任意设置权重-但不能在运行`updateWeightsGradually`时设置。并且他们必须具有可用的令牌来做到这一点。