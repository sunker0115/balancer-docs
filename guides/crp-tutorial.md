# CRP指南 - CRP Tutorial

Similar to Core Pools, Configurable Rights Pools are created from a public factory. Refer to [Addresses](../smart-contracts/addresses.md) to find the contract on your network of choice.

与核心池类似，可配置权限池是从公共工厂创建的。请参考[地址](../smart-contracts/addresses.md)在您选择的网络上查找合约。

To deploy a new Configurable Rights Pool, call newCRP on the CRPFactory. This function takes three parameters \(cheating a bit, since two of them are big structs\):

要部署新的可配置权限池，请在CRPFactory上调用newCRP。此函数采用三个参数\ (有点作弊，因为其中两个是大型结构\)：

```text
function newCrp(
    address factoryAddress,
    ConfigurableRightsPool.PoolParams calldata poolParams,
    RightsManager.Rights calldata rights
)
    external
    returns (ConfigurableRightsPool)
```

The factoryAddress is that of the BFactory \(see [Core Concepts](../protocol/concepts.md)\). The Configurable Rights Pool is the Smart Pool "wrapper" around the underlying Core Pool \(BPool\). You \(caller of newCRP\) are the controller of the Smart Pool. The Smart Pool is the controller of the Core Pool. So the Smart Pool needs to deploy a Core Pool - and requires the BFactory to do that.

factoryAddress是BFactory 的地址\(请参阅[Core Concepts](../protocol/concepts.md)\)。可配置权限池是围绕基础核心池\(BPool \)的智能池“包装器”。您\(newCRP的调用者\)是智能池的控制器。智能池是核心池的控制器。因此，智能池需要部署一个核心池-并要求BFactory来做到这一点。

This is also a bit of future-proofing. At the moment all the Core Pools are "Bronze," but at some point we will release "Silver" and "Gold" versions of the Core Pools, and corresponding factories to create them.

这也是面向未来的。目前，所有核心池都是“Bronze”，但在某个时候，我们将发布“Silver”和“Gold”版本的核心池，并发布相应的工厂来创建它们。

The next argument is PoolParams - this is where you define the structure and basic parameters of the pool, such as the tokens it will hold, their initial weights and balances, and the swap fee.

下一个参数是PoolParams-您可以在其中定义池的结构和基本参数，例如其持有的tokens，其初始权重和余额以及swap费。

```text
struct PoolParams {
    // Balancer Pool Token (representing shares of the pool)
    string poolTokenSymbol;
    string poolTokenName;
    // Tokens inside the Pool
    address[] constituentTokens;
    uint[] tokenBalances;
    uint[] tokenWeights;
    uint swapFee;
}
```

Since the [Balancer Pool Tokens](../protocol/btoken.md) are themselves ERC20 tokens, they have symbols and names. You can set both when creating your pool.

由于[Balancer Pool令牌](../protocol/btoken.md)本身就是ERC20令牌，因此它们具有符号和名称。创建池时，可以同时设置两者。

The tokens must be addresses of conforming ERC20 tokens. Balances and weights are expressed in Wei - and the weights are denormalized, not percentages. Valid denormalized weights range from 1 to 49, since the maximum total denormalized weight is 50. \(This corresponds to a percentage range from 2% to 98%: 1/\(1+49\) = 2%; 49/\(1+49\) = 98%\)

Token必须是符合ERC20Token的地址。余额和权重以Wei表示-权重是非规范化的，而不是百分比。有效的非规格化权重范围为1到49，因为最大的非规格化总权重为50。\(这对应于2％到98％的百分比范围： 1/\(1+49\) = 2%; 49/\(1+49\) = 98%\)

{% hint style="warning" %}
Note that if you're going to be doing gradual weight updates, using denorm totals near the maximum 50 can be problematic! It is possible for `pokeWeights`to fail if the weights are 45/5, since the contract will never allow the total to go over 50 - even temporarily during an intermediate step. Unless you truly need the full range, best practice is to use a lower total, like 40; e.g.; for 90/10, use 36/4. \(If you need to use extreme weights, put tokens whose weights should go down first in the array; that way the weight reductions will be processed before the increases.\)
{% endhint %}

{% hint style="warning" %}
请注意，如果您要逐步进行权重更新，则使用最大50附近的分母合计可能会有问题！如果权重为45/5，`pokeWeights`可能会失败，因为合同永远不允许总和超过50，即使是在中间步骤中也是如此。除非您确实需要全系列产品，否则最佳实践是使用较低的总数，比如40；例如。;对于90/10，请使用36/4。\(如果需要使用极高的权重，请将权重应降低的令牌放在数组中的首位；这样，权重的减少将在增加之前进行处理。\)
{% endhint %}

