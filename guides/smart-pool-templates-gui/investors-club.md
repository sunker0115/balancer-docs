# 投资俱乐部 - Investors' Club

With Smart Pools, it is possible to restrict liquidity providers to a defined group. For instance, accredited investors, or those who have complied with a KYC process \(e.g., for STOs\). However, the privileged addresses could be determined by anything - holders of certain tokens, those who participated in community building or voting, winners of a "lottery" for investment slots, etc.

使用智能池，可以将流动性提供者限制为已定义的组。例如，经过认证的投资者，或遵守KYC流程\(例如，对于STO \)的投资者。但是，特权地址可以由任何东西决定-某些令牌的持有者，参与社区建设或投票的人员，投资老虎机“彩票”的获胜者等。

Legend**: required;** ~~not required;~~ _optional_

**权限配置 - Rights configuration:**

* _canPauseSwapping_
* ~~canChangeSwapFee~~
* ~~canChangeWeights~~
* ~~canAddRemoveTokens~~
* **canWhitelistLPs**
* _canChangeCap_

The mechanism for this is the LP whitelist, which has to be enabled. The controller can add or remove addresses from this list, and only those addresses are permitted to join pools. \(NB: the pool controller cannot prevent swaps - other than by using the pauseSwapping right, which halts it altogether for everyone. Also, the pool tokens are ERC-20 conforming, so do not prevent transfers. It's the LP's responsibility to keep track of their tokens, and not transfer them to unauthorized parties.\)

LP白名单的机制是必须启用的。控制器可以在此列表中添加或删除地址，并且仅允许这些地址加入池。 \(注意：池控制器不能阻止交换-除了使用pauseSwapping权限之外，它对所有人都完全停止。而且，池令牌符合ERC-20，因此请勿阻止转移。LP负责跟踪的令牌，请勿将其转让给未经授权的各方。\)

Depending on the use case, the pool operator might need to pause swapping \(e.g., during a signup or redemption period\). If it is an STO, there might also be hard- or soft caps on the value of the pool. If this is the case, the cap right would also need to be enabled. See [Experimental](experimental.md).

根据使用情况，池操作员可能需要暂停交换\(例如，在注册或兑换期间\)。如果它是一个STO，则池的价值上可能也有硬上限或软上限。在这种情况下，还需要启用上限权利。请参阅[实验](experimental.md)。