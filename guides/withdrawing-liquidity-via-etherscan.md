# 通过Etherscan提取流动性 - Withdrawing Liquidity via Etherscan

Our UI allows for any liquidity provider to withdraw liquidity by clicking on the button "Remove Liquidity" on the detailed pool view page. This article though focus on how to withdraw liquidity without our UI \(in case it is down or inaccessible\). To do that we'll be using Etherscan.io.

我们的用户界面允许任何流动性提供者通过单击详细池视图页面上的“删除流动性”按钮来撤回流动性。本文虽然重点讨论如何在没有用户界面的情况下撤回流动资金\(以防万一其关闭或无法访问\)。为此，我们将使用Etherscan.io。

If a pool is still private \(i.e. not finalized\), you can withdraw liquidity by simply unbinding each of its tokens:

如果某个池仍为私有\(即未完成\)，则可以通过简单地解除绑定每个令牌来撤回流动性：

![](https://lh5.googleusercontent.com/epkR6-l5a2awnjvRAjNtBkFqE2rjHe7gfZ5fo2BH0l1uj87IA3fN2n6mfhZEtIJ-VbrtHEhosjgo35k7sdEMhJhZSbyVz0AWeEoNCFn-YPG4fSRHtTznRaYSEGXQ_OM0KlsGOggo)

In case we’re dealing with a shared pool, things look a bit different. Once a pool is finalized, a BPT \(Balancer Pool Token\) associated with that pool is created with an initial supply of 100 units \(always with 18 decimals\). From that point on, ownership of the pool is tracked by BPT: holders are pro-rata owners of the pool’s liquidity.

如果我们要处理共享池，情况可能会有所不同。最终确定池后，将创建与该池关联的BPT \(Balancer池令牌\)，其初始供应量为100个单位\(始终为18个小数位\)。从那时起，BPT会跟踪池的所有权：持有者是池流动性的按比例所有者。

Just to illustrate, this is how a BPT distribution looks like for the more popular “75-MKR / 25-WETH” pool:

只是为了说明一下，这是更流行的“75-MKR / 25-WETH”池的BPT分布的样子：

[https://etherscan.io/token/tokenholderchart/0x987D7Cc04652710b74Fff380403f5c02f82e290a](https://etherscan.io/token/tokenholderchart/0x987D7Cc04652710b74Fff380403f5c02f82e290a)

![](https://lh5.googleusercontent.com/CwFnsvCn0E960qFS7qvBjXTSdVHgJyXoIW1skGhgvmelZrXJDTDCWVUh5GEY9AZlhd9r0vUy1_XGjb9vLp7ExH3xpVDkheUwT1yytFcyxJlUm_GBsj-q5L6s8XjCNSGKWanP5vuJ)

This means account 0x821a...46 owns about 25% of all the MKR and WETH held by the pool, and they may withdraw those tokens from the pool at any moment.

这意味着帐户 0x821a...46 拥有该池所持有的所有MKR和WETH的约25％，并且他们随时可能从池中提取这些令牌。

The simpler way to remove liquidity \(either fully or partially\) from a shared pool is of course to navigate to the pool’s page and click “Remove Liquidity”:

从共享池中删除\(全部或部分\)流动性的更简单方法当然是导航到池的页面，然后单击“删除流动性”：

[https://pools.balancer.exchange/\#/pool/0x987D7Cc04652710b74Fff380403f5c02f82e290a](https://pools.balancer.exchange/#/pool/0x987D7Cc04652710b74Fff380403f5c02f82e290a)

#### 奖励积分：通过Etherscan从共享池中消除流动性 - Bonus Points: Removing Liquidity from a Shared Pool via Etherscan

If you don’t want to depend on Balancer’s website being always operational; you may choose to run it locally \(it’s open-source\).

如果您不想依靠Balancer的网站始终运行；您可以选择在本地\(开源\)运行它。

But maybe you want to understand a bit better what happens under the hood during a withdrawal. 

但是，也许您想更好地了解提款期间背后发生的情况

So let’s use Etherscan to explore our [BTC++/DAI/USDC pool](https://etherscan.io/address/0x75286e183D923a5F52F52be205e358c5C9101b09#writeContract).

因此，让我们使用Etherscan探索下 [BTC++/DAI/USDC pool](https://etherscan.io/address/0x75286e183D923a5F52F52be205e358c5C9101b09#writeContract).


The function we’re looking for is called exitPool. The parameters expected are: 

我们正在寻找的功能称为exitPool。预期参数为：

1. the amount of BPT \(in base units, so 1 is actually 10^18\) you’re giving back to the pool in exchange for the underlying tokens; and
2. the minimum amount of each underlying token \(also in base units, separated by commas\) you expect to receive \(transaction fails if any minimum isn’t met\).

1. 您要返还给池以换取基础代币的BPT \(以基本单位为单位，所以1实际上是10 ^ 18 \)；
2. 您希望收到的每个基础令牌的最小数量\(也以基本单位，用逗号分隔\) \(如果未达到最小数量，则交易失败\)。


To withdraw all your liquidity in one transaction, simply give back to the pool your full BPT balance \(in our case it’s a bit over 1184.12 BPT\). Remember all BPTs have 18 decimals. To accept any amount of underlying tokens, just set all minimum amounts to zero. Since we have 3 underlying tokens, minAmountsOut has 3 zeros: "0,0,0".

要在一次交易中提取所有流动资金，只需将您的全部BPT余额退还给资金池\(在我们的示例中，BPT余额超过1184.12 BPT\)。请记住，所有BPT都有18位小数。要接受任何数量的基础令牌，只需将所有最小数量设置为零。由于我们有3个基础令牌，因此minAmountsOut具有3个零："0,0,0".

![](https://lh5.googleusercontent.com/T0sUcnFlu5-kXQmGbD2m8WAB5mgygcRblysrZVyGF1bTN4EYu9cW61GCSA0Sp2GTXC_gpThd1HejDGBMYar3flSdMcBYT8N55zNnqMOIwft32kn20aZykWD1zBW5JbqY3tKmhkO1)

And what is the point of getting to know the actual function being called and its parameters? 

知道实际调用的函数及其参数的目的是什么？

One use case that comes to mind is to keep handy \(e.g. saved on a mobile phone\) a pre-signed transaction withdrawing all your funds from a pool. This would be useful in an emergency: you could immediately broadcast your transaction, from any device \(that doesn’t hold the private key to that account\).

我想到的一个用例是方便\(例如保存在手机上\)预先签名的交易，从资金池中提取所有资金。这在紧急情况下很有用：您可以从任何设备\(不持有该帐户的私钥\)立即广播交易。

After clicking “Write” at the figure above, you can copy the DATA field shown in Metamask, and use it to craft an offline transaction using [MyEtherWallet](https://www.myetherwallet.com/interface/send-offline).

单击上图的“Write”后，您可以复制Metamask中显示的DATA字段，并使用它通过[MyEtherWallet](https://www.myetherwallet.com/interface/send-offline)进行脱机交易。
