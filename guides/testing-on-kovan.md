# Testing on Kovan

## 概述 - Overview

This guide will walk through the process of interacting with Balancer on Kovan directly with the smart contracts along with additional tooling such as the subgraph and SOR.

本指南将逐步介绍直接通过智能合约与Kovan上的Balancer进行交互的过程以及诸如subgraph和SOR之类的其他工具。

### 安装 - Setup

This guide will use `seth` - a tool built by Dapphub to interact directly with smart contracts. To install, run the below command. Note: the Dapp Tools suite installs Nix OS.

本指南将使用Dapphub构建的工具`seth`直接与智能合约进行交互。要安装，请运行以下命令。注意：Dapp Tools套件将安装Nix OS。

```text
curl https://dapp.tools/install | sh
```

If this doesn't work, you might need to install nix manually first:  
如果这不起作用，则可能需要先手动安装nix：

`curl -L https://nixos.org/nix/install | sh`

{% hint style="warning" %}
Nix is broken on MacOS Catalina. To fix, follow the steps at: [https://github.com/NixOS/nix/issues/2925\#issuecomment-539570232](https://github.com/NixOS/nix/issues/2925#issuecomment-539570232)
{% endhint %}

{% hint style="warning" %}
Nix在MacOS Catalina上损坏。要修复，请按照以下步骤操作: [https://github.com/NixOS/nix/issues/2925\#issuecomment-539570232](https://github.com/NixOS/nix/issues/2925#issuecomment-539570232)
{% endhint %}

Follow the remaining steps at: [https://github.com/dapphub/dapptools/tree/master/src/seth\#example-sethrc-file](https://github.com/dapphub/dapptools/tree/master/src/seth#example-sethrc-file) in order to select the `SETH_CHAIN` and address / key.

请按照以下其余步骤操作：[https://github.com/dapphub/dapptools/tree/master/src/seth\#example-sethrc-file](https://github.com/dapphub/dapptools/tree/master/src/seth#example-sethrc-file) 以选择`SETH_CHAIN`和地址/密钥。

Run the below lines in your terminal to setup environment variables that will be used later in the guide.

在终端中运行以下行以设置环境变量，这些变量将在本指南的后面部分中使用。

```text
export BFACTORY=0x8f7F78080219d4066A8036ccD30D588B416a40DB
export FAUCET=0xb48Cc42C45d262534e46d5965a9Ac496F1B7a830
export WETH=0xd0A1E359811322d97991E03f863a0C30C2cF029C
export DAI=0x1528F3FCc26d13F7079325Fb78D9442607781c8C
export MKR=0xef13C0c8abcaf5767160018d268f9697aE4f5375
export USDC=0x2F375e94FC336Cdec2Dc0cCB5277FE59CBf1cAe5
export REP=0x8c9e6c40d3402480ACE624730524fACC5482798c
export WBTC=0xe0C9275E44Ea80eF17579d33c55136b7DA269aEb
export BAT=0x1f1f156E0317167c11Aa412E3d1435ea29Dc3cCE
export SNX=0x86436BcE20258a6DcfE48C9512d4d49A30C4d8c4
export ANT=0x37f03a12241E9FD3658ad6777d289c3fb8512Bc9
export ZRX=0xccb0F4Cf5D3F97f4a55bb5f5cA321C3ED033f244
```

### 获取测试资金 - Acquire Test Funds

All pools use wrapped ETH. In order to get WETH, either use the faucet at [https://faucet.kovan.network](https://faucet.kovan.network/) and then `deposit()` into the WETH contract. Or the faucet below may have some available WETH.

所有池都使用包装的ETH。为了获得WETH，请使用位于[https://faucet.kovan.network](https://faucet.kovan.network/)的水龙头，然后将`deposit()`存入WETH合约中。或者下面的水龙头可能有一些可用的WETH。

For all other tokens, Balancer has deployed a set of test tokens and a faucet on Kovan. The guide below will use DAI & MKR, but feel free to use any of the available tokens. An user can call `drip` once per day per token.

对于所有其他令牌，Balancer在Kovan上部署了一组测试令牌和一个水龙头。以下指南将使用DAI和MKR，但可以随时使用任何可用的令牌。用户每天可以为每个令牌调用一次`drip`。

| Token | Address | Drip Amount |
| :--- | :--- | :--- |
| WETH | [0xd0a1e359811322d97991e03f863a0c30c2cf029c](https://kovan.etherscan.io/address/0xd0a1e359811322d97991e03f863a0c30c2cf029c) | 0.25 WETH |
| DAI | [0x1528F3FCc26d13F7079325Fb78D9442607781c8C](https://kovan.etherscan.io/address/0x1528F3FCc26d13F7079325Fb78D9442607781c8C) | 100 DAI |
| MKR | [0xef13C0c8abcaf5767160018d268f9697aE4f5375](https://kovan.etherscan.io/address/0xef13C0c8abcaf5767160018d268f9697aE4f5375) | 0.5 MKR |
| USDC | [0x2F375e94FC336Cdec2Dc0cCB5277FE59CBf1cAe5](https://kovan.etherscan.io/address/0x2F375e94FC336Cdec2Dc0cCB5277FE59CBf1cAe5) | 100 USDC |
| REP | [0x8c9e6c40d3402480ACE624730524fACC5482798c](https://kovan.etherscan.io/address/0x8c9e6c40d3402480ACE624730524fACC5482798c) | 10 REP |
| WBTC | [0xe0C9275E44Ea80eF17579d33c55136b7DA269aEb](https://kovan.etherscan.io/address/0xe0C9275E44Ea80eF17579d33c55136b7DA269aEb) | 0.02 WBTC |
| BAT | [0x1f1f156E0317167c11Aa412E3d1435ea29Dc3cCE](https://kovan.etherscan.io/address/0x1f1f156E0317167c11Aa412E3d1435ea29Dc3cCE) | 500 BAT |
| SNX | [0x86436BcE20258a6DcfE48C9512d4d49A30C4d8c4](https://kovan.etherscan.io/address/0x86436BcE20258a6DcfE48C9512d4d49A30C4d8c4) | 85 SNX |
| ANT | [0x37f03a12241E9FD3658ad6777d289c3fb8512Bc9](https://kovan.etherscan.io/address/0x37f03a12241E9FD3658ad6777d289c3fb8512Bc9) | 200 ANT |
| ZRX | [0xccb0F4Cf5D3F97f4a55bb5f5cA321C3ED033f244](https://kovan.etherscan.io/address/0xccb0F4Cf5D3F97f4a55bb5f5cA321C3ED033f244) | 400 ZRX |

Call `drip` on the faucet specifying a token address:

在水龙头上调用`drip`以指定令牌地址：

```text
seth send $FAUCET "drip(address)" $DAI
```

Confirm that you received test tokens:

确认您已收到测试令牌:

```text
seth --from-wei $(seth --to-dec $(seth call $DAI "balanceOf(address)" $ETH_FROM))
```

Repeat the above steps for any additional tokens you would like to interact with on the Balancer test deployment.

对要在Balancer测试部署上与之交互的任何其他令牌重复上述步骤。

### 池创建 - Pool Creation

Next we're going to create a new pool. This guide will create a 50% WETH, 25% DAI, 25% MKR pool. Feel free to use any of the tokens

接下来，我们将创建一个新池。本指南将创建50％WETH，25％DAI，25％MKR池。随意使用任何代币

{% hint style="warning" %}
Some tokens do not use the standard 18 decimals. Be aware of balance calculations and weights when using tokens with different decimals.

一些令牌不使用标准的18位小数。使用带有不同小数位的令牌时，请注意余额计算和权重。

USDC - 6  
WBTC - 8  
Any Compound token - 8
{% endhint %}

```text
seth send --gas 5000000 $BFACTORY "newBPool()"
```

Make note of the created contract address and set the below `BPool` variable to the address returned. This can be found on etherscan by looking at the internal txs of the tx hash.

记下创建的合同地址，并将下面的 `BPool` 变量设置为返回的地址。通过查看tx哈希的内部tx，可以在etherscan上找到它。

```text
export BPOOL=0x... (address returned from previous step)
```

```text
BPOOL=0x... (address returned from previous step)
```

Token approvals are needed for any tokens you plan on adding to the pool. Approvals per pool is only required when interacting with the contracts directly. The frontend interfaces will all use a proxy so there will only be 1 time approvals to interact with all pools in the Balancer ecosystem.

您计划添加到池中的任何令牌都需要令牌批准。仅在与合约直接交互时才需要每个池的批准。前端接口将全部使用代理，因此只有批准1次才能与Balancer生态系统中的所有池进行交互。

```text
amount=$(seth --to-uint256 $(seth --to-wei 1000000 ether))
seth send $WETH "approve(address,uint256)" $BPOOL $amount
seth send $DAI "approve(address,uint256)" $BPOOL $amount
seth send $MKR "approve(address,uint256)" $BPOOL $amount
```

After all approvals are confirmed, tokens are bound to a specific pool by calling \`\`bind\(address token, uint balance, uint denorm\)\`\`\`

确认所有批准后，通过调用\(address token, uint balance, uint denorm\)\`\`\`将令牌绑定到特定池


`denorm` is the denormalized weight for the token being added. Weights are stored in this fashion to prevent unnecessary gas costs of recalculating weights every time a new token is added. In our example, since we want 50% WETH, 25% DAI, and 25% MKR we can use the following denormalized weights: 10, 5, 5. \(Note: the denorm weights are scaled to Wei; i.e., "1" = 10^18, to allow for non-integer weights.\)

`denorm`是要添加的令牌的非规范化权重。权重以这种方式存储，以防止每次添加新令牌时重新计算权重的不必要的气体成本。在我们的示例中，由于我们需要50％的WETH，25％的DAI和25％的MKR，因此我们可以使用以下非规范化的权重：10、5、5。\(注意：分母权重被缩放为Wei；即“1” = 10 ^ 18，以允许使用非整数权重。\)


Valid denorm values range from 1 to 50 - and the total sum must be &lt;= 50. In practice, to ensure the math always works, best practice is to use a smaller range, such as 2 - 40.

有效分母值的范围是1到50-总和必须<=50。实际上，为了确保数学始终有效，最佳实践是使用较小的范围，例如2-40。

```text
# Bind 1 WETH with a denormalized weight of 10
amount=$(seth --to-uint256 $(seth --to-wei 1 ether))
weight=$(seth --to-uint256 $(seth --to-wei 10 ether))
seth send $BPOOL "bind(address, uint256, uint256)" $WETH $amount $weight
```

Since we also want to bind DAI and MKR, and we know the desired weights, we have to calculate a balance for each token such that the internal token prices calculated by the protocol - based only on the weights and balances - match the external market prices of the tokens. \(Otherwise, there would be immediate, unintended arbitrage opportunities as soon as you created the pool.\) This example will assume the following asset prices: ETH - $200, MKR - $400, DAI - $1.

由于我们还希望绑定DAI和MKR，并且知道所需的权重，因此我们必须为每个令牌计算余额，以使协议计算的内部令牌价格（仅基于权重和余额）与外部市场价格匹配令牌。\(否则，一旦创建池，就会有立即的，意外的套利机会。\)此示例将假定以下资产价格：ETH-$ 200，MKR-$ 400，DAI-$ 1。

Since we bound 1 WETH at a price of $200 and we want that to be 50% of the pool, we want to bind $100 of both DAI and MKR.

由于我们以200美元的价格绑定1个WETH，并且希望将其作为池的50％，因此我们希望绑定100美元的DAI和MKR。

```text
# Bind 100 DAI with a denormalized weight of 5
amount=$(seth --to-uint256 $(seth --to-wei 100 ether))
weight=$(seth --to-uint256 $(seth --to-wei 5 ether))
seth send $BPOOL "bind(address, uint256, uint256)" $DAI $amount $weight

# Bind 0.25 MKR with a denormalized weight of 5
amount=$(seth --to-uint256 $(seth --to-wei 0.25 ether))
weight=$(seth --to-uint256 $(seth --to-wei 5 ether))
seth send $BPOOL "bind(address, uint256, uint256)" $MKR $amount $weight
```

Let's confirm that all the tokens were added by using some view functions. This will get a token's normalized weight, or percentage of the pool, to make sure our original math was correct.

让我们确认使用某些视图函数添加了所有标记。这将获得令牌的归一化权重或池的百分比，以确保我们的原始数学是正确的。

```text
seth call $BPOOL "getNumTokens()"

seth --from-wei $(seth --to-dec $(seth call $BPOOL "getNormalizedWeight(address)" $WETH))

seth --from-wei $(seth --to-dec $(seth call $BPOOL "getNormalizedWeight(address)" $DAI))

seth --from-wei $(seth --to-dec $(seth call $BPOOL "getNormalizedWeight(address)" $MKR))
```

Now that we're finished adding tokens, we need to set a swap fee for our pool. Otherwise we wouldn't earn anything on our assets and be more exposed to impermanent loss. In this example, we will set a swap fee of 0.3%:

现在我们已经完成了添加令牌，我们需要为我们的池设置掉期费。否则，我们将不会在资产上赚到任何钱，更容易遭受无常的损失。在此示例中，我们将交换费设置为0.3％：

```text
fee=$(seth --to-uint256 $(seth --to-wei 0.003 ether))
seth send $BPOOL "setSwapFee(uint256)" $fee
```

We have a valid pool with 3 bound tokens and a swap fee of 0.3%. At this point, a decision has to be made whether to `finalize` the pool. A pool can either be private or shared. A private pool allows the owner to continually adjust tokens, balances, weights, and fees. But prevents anyone else from adding or removing liquidity to that pool - it wouldn't be fair if the owner changed weights if other users have contributed liquidity! A shared pool is created when the `finalize` function is called and is a one-way transition. This locks all of the tokens, balances, weights, and opens the ability for outside users to add and remove liquidity.

我们有一个带有3个绑定令牌的有效池，交换费为0.3％。在这一点上，必须决定是否`finalize`游泳池。池可以是私有的也可以是共享的。专用池允许所有者连续调整令牌，余额，权重和费用。但是，要防止其他人向该池中添加或删除流动性-如果其他用户贡献了流动性，所有者改变权重，那将是不公平的！共享池是在调用`finalize`函数时创建的，并且是单向转换。这将锁定所有代币，余额，权重，并为外部用户打开添加和删除流动性的能力。

A private pool allows the owner to continually adjust tokens, balances, weights, and fees - but prevents anyone else from adding or removing liquidity to that pool. After all, it wouldn't be fair if the owner changed weights after other users contributed liquidity! A shared pool is created when the `finalize` function is called - and this is a one-way transition. This locks all of the tokens, balances, weights, and opens the pool to outside users to add and remove liquidity.

私有池允许所有者不断调整代币，余额，权重和费用-但阻止其他任何人添加或删除该池的流动性。毕竟，如果所有者在其他用户提供流动性后改变权重，那将是不公平的！调用`finalize`函数时会创建一个共享池-这是单向转换。这将锁定所有代币，余额，权重，并向外部用户开放该池以添加和删除流动性。

For our example, we want other users to be able to add liquidity, so let's finalize it. 100 Balancer Pool Tokens will be minted as part of the pool finalization regardless of the bound token balances. Balancer Pool Tokens, or BPTs, represent proportional ownership of a pools liquidity. Any future joins or exits are calculated based on the relative liquidity being added.

对于我们的示例，我们希望其他用户能够增加流动性，因此让我们最终确定它。无论绑定的令牌余额如何，都会在池最终确定过程中铸造100个Balancer Pool令牌。Balancer池代币或BPT代表池流动性的比例所有权。任何将来的联接或退出都将基于所添加的相对流动性进行计算。

```text
seth send $BPOOL "finalize()"
```

All done, now anyone can add liquidity and swap against the assets in the pool!

全部完成，现在任何人都可以增加流动性并与资产池中的资产互换！

Let's confirm that we received BPTs by calling `balanceOf` directly on the pool address \(since the pools themselves are ERC20 tokens\)

让我们确认是否通过直接在池地址\上调用`balanceOf`来接收BPT\(因为池本身是ERC20令牌\)

```text
seth --from-wei $(seth --to-dec $(seth call $BPOOL "balanceOf(address)" $ETH_FROM))
```