The swap fee is also expressed in Wei, as a percentage. For instance, toWei\("0.01"\) means a 1% fee.

交易费用也以百分数表示。例如，toWei\(“0.01”\)表示1％的费用.

Finally, the Rights struct defines the permissions.

最后，“权限”结构定义。

```text
struct Rights {
    bool canPauseSwapping;
    bool canChangeSwapFee;
    bool canChangeWeights;
    bool canAddRemoveTokens;
    bool canWhitelistLPs;
    bool canChangeCap;
}
```

 At this point \(after calling newCRP\), we have a deployed Configurable Rights Object with all its permissions and parameters defined. But we can't do much with it - mainly because there is no Core Pool yet. We need to deploy a new Core Pool, with our Smart Pool as the controller, by calling createPool\(initialSupply\). \(There is also an overloaded version of createPool; more on that later.\) 

 此时\(调用newCRP之后\)，我们已部署了可配置权限对象，其中定义了所有权限和参数。但是我们不能做太多-主要是因为还没有Core Pool。我们需要通过调用createPool \(initialSupply\)部署一个新的Core Pool，以我们的Smart Pool作为控制器。\(还有createPool的重载版本；稍后将对此进行更多介绍。)

We've already defined the tokens and balances we want the pool to hold. When we call createPool with a value for initialSupply, it will mint _initialSupply_ Balancer Pool Tokens \(BPTs\) and transfer them to the caller, simultaneously pulling the correct amount of collateral tokens into the contract. \(They end up in the Core Pool, passed through the CRP.\)

我们已经定义了希望池容纳的令牌和余额。当我们调用带有initialSupply值的createPool时，它将创建_initialSupply_ Balancer池令牌\(BPTs\)并将其转移给调用者，同时将正确数量的抵押令牌引入合约。\(它们最终在核心池中通过CRP传递。\)

To accomplish this, we need to allow the CRP to spend our collateral tokens, before calling createPool. For an example three-token pool, we might write:

为此，我们需要允许CRP在调用createPool之前花费我们的抵押令牌。对于一个三种token的池示例，我们可以这样写：

```text
const MAX = web3.utils.toTwosComplement(-1);

// crpPool was returned from CRPFactory.newCRP()
    
await weth.approve(crpPool.address, MAX);
await dai.approve(crpPool.address, MAX);
await xyz.approve(crpPool.address, MAX);

// consume the collateral; mint and xfer 100 BPTs to caller
await crpPool.createPool(toWei('100'));
```

Now we're in business! The pool will already be set up for public swapping, and depending on the permissions settings, possibly public liquidity provision as well. The Core Pool will show up on the Exchange GUI.

现在我们开始营业！该池将已经设置好以进行公开交易，并取决于权限设置（可能还包括公共流动性提供）。核心池将显示在Exchange GUI上。

{% hint style="info" %}
Refer to [Exchange and Reward Listing](../protocol/bal-liquidity-mining/exchange-and-reward-listing.md) for instructions on adding any new tokens you might be introducing to the Exchange GUI, and making them eligible for BAL governance token rewards. Rewards are distributed weekly, and are not applied retroactively, so you'll need to have the token approved by 00:00 UTC the Monday **before** you launch your pool.
{% endhint %}

{% hint style="info" %}
请参阅[Exchange和奖励列表](../protocol/bal-liquidity-mining/exchange-and-reward-listing.md)，以获取有关添加可能要引入到Exchange GUI的新令牌并使它们符合条件的说明。 BAL治理令牌奖励。奖励是每周分发的，不会追溯应用，因此您需要在启动池之前的周一（星期一）00:00 UTC批准令牌。
{% endhint %}

{% hint style="danger" %}
If your smart pool is eligible for BAL rewards, rewards will be redirected to LPs - as long as you create the pool through our standard factory. If you create a new pool using a different factory, or deploy a pool contract directly, you will need to apply for a redirect or redistribution.

