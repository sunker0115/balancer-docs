#  实验性  - Experimental

If you've been in the crypto space since 2017, you'll remember well the ICO hype. People were throwing money at every new token with a website. \(If you're just a bit older, you'll remember the .com boom of '99 as well - when it was every company with a website.\)

如果您自2017年以来一直在加密领域，那么您会记得ICO的炒作。人们通过网站向每个新令牌投入资金。 \(如果您年龄稍大一点，您还会想起'99的.com繁荣时期-当时每个公司都有网站。\)

![.COM -&amp;gt; ICO -&amp;gt; DeFi](../../.gitbook/assets/takemoney.jpeg)

We are now seeing the same phenomenon in DeFi. Cross out "token," insert "protocol." A high school student at a hackathon can deploy a contract written over a sleepless weekend - and by Monday there's $7 million dollars locked in it!

现在，我们在DeFi中看到了相同的现象。划掉“令牌”，插入“协议”。黑客马拉松的一名高中学生可以部署在一个不眠的周末写的合同-到星期一，其中已经锁定了700万美元！

This is dangerous, to say the least. Even worse, with everything public and open source, even if you don't intend to release something - [literally anyone](https://cointelegraph.com/news/anonymous-developer-deploys-curve-contracts-forcing-early-launch) can copy and deploy it for you.

至少可以说这很危险。更糟糕的是，即使您不打算发布任何东西，在公开和开源的环境中-[字面意义上的所有人](https://cointelegraph.com/news/anonymous-developer-deploys-curve-contracts-force-early-发布)可以为您复制并部署它。

Capped pools are a way to - if not completely eliminate the danger - at least limit the potential damage. Furthermore, it is flexible enough to allow the pool controller to halt and resume the influx of new capital at any time, for any reason.

封闭式水池是一种方法-如果不能完全消除危险-至少可以限制潜在的损害。此外，它具有足够的灵活性，允许池控制器因任何原因随时停止并恢复新资金的流入。

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* _canPauseSwapping_
* _canChangeSwapFee_
* ~~canChangeWeights~~
* ~~canAddRemoveTokens~~
* ~~canWhitelistLPs~~
* **canChangeCap**

Change cap is required, of course. You might also want to be able to control \(or at least influence\) trading through pausing or setting fees, but that is optional.

当然需要更改限额。您可能还希望能够通过暂停或设置费用来控制\( 至少影响\) 交易，但这是可选的。

The "cap" refers to the total supply of pool tokens. Recall that pool tokens are themselves ERC-20s, so this corresponds to the `totalSupply()`.

“上限”是指池令牌的总供应量。回想一下，池令牌本身就是ERC-20，因此它对应于`totalSupply()`.

The cap is always there, but if the right is not enabled, it is always set to `MAX` \(i.e., unlimited\). If the right is enabled, the cap is set to the initial supply on `createPool()`. Immediately after creating the pool, no one can add liquidity \(including the controller\).

上限始终存在，但是如果未启用此权限，则始终将其设置为“ MAX” \(即无限制\)。如果启用了该权限，则将上限设置为`createPool()`上的初始供应量。创建池后，没有人可以添加流动性\(包括控制人\)。

To allow others to join the pool, the controller can set the cap higher - for instance, to a supply corresponding to a given total underlying asset value. At any point, they can raise or lower the cap - including to 0 \(to "freeze" all new investment\), or unlimited \(if confidence is high and it's party time\).

为了允许其他人加入池中，控制器可以将上限设置得更高-例如，设置为与给定的基础资产总值相对应的供应。他们可以随时提高或降低上限-包括提高到0 \(“冻结”所有新投资\)，或无限提高\(如果置信度很高并且是聚会时间\)。



