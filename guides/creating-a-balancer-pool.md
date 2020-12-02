# 创建一个共享的Balancer池 - Creating a Shared Balancer Pool

## 设置代理 - Setting up a proxy

All the interactions to add liquidity or to create Balancer pools on our UIs happen through a proxy. This way our UI can simplify the UX by avoiding token approvals on every new pool the user interacts with.

You'll be asked to setup a proxy when your address is interacting with our UI for the first time. For example, when you click on "Create Pool" on our [pool management interface](https://pools.balancer.exchange/#/):

![](../.gitbook/assets/image%20%283%29.png)

After clicking on "Create Pool," you'll see the setup button:

![](../.gitbook/assets/image%20%284%29.png)

The proxy setup waits for 10 confirmations to be extra safe.

An interesting observation: we use the same DSProxy smart contracts as MakerDAO, so if you have an MCD vault already you won't need to create another proxy!

所有增加交互性或在我们的UI上创建Balancer池的交互都是通过代理进行的。这样，我们的UI可以避免用户与之交互的每个新池上的令牌批准，从而简化UX。

当您的地址首次与我们的用户界面进行交互时，系统会要求您设置代理。例如，当您在我们的[池管理界面](https://pools.balancer.exchange/#/)上单击“创建池”时：

![](../.gitbook/assets/image%20%283%29.png)

点击“创建池”后，您会看到设置按钮:

![](../.gitbook/assets/image%20%284%29.png)

代理设置会等待10个确认，以确保更加安全。

一个有趣的发现：我们使用与MakerDAO相同的DSProxy智能合约，因此，如果您已经拥有MCD保管库，则无需创建另一个代理！

### 共享池创建 - Shared Pool Creation

At the moment you can only create **shared pools** on our UI. There will be soon the option to create and manage private pools. As a reminder:

* **shared pools** are open to anyone to join by adding liquidity and getting BPTs \(Balancer Pool Tokens\) in return, but all the pool parameters are immutable
* **private pools** only allow the owner to add liquidity to the pool, but all its parameters are flexible. So the owner of the private pool can change the swap fees, pause trades, add/remove tokens, change token weights, etc.

A Balancer pool allows up to 8 tokens and the weights have to be between 2% and 98%. The swap fee can be between 0.0001% and 10%.

![](../.gitbook/assets/image.png)

If the token you want to add is not listed on the token picker panel, you can add any custom token by pasting its address in the search field.

![](../.gitbook/assets/image%20%282%29.png)

**IMPORTANT**: make sure that the custom token you are adding complies with the ERC20 standard. For example it has to allow 0 value transfers and the transfer function must return a boolean. You can check if the token you are adding is on any of these two lists that gather many tokens that are not ERC20-compliant:

{% embed url="https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca" caption="" %}

{% embed url="https://github.com/sec-bit/awesome-buggy-erc20-tokens" caption="" %}

These lists though are **NOT** exhaustive, so make sure you check your token is compatible before creating a pool with it to avoid losing your tokens forever.

目前，您只能在我们的用户界面上创建 **共享池**。很快将有创建和管理专用池的选项。提醒一句：

* **共享池**向所有人开放，方法是增加流动性并获得BPT \(Balancer Pool Tokens \)作为回报，但是所有池参数都是不可变的
* **私有池**仅允许所有者向池中添加流动性，但是其所有参数都是灵活的。因此，私有池的所有者可以更改掉期费，暂停交易，添加/删除令牌，更改令牌权重等。

一个Balancer池最多允许8个令牌，权重必须在2％到98％之间。掉期费可以在0.0001％到10％之间。

![](../.gitbook/assets/image.png)

如果要添加的令牌未在令牌选择器面板上列出，则可以通过将其地址粘贴到搜索字段中来添加任何自定义令牌。

![](../.gitbook/assets/image%20%282%29.png)

**重要**：请确保您要添加的自定义令牌符合ERC20标准。例如，它必须允许0值传输，并且传输函数必须返回布尔值。您可以检查要添加的令牌是否在以下两个列表中的任何一个上，这些列表收集了许多不符合ERC20的令牌：

{% embed url="https://medium.com/coinmonks/missing-return-value-bug-at-least-130-tokens-affected-d67bf08521ca" caption="" %}

{% embed url="https://github.com/sec-bit/awesome-buggy-erc20-tokens" caption="" %}

这些列表虽然**不是**全部，但因此请确保在创建令牌池之前检查令牌是否兼容，以避免永久丢失令牌。