The process for the redirect is to make a pull request to update [this file](https://github.com/balancer-labs/bal-mining-scripts/blob/master/redirect.json) in our script repository with the CRP and your wallet address, along with proof that you own the pool \(e.g., the CRP deployment transaction hash\). Here's an [example request](https://github.com/balancer-labs/bal-mining-scripts/pull/11). \(Redistribution will be similar, but is not yet released.\)
{% endhint %}

{% hint style="danger" %}
如果您的智能池有资格获得BAL奖励，则只要您通过我们的标准工厂创建池，奖励就会重定向到LP。如果使用其他工厂创建新池，或直接部署池合同，则需要申请重定向或重新分配。

重定向的过程是在我们的脚本存储库中发出请求以更新[此文件](https://github.com/balancer-labs/bal-mining-scripts/blob/master/redirect.json)。CRP和您的钱包地址，以及您拥有池\(例如CRP部署交易哈希\)的证明。这是一个[示例请求](https://github.com/balancer-labs/bal-mining-scripts/pull/11)。 \(重新分配将类似，但尚未发布.)
{% endhint %}

{% hint style="warning" %}
On a related note, while it is possible to send tokens directly to a core pool contract - as long as they are in the pool, you they can be recovered with gulp\(\) - this is not the case for smart pools! Any tokens sent directly to the CRP contract will be unrecoverable. \(This can happen in some circumstances even without a direct transfer, such as airdrops to token holders.\)
{% endhint %}

{% hint style="warning" %}
与此相关的是，虽然可以将令牌直接发送到核心池合同-只要它们在池中，就可以使用gulp来恢复它们\(\)-智能池不是这种情况！直接发送到CRP合同的任何令牌都将无法恢复。\(在某些情况下，即使没有直接转移也可能发生，例如向令牌持有者空投。\)
{% endhint %}

Note that there is also an overloaded version of createPool, where you can specify additional parameters related to updateWeightsGradually.

请注意，还有一个createPool的重载版本，您可以在其中指定与updateWeightsGradually相关的其他参数。

```text
function createPool(
    uint initialSupply,
    uint minimumWeightChangeBlockPeriod,
    uint addTokenTimeLockInBlocks
)
```

If the pool you're creating doesn't have permission to change weights, or you accept the default values of the time parameters, you can just use the single-argument version.

如果您正在创建的池无权更改权重，或者您接受时间参数的默认值，则可以仅使用单参数版本。

_initialSupply_ can be set to a value of your choice - within min/max limits \(currently 100 - 1 billion, in ether units\).

可以将 _initialSupply_ 设置为您选择的值-在最小/最大限制\(目前为100-10亿，在以太单位\)内。

_minimumWeightChangeBlockPeriod_ enforces a minimum time between the start and end blocks of a gradual update. _addTokenTimeLockInBlocks_ is used when adding a token \(if you have the AddRemoveTokens permission\), as the minimum wait time between committing a new token, and applying it \(i.e., actually adding it to the pool\). There is one additional constraint: _minimumWeightChangeBlockPeriod &gt;= addTokenTimeLockInBlocks._


_minimumWeightChangeBlockPeriod_ 强制执行逐步更新的开始和结束块之间的最短时间。 _addTokenTimeLockInBlocks_ 在添加令牌\(如果您具有 AddRemoveTokens 权限\)时使用，作为提交新令牌与应用\(即实际上将其添加到池\)之间的最短等待时间。还有一个附加约束：_minimumWeightChangeBlockPeriod &gt;= addTokenTimeLockInBlocks._

These parameters have default values, and can only be changed by overriding them in this version of createPool. The addTokenTimeLock defaults to 500 blocks \(~ 2 hours\), and the blockPeriod defaults to 90,000 \(~ 2 weeks\). If you want to use different minimum values, be sure to set them in createPool, since they are immutable thereafter!

这些参数具有默认值，并且只能通过在此版本的createPool中覆盖它们来进行更改。 addTokenTimeLock默认为500个块\(〜2小时\)，blockPeriod默认为90,000个\(〜2周\)。如果要使用其他最小值，请确保在createPool中设置它们，因为它们在以后是不可变的！

Also note that these only apply to gradual updates. The controller can call updateWeight directly at any time, as long as no gradual update is in progress.

另请注意，这些仅适用于逐步更新。控制器可以随时直接调用updateWeight，只要没有正在进行的逐步更新即可。

You can use [this simulator](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit#gid=1392289526) to explore how weight changes and updates work, estimate slippage and impermanent loss, etc. There is also a [demo video](https://vimeo.com/466075719) illustrating the simulator functionality.

您可以使用[此模拟器](https://docs.google.com/spreadsheets/d/1t6VsMJF8lh4xuH_rfPNdT5DM3nY4orF9KFOj2HdMmuY/edit#gid=1392289526)探索重量变化和更新的工作方式，估计打滑和意外损失等。此外，演示模拟器功能的[演示视频](https://vimeo.com/466075719)。