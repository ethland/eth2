# Vitalik: Proof of Stake设计哲学

前言：以太坊2.0将逐渐从Proof of Work过渡到Proof of Stake共识机制 \(Casper PoS\)，Vitalik Buterin在2016年就撰文阐述了其背后的价值权衡和设计哲学。时隔多年，随着社区对加密经济学的不断探索，又启发了我们怎样的新思考？

像以太坊（以及比特币、NXT和Bitshares等等）的系统基本上是加密经济有机体系中新产生的层级——是完全存在于加密空间的去中心化、无权威介入的实体，并由加密学、经济学和社会共识机制共同维系的。它们有点像是BitTorrent，但又有所区别，因为BitTorrent并没有状态的概念——结果证明这是至关重要的一个区别。它们有时候会被形容为“[去中心化的自治公司](https://letstalkbitcoin.com/is-bitcoin-overpaying-for-false-security)”，但它们又不是特别公司化，比如你并不能对微软公司实行硬分叉。它们像是开源软件项目，但两者之间也并不是特别像，你可以对区块链进行分叉，但这又没有分叉OpenOffice那么容易。

这些加密经济网络多种多样，有基于ASIC的PoW、基于GPU的PoW、朴素PoW、委托PoS，还有未来有希望实现的Casper PoS，而且每一种都不可避免地会有它自己背后的哲学。一种比较著名的例子就是以工作量证明机制为最高纲领。在这种机制中，会将矿工投入了最大数额的经济资本去创建的单条区块链定义为“唯一正确”的区块链。原本这只是协议内的分叉选择规则，但这种机制却在很多情况下被上升为一种神圣的信条。作为示例，可以[看一下我和Chris DeRose在Twitter上的讨论](https://twitter.com/vitalikbuterin/status/687050458301657088)，它展现了一个人即使是在面对协议中哈希算法不断改变的硬分叉时，还是以他纯粹的形式为这种想法辩护。Bitshares的[委托权益证明机制 \(DPoS\)](https://bitshares.org/technology/delegated-proof-of-stake-consensus/) 展现了另一种符合逻辑的哲学，也就是一切又再次从单一的信条衍生而来。这种信条可以更简单描述为：[股东投票](http://docs.bitshares.org/bitshares/dpos.html)。

其中的每一种哲学：包括中本（聪）共识机制，社会共识机制，股东投票

共识机制，都诞生了一套自身的结论，并形成了一种基于其自身观点来看颇有道理的价值体系，尽管在互相比较时一定会受到批判。Casper共识机制也有其哲学基础，尽管至今还没能以一种简要清晰的方式描述出来。

我、Vlad、Dominic、Jae和其他人都对权益证明协议存在的原因以及如何去设计这一类协议都有各自的看法，但本文仅阐释我个人的观点。

我会直接进一步列出观察情况以及结论。

* 密码学在21世纪中确实是非常特殊的，**因为在对立冲突中仍大多站在防御者一方的领域已经不多了，密码学就是其中一个**。比起建造一个城堡，摧毁它会更加容易；岛屿的防御性更强，但也会被袭击；但是一个普通人的椭圆曲线密码 \(ECC\)密钥却能足够安全，甚至能抵御国家级的入侵。密码朋克的哲学本质是利用这种宝贵的非对称性来创造一个能够更好地保护个人自由的世界。密码经济学从一定程度上说是密码学哲学的延伸。不同的是密码经济学保护的不仅是个人信息的隐私和安全，也要保护复杂的协作系统的安全性和活力。**把自己看作是密码朋克精神继承者的系统应该保持这种基本属性，毁坏一个系统要远比使用和维护系统的代价更高。**
* “密码朋克精神”并不单单只是理想主义，而建造一个易守难攻的系统，单就工程设计而言也理应如此。
* 在一个中期到长期的时间范围里，人们非常擅长共识。**即使敌手拥有无限的哈希算力，并且能对任意主要区块链系统进行51%攻击，甚至将其回滚到一个月前，但比起超越主链的哈希算力，要说服社区该链具有有效性要难得多。他们还需要篡改互联网上许多其他信息源，例如区块浏览器、社区中每一位可靠的成员、纽约时代、archive.org等等。总言之，在信息技术发达的21世纪，攻击者想要说服全世界接受他攻下的区块链，难度大概不亚于说服全世界美国没有登陆过月球。**因此，归根结底这些社会因素才是区块链的长期保障，无论区块链社区是否承认这一点（Bitcoin Core[确实承认](https://www.reddit.com/r/Bitcoin/comments/3fg0jw/could_a_cartel_of_pool_operators_collude_to/ctoat0d/)了社会层面的首要性）\*\*。
* 然而单单由社会共识保障的区块链还是太低效率了，运行的速度也不够快，并且很容易让分歧无休止地持续下去（不管怎么去防止它，结果[还是发生了](http://www.npr.org/sections/money/2011/02/15/131934618/the-island-of-stone-money)）；因此，**在短期内，经济共识机制在保护区块链活性以及安全性上起到了非常重要的作用。**
* 因为只能用区块奖励保证工作量证明机制的安全性（用Dominic William的话来说，就是三个Es当中少了两个）译者注：即Entry cost \(进入成本\)，Exist cost \(存在成本\), Exit penalty \(退出惩罚\)，再加上矿工的激励仅仅来自于他们可能失去区块奖励的风险，因此，工作量证明机制的运行逻辑是：通过巨额奖励来催生大量算力。在PoW 当中要想从攻击中恢复过来是非常困难的：如果它是第一次发生，你可 以通过硬分叉改变工作量证明，这样就可以使得攻击者的ASIC失效，但如果再次发生的话，你就没得选择了，所以攻击者可以一而再再而三 地攻击。因此，挖矿的网络的规模要足够大才能降低攻击的风险。假设网络每天的算力成本是X，那么就能阻止规模小于X的攻击者出现。**我反对这一逻辑是因为（i）PoW会**[**消耗大量能源**](http://digiconomist.net/beci)**；（ii）PoW并不能实现密码朋克精神，因为其攻守成本是1：1，所以根本没有防御者优势。**
* PoS权益证明机制不再依靠为网络安全性提供奖励的机制，而是通过惩罚措施来打破这种对称性。质押资金（存款）的验证者会得到小小的奖励，这是为了对他们锁定资本、维护节点以及还要额外警惕私钥安全性做出的补偿，但是回滚交易受到的惩罚是他们同时间所获奖励的成百上千倍。因此权益证明机制的“一句话哲学”并不是“消耗能源来获得安全性”，而更应该是“提高损失的经济价值来保障安全性”。如果说一个给定的区块或状态享有价值X的安全性，前提是你得证明任何冲突区块或状态无法达到相同等级的最终确定性，除非恶意节点勾结起来支付价值X的协议内罚金。
* 理论上来说，大多数验证者勾结起来有可能会控制权益证明区块链，然后就开始作恶。然而（i）通过巧妙的协议设计，他们通过这种操纵手段攫取利润的能力就会尽可能被限制，而且更重要的是，（ii）如果他们尝试阻止新的验证者参与网络，或是执行51%攻击的话，那么社区就可以简单地协调好某个硬分叉并清除行为不端的验证者的存款。一次成功的攻击可能会耗费五千万美元，但比起[2016.11.25那一次的geth/parity共识错误](https://blog.ethereum.org/2016/11/25/security-alert-11242016-consensus-bug-geth-v1-4-19-v1-5-2/)处理情况来看，收拾残局的进程不会太艰巨。两天之后，区块链和社区会回到正轨，攻击者损失了五千万美元， 而由于攻击事件之后的供应量紧缩，代币的价值会上涨，社区成员可能会有所受益。这即是攻击和防御的不对称性。
* 上述并不能拿来表明非计划性的硬分叉将来会发展成为规律性事件；必要时，可以将在PoS中发起单次51%攻击的成本设置得和在PoW中进行永久的51%攻击一样高。这样庞大的费用和攻击的低效性应该能够保证在实际状况中不会有人尝试攻击。
* **经济学并不是万灵丹**。有些个人可能是出于协议外的动机，比如说他们的计算机可能会遭到入侵、他们可能会被挟持或者可能仅仅因为某一天喝醉了，然后决定破坏这条区块链，完全不计成本。再者，就积极的一面来说，个人的道德自制和沟通低效会将攻击所需的成本提升到比协议定义的损失价值 \(value-at-loss\) 更高的水平。这是我们不能依赖的优势，但与此同时它也是我们不应该觉得没有必要就抛弃的优势。
* **因此，最优的协议应该是那些在多种多样的模型和假设当中仍能够正常运行的协议**——具备协调选择的经济理性、具备个人选择的经济理性、简单的容错机制、拜占庭容错机制（在理想‘情况下既是适应性也是非适应性的对抗变体）、受到[Ariely/Kahneman启发的行为经济模型](https://www.amazon.ca/Honest-Truth-About-Dishonesty-Everyone-Especially/dp/0062183613)（“我们都只是轻微作弊”）以及在理想条件下既具有现实意义又具有实践意义的经得起推敲的模型。**重要的是要做好双层防御：防止中心化企业联盟做出反社会行为的经济激励，和一开始就防止企业联盟形成的反中心化激励。**
* **运作充分快速的共识协议具有一定风险，需要非常谨慎地对待**，因为如果系统效率和激励挂钩，那么这样的结合将会带来高额奖励，以及足以引发系统性风险的**网络层中心化**（例如所有的验证者都在同一个主机服务商中运行）。有些共识协议并没有这些担忧，这类协议并不要求验证者发送信息有多快，只要他们能够在在可接受的时间间隔内发送信息就行了（4-8秒，根据经验我们知道以太坊延迟时间通常在500毫秒-1秒）。一个可能的折中就是，创建一种快速运行的协议，但其中可以应用和以太坊叔块类似的机制，以确保节点的网络连接度超过了某个易达到的程度之后，其边际收益是非常低的。

至此，对一些具体细节肯定还有很多不同的情况和方法，但上述说法至少是我的Casper版本所基于的核心原则。我们当然还可以讨论互相竞争的价值观之间的利弊。是年发行率1%的ETH和成本五千万美元的修复性硬分叉，还是年发行率为0的ETH和成本五百万美元的修复性硬分叉？我们该什么时候通过在容错模型下降低安全性作为在经济模型下提高协议安全性的交换呢？可预测的安全性和可预测的发行率，我们更在意哪个？这些都是留待另一篇文章探讨的问题，而至于对这些价值进行权衡的多种方式，则需要更多的文章来回答，但我们迟早会讨论到 🙂  


