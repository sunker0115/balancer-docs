# 交易和奖励列表 - Exchange and Reward Listing

Token listings are managed in [this repository](https://github.com/balancer-labs/assets), with the following categories:

令牌列表在[此存储库](https://github.com/balancer-labs/assets)中进行管理，具有以下类别：

* `eligible.json`: assets eligible for BAL mining as per weekly proposals - 每周提案中符合BAL开采条件的资产
* `listed.json`: assets listed on balancer.exchange - 在balancer.exchange上列出的资产
* `ui-not-eligible.json`: assets vetted by community members - 社区成员审查的资产
* `untrusted.json`: assets that are incompatible with Balancer - 与Balancer不兼容的资产

There are two kinds of token "listings" on Balancer. The first is listing on the [Exchange](https://balancer.exchange/#/swap). Listed tokens appear on the main page as swapping options. It is possible to access unlisted pairs through a "deep link" with their addresses \(balancer.exchange/\#/swap/&lt;address1&gt;/&lt;address2&gt;\), but it display unlisted tokens as addresses, and with a warning.

Balancer上有两种令牌“清单”。第一个是在[Exchange](https://balancer.exchange/#/swap)上列出。列出的令牌作为交换选项出现在主页上。可以通过地址为\(balancer.exchange/\#/swap/&lt;address1&gt;/&lt;address2&gt;\)的“深层链接”访问未列出的对，但它会将未列出的令牌显示为地址，并带有一个警告。

There is no formal process for listing tokens on the Balancer exchange UI. That is up to the team's discretion and relies on internal factors around trading volume, usage, and legitimacy. \(As noted above, it's always possible to trade tokens via contract address.\)

在Balancer交易UI上没有正式的列出令牌的过程。这取决于团队的判断力，并取决于围绕交易量，使用情况和合法性的内部因素。\(如上所述，始终可以通过合同地址进行令牌交易。\)

The second kind of listing is eligibility for BAL rewards. New tokens are listed \(and occasionally removed\) through a weekly governance process There is a streamlined process for simply listing a new token - tokens that meet a set of technical criteria below can be approved without requiring community vote. Only increasing the cap, introducing "controversial" or non-conforming tokens, or adjusting the reward process or its parameters requires a vote.

第二种是获得BAL奖励的资格。通过每周的治理过程列出\(有时被删除\)新令牌。有一个简化的过程可以简单地列出新令牌-满足以下一组技术标准的令牌无需社区投票即可获得批准。仅增加上限，引入“有争议的”或不一致的令牌，或调整奖励过程或其参数需要投票。

To request approving your token for BAL rewards, just post to \#token-requests on the [Discord](https://discord.gg/ARJWaeF) channel. Each weekly reward period begins at 00:00 UTC on Monday; any requests approved after then will take effect the following week.

要请求批准您的BAL奖励令牌，只需在[Discord] (https://discord.gg/ARJWaeF)频道上发布到 \#token-requests即可。每个每周奖励期从星期一的00:00 UTC开始；此后批准的所有请求将在下周生效。

The listing criteria below are taken from this [accepted proposal](https://forum.balancer.finance/t/proposal-to-update-the-whitelist-process/217/4). \(We will endeavor to keep this up-to-date, but see the [Balancer Forum](https://forum.balancer.finance/) for the very latest.\)

以下列出的标准选自此[接受的提案](https://forum.balancer.finance/t/proposal-to-update-the-whitelist-process/217/4)。\(我们将努力保持最新状态，但请参阅[Balancer论坛](https://forum.balancer.finance/)以获取最新信息。\)

1. The token’s smart contract must be verified on [Etherscan](https://etherscan.io/). Neglecting this small degree of transparency adds unnecessary friction to the process of vetting for the remaining criteria. - 令牌的智能合约必须在[Etherscan](https://etherscan.io/)上进行验证。忽略这种小的透明度会增加审核剩余标准的不必要的摩擦。
2. The token must conform to the ERC-20 interface described in [EIP-20](https://eips.ethereum.org/EIPS/eip-20). Namely, the functions `transfer()`, `transferFrom()`, and `approve()` must return booleans. - 令牌必须符合[EIP-20](https://eips.ethereum.org/EIPS/eip-20)中所述的ERC-20接口。即，函数`transfer()`, `transferFrom()`, `approve()`必须返回布尔值。
3. The token’s `transfer()` and `transferFrom()` implementations must exhibit the expected behavior - namely, transferring N tokens from one address to another. Certain divergences from this behavior, such as transfer fees, can cause issues with Balancer pools, and these tokens will be rejected. - 令牌的`transfer()` 和 `transferFrom()`实现必须表现出预期的行为-即将N个令牌从一个地址转移到另一个地址。与该行为的某些差异（例如转移费用）可能会导致Balancer池出现问题，并且这些令牌将被拒绝。
4. The token’s `approve()` implementation must exhibit the expected behavior - namely, it must allow for infinite approval, or the token cannot be sold using the Balancer exchange proxy. - 令牌的`approve()`实现必须表现出预期的行为-即它必须允许无限的批准，否则就不能使用Balancer交换代理出售令牌。
5. The token must not be vulnerable to the so-called "`gulp()` attack," which is exposed when a pool’s token balance changes unbeknownst to the pool. For now, known cases include tokens that charge a transfer fee \(as described in \#3\) and tokens that periodically rebase. A rebasing token utilizing an atomic rebase+gulp should be safe from such attacks, but none has yet been discovered. **\(Added note:** Ampleforth released a pool with this behavior, though it was implemented through a Balancer Pool subclass, not the token contract.\) - 令牌不得易受所谓的“`gulp()`攻击”，当池的令牌余额发生未知变化时，该攻击就会暴露出来。目前，已知的情况包括收取转让费\（如\#3\中所述）的令牌和定期重新设置基准的令牌。利用原子rebase + gulp的重定基础令牌应该可以抵御此类攻击，但尚未被发现。 **\(添加的注释：** Ampleforth发布了具有此行为的池，尽管它是通过Balancer Pool子类而不是令牌合约实现的。\)
6. The token must not possess any mechanisms which, when combined with a Balancer pool, result in material losses to the principal value of the pooled token. This criterion covers all value-loss edge cases which may be difficult to anticipate but can still preclude a token’s whitelisting. Examples will be added as they are discovered; the only currently known example is [VBZRX](https://etherscan.io/address/0xB72B31907C1C95F3650b64b2469e08EdACeE5e8F), whose `claim()` mechanism entitles token holders to receive BZRX airdrops on each token transfer. In isolation, this mechanism is perfectly safe, but if a token is airdropped to a Balancer pool whose asset list does not contain said token, then the airdropped tokens are forever locked in the pool contract and cannot be recovered by anyone \(including Balancer Labs\). - 令牌不得拥有任何机制，当与平衡器池组合使用时，会导致池化令牌的本金遭受重大损失。该标准涵盖了所有可能会难以预料但仍可以防止令牌白名单的价值损失边缘情况。发现的示例将被添加；当前唯一已知的示例是[VBZRX](https://etherscan.io/address/0xB72B31907C1C95F3650b64b2469e08EdACeE5e8F)，其`claim()`机制使令牌持有者有权在每次令牌传输时接收BZRX空投。孤立地，此机制是绝对安全的，但是如果将令牌空投到资产列表中不包含该令牌的Balancer池中，则将空投的令牌永久锁定在池合同中，并且任何人都无法将其收回\(包括Balancer Labs\)
7. The token must have a price feed accessible via CoinGecko’s API, e.g. [this example link](https://api.coingecko.com/api/v3/simple/token_price/ethereum?contract_addresses=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&vs_currencies=usd) for WETH. This is instrumental to calculating a pool’s eligibility for BAL rewards. If a token meets all of the remaining criteria but not this criterion, it can be added to the UI whitelist and reconsidered for the mining whitelist once the price feed becomes available.  - 令牌必须具有可通过CoinGecko的API访问的价格供稿，例如[此示例链接](https://api.coingecko.com/api/v3/simple/token_price/ethereum?contract_addresses=0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2&vs_currencies=usd)。这有助于计算资产池中获得BAL奖励的资格。如果令牌满足所有其余标准但不满足此标准，则可以将其添加到UI白名单中，并在价格供稿可用时重新考虑用于采矿白名单。

