# 流动性引导例子 - Liquidity Bootstrapping Example

Let's walk through a complete example, using the [Liquidity Bootstrapping](https://balancer.finance/2020/03/04/building-liquidity-into-token-distribution/) use case.

让我们使用[Liquidity Bootstrapping](https://balancer.finance/2020/03/04/building-liquidity-into-token-distribution/)用例来演示一个完整的示例。

First, we give the token a symbol and name, set the basic pool parameters, and determine the permissions. All we really need to be able to do is change the weights, so we can set all the other permissions false.

首先，我们给令牌一个符号和名称，设置基本的池参数，并确定权限。我们真正需要做的就是更改权重，因此我们可以将所有其他权限设置为false。

As noted earlier, setting the permissions as strict as possible minimizes the trust investors need to place in the pool creator. Liquidity providers for this pool can rest assured that the fee can never be changed, no tokens can be added or removed, and they cannot be prevented from adding liquidity \(e.g., by being removed from the whitelist, or having the cap lowered\).

如前所述，将权限设置得尽可能严格，可以最大程度地减少投资者对池创建者的信任。此池的流动性提供者可以放心，费用永远不会改变，不能添加或删除代币，也不能阻止它们增加流动性\(例如，从白名单中删除或降低上限\) 。


```text
// XYZ and DAI are addresses
// XYZ is the "project token" we're launching

const poolParams = {
    tokenSymbol: 'LBT',
    tokenName: 'Liquidity Bootstrapping Token',
    tokens: [XYZ, DAI],
    startBalances: [toWei('4000'), toWei('1000')],
    startWeights: [toWei('32'), toWei('8')],
    swapFee: toWei('0.005'), // 0.5%
}

const permissions = {
    canPauseSwapping: false,
    canChangeSwapFee: false,
    canChangeWeights: true,
    canAddRemoveTokens: false,
    canWhitelistLPs: false,
    canChangeCap: false,
    canRemoveAllTokens: false,
};
```

Next, we use these structs to deploy a new Configurable Rights Pool \(and the underlying Core Pool\).

接下来，我们使用这些结构来部署新的可配置权限池\(以及基础Core Pool\)。

```text
// If deploying locally; otherwise use the published addresses
// This is the factory for the underlying Core BPool
bFactory = await BFactory.deployed();

// This is the Smart Pool factory
crpFactory = await CRPFactory.deployed();

// Static call to get the return value
const crpContract = await crpFactory.newCrp.call(
    bFactory.address,
    poolParams,
    permissions,
);

// Transaction that actually deploys the CRP
await crpFactory.newCrp(
    bFactory.address,
    poolParams,
    permissions,
);

// Wait for it to get mined
const crp = await ConfigurableRightsPool.at(crpContract);

// Creating the pool transfers collateral tokens
// Must allow the contract to spend them
await dai.approve(crp.address, MAX);
await xyz.approve(crp.address, MAX);

// Create the underlying pool
// Mint 1,000 LBT pool tokens; pull collateral into BPool
// Override with fast block wait times for testing purposes
//   (Defaults are 2 hours min delay / 2 weeks min duration)
await crp.createPool(toWei('1000'), 10, 10);
```

At this point we have an initialized pool. The admin account has 1,000 LPTs, and the underlying BPool is holding the tokens. The pool is enabled for public trading and adding liquidity.

至此，我们有了一个初始化池。管理员帐户具有1,000个LPT，底层BPool持有令牌。该池可用于公开交易和增加流动性。

To facilitate the token launch - with low slippage, low initial capital, and stable prices over time, per the paper referenced above - we want to gradually "flip" the weights over time. We start with the project token at a high weight \(32/\(32+8\), or 80%, and collateral DAI at 20%. At the end of the launch, we want XYZ at 20%, and DAI at 80%. We accomplish this by calling updateWeightsGradually; we're allowed to do this because the canChangeWeights permission was set to true.

根据上面引用的论文，为了促进令牌的发行-具有较低的延误，较低的初始资金和随时间推移的稳定价格，我们希望随着时间的推移逐渐“翻转”权重。我们以高权重\(32/\(32+8\)或80％的项目令牌开始，并以20％的抵押DAI开始。在发布结束时，我们希望XYZ为20％，DAI为80％。我们通过调用updateWeightsGradually来完成此操作；因为canChangeWeights权限设置为true，所以我们可以这样做。

```
// Start changing the weights in 100 blocks
const block = await web3.eth.getBlock('latest');
const startBlock = block.number + 100;
const blockRange = 10000;

// "Flip" the weights linearly, over 10,000 blocks
const endBlock = startBlock + blockRange;

// Set the endWeights to the reverse of the startWeights
const endWeights = [toWei('8'), toWei('32')];

// Kick off the weight curve
await crp.updateWeightsGradually(endWeights, startBlock, endBlock);
```

Of course, smart contracts can't change state by themselves. What _updateWeightsGradually_ actually does is put the contract into a state where it will respond to the _pokeWeights_ call by setting all the weights according to the point on the "weight curve" corresponding to the current block. \(It will revert before the start block, and set the weights to their final values if called after the end block.\)

当然，智能合约不能自行改变状态。 _updateWeightsGradually_实际执行的操作是使合同进入一种状态，通过根据与当前块相对应的“权重曲线”上的点设置所有权重，合同将响应_pokeWeights_调用。\(它将在开始块之前还原，如果在结束块之后调用，则将权重设置为其最终值。)

```text
// Sample code to print the current weights,
// Then call pokeWeights to update them

for (i = 0; i < blockRange; i++) {
    weightXYZ = await controller.getDenormalizedWeight(XYZ);
    weightDAI = await controller.getDenormalizedWeight(DAI);
    block = await web3.eth.getBlock("latest");
    console.log('Block: ' + block.number + '. Weights -> XYZ: ' +
        (fromWei(weightXYZ)*2.5).toFixed(4) + '%\tDAI: ' +
        (fromWei(weightDAI)*2.5).toFixed(4) + '%');

    // Actually cause the weights to change
    await crp.pokeWeights();
}
```

There are many subtleties; for instance, you could implement a non-linear bootstrapping weight curve by calculating the weights off-chain and setting them directly. For a comprehensive set of tests that demonstrate all features of the Configurable Rights Pool, see our [GitHub](https://github.com/balancer-labs/configurable-rights-pool).

有很多微妙之处；例如，您可以通过计算链下权重并直接设置权重来实现非线性自举权重曲线。有关演示可配置权限池所有功能的全面测试，请参见我们的[GitHub](https://github.com/balancer-labs/configurable-rights-pool)。