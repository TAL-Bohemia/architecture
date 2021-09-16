# 【译】Google 官方文章——如何去做code review

> Google 前几天公开了一篇谷歌的工程实践文档。而且文档的内容都是跟 code review 相关的内容，里面包含了 Google 工程师如何进行 code review 的内容，以及 code review 指南。 原文地址: [google.github.io/eng-practic…](https://google.github.io/eng-practices/review/reviewer/)

本文的名词解释：

- cr: code review
- cl: change list，指这次改动
- reviewer: cr的那个review人
- nit: 全称nitpick，意思是鸡蛋里挑骨头
- 作者: 也就是本次CL的开发者，原文中是以author来称开发者的。以老外的思维，意思是“CL的作者”

# cr的标准

cr(Code review)主要目的在于确保Google 的代码库代码质量越来越好。而所有相关的工具与流程皆是因应这个目的而生。为达到此目的，势必需要做出一连串的权衡与取舍

首先，开发人员必须能够在自己负责的任务上有所进展。如果你从来没有向代码库提交改进过的代码，那么代码库就永远不会改进。另外，如果一个reviewer使cr都很难进行的话，那么开发人员就不愿意在将来进行改进。

另一方面，reviewer有责任确保每个change list（**下称CL**）的质量，保证其代码库的整体代码质量不会越来越差。这可能很棘手，因为通常会随着时间的推移，代码需要降级才能让代码运行起来，特别是当团队受到严重的时间限制时，大家觉得必须走捷径才能实现他们的目标。

此外，reviewer对他们正在review的代码拥有所有权和责任。他们希望确保代码保持一致、可维护，以及下文的“**在cr中可以得到什么**”中提到的内容。

因此，我们有以下规则，作为我们在cr中所期望的标准：

一般来说，当CL存在的时候，reviewer应该赞成它，此时它肯定会提高正在工作的系统的整体代码质量，即使这个CL并不完美。这是所有cr中的首要原则。当然，这是有局限性的。例如，如果一个CL添加了一个reviewer不希望在其系统中使用的功能，那么reviewer当然可以拒绝，即使代码设计得很好。

这里的一个关键点是，没有“完美”的代码，只有更好的代码。reviewer不应该要求作者在approve之前对一篇文章的每一小段进行润色。相反，reviewer应该权衡发展的需要和他们所建议的change的重要性。reviewer不应追求完美，而应追求持续改进。作为一个整体，提高系统的可维护性、可读性和可理解性的CL不应该因为它不是“完美的”而被延迟几天或几周。

reviewer应该随时可以留下评论，表达一些更好的东西，但如果不是很重要，可以在评论前加上“nit:”这样的前缀，让作者知道这只是一个他们可以选择忽略的修饰点。

## 指导

cr有一个重要的功能，教开发人员一些关于语言、框架或一般软件设计原则的新知识。留下有助于开发人员学习新知识的评论是可以的。随着时间的推移，共享知识是提高系统代码健康度的一部分。你只需要记住，如果你的评论纯粹是教育性的，但对达到本文档中描述的标准并不重要，请在其前面加上“nit:”，或者以其他方式表明作者不必在本CL中解决它。

## 原则

现实和数据推翻了个人喜好。在代码风格方面，风格指南（[style guide](http://google.github.io/styleguide/)）是绝对的权威。任何不在style guide中的一些点（如空格等）都是个人偏好的问题。代码风格应该与现有的一致。如果项目没有以前的统一风格，那就接受作者的风格。

软件设计的各个方面从来都不仅仅是一个纯粹的代码风格问题或者是个人喜好问题。它们是以基本原则为基础的，应当以这些原则为依据，而不仅仅是以个人意见为依据，有时几乎都没有选择的。如果作者能够证明（通过数据或基于原理的一些事实）他的方法是同样有效的，那么reviewer应该接受作者的偏好。否则，代码风格选择取决于软件设计的标准原则。

如果没有其他规则适用，那么reviewer可以要求作者与当前代码库中的内容保持一致，只要这些代码不会恶化系统的整体代码健康状况。

## 解决冲突

在cr的任何冲突中，第一步应该始终是开发人员和reviewer根据本文和[《CL Author’s Guide》](https://google.github.io/eng-practices/review/developer/)，尝试达成共识。

当达成共识变得特别困难时，reviewer和作者需要进行面对面会议，而不是仅仅试图通过cr的注释来解决冲突。（不过，如果这样做，请确保将讨论结果记录在CL的评论中，以供将来的读者阅读。）

如果这不能解决问题，最常见的解决方法就是升级。通常情况下，升级的途径是进行更广泛的团队讨论，让team leader参与进来，请求代码维护人员做出决定，或者请求技术经理提供帮助。不要因为作者和审稿人不能达成一致意见而让一个其他人袖手旁观。

# 在cr中要看些什么

> 注意：在考虑每一点时，一定要考虑cr的标准。

## 设计

在cr中重要的是看CL的总体设计。CL中不同代码段的交互是否有意义？此更改属于你的业务代码库还是属于引进来的其他代码库？它是否与系统的其他部分很好地集成？现在是添加此功能的合适时机吗？

## 功能

这个CL做了开发者想要的吗？开发者对这些代码的设计初衷用户有好处吗？“用户”通常既是最终用户（当他们受到更改的影响时）又是开发人员（他们将来必须“使用”此代码）。

大多数情况下，我们希望开发人员在进行cr时能够对CL进行充分的测试，使其能够正常工作。但是，作为reviewer，仍然应该考虑边缘状况，寻找问题，尝试像用户一样思考，并确保仅通过阅读代码就不会看到错误。

如果你愿意的话，你可以自己去验证CL。如果改动会直接带来的用户可见的影响，比如说ui改动，验证CL的变化是非常关键的。 在阅读代码时，很难理解某些更改会对用户产生怎样的影响。对于这样的更改，如果不方便自己测试，则可以让开发人员演示该功能(demo)。

另外，在cr期间考虑功能性特别重要的点是，cl中是否并发式编程，理论上可能导致死锁或竞争条件。这些类型的问题很难通过运行代码来发现，通常需要有人（开发人员和reviewer）仔细考虑，以确保不会引入问题。（注意，这也是不使用平行式编程的一个很好的理由，在这种情况下，可能出现竞争条件或死锁，这会使代码检查或理解代码变得非常复杂。）

## 复杂性

一个CL是否复杂到超过预期的必须？针对任何层级的CL必须确认这几点：单行代码是否过于复杂？函数是否过于复杂？class是否过于复杂？“复杂”通常意味着该代码很难阅读，很有可能也意味着其他开发者在修改这段代码的时候引入其他bug

其中一种复杂性就是过度设计(Over-engineering)造成的，开发者会让那段代码过度通用化，超过了原本所需，或者还新增了系统目前不需要的功能。reviewer应特别注意一下过度设计。鼓励开发者解决他们知道现在需要解决的问题，而不是推测将来可能需要解决的问题。当那些将来出现的问题出现的时候才开始解决它们，因为那时候你可以更加清晰看见问题的原样子

> Donald Knuth说过：过早的优化是万恶之源(Premature optimization is the root of all evil)

## 测试

请将单元测试、整合测试、端到端测试视为要求CL所做的适当变更。一般CL内除了生产环境的业务代码外，测试也应该要被加入其中。除非该CL是为了处理某个紧急事情而存在。

另外，也要确保测试是正确、合理、有用的。测试并非来测试它们本身，一般也极少为了测试而测试（如测试一下测试代码有没有问题又走了测试流程），因此我们要保证测试是有效的。

当代码真的有问题，测试是否会失败？如果被测试的程序发生改动时，测试是否会产生误报？每一个测试是否做出了简单而有用的断言？不同的测试方法之间的测试是否适当分开？

请记住，测试代码也是必须维护的代码，不要因为它们不在主要关注的范围内。

## 命名

开发者是否为了每一个东西都挑了一个适当的名字？一个好的命名意味着，通过名字就足以完整表达该东西的作用是什么或者要做什么。但是同时名字也不要长得难以阅读

推荐参考文章[「Clean code 101 — Meaningful names and functions」](https://medium.com/coding-skills/clean-code-101-meaningful-names-and-functions-bf450456d90c)

## 注释

开发者是否用可理解的英文留下清晰的注释? 这些注释是否真的必要?

通常注释是解析这段代码为什么存在的时候是相当有用的，而不应该去解释某段代码正在做什么。如果代码本身不能解释清楚的话，意味着它更加需要简化了。当然也有例外，比如解释正规的表达式或者复杂的算法正在做什么的时候，注释解释这段代码正在做什么就相当有用。但对于大部分注释来说是用来说明那些不包含在程序本身但资讯，比如说为什么要这样子做的理由

查看该CL之前的注释也很有帮助，或许有一个todo项目现在可以一处、一个评论建议为什么不要进行这种更改等等。

要注意的是，注释与class、module、function的文件不同。后三者要能够表达一段代码的目的、如何使用它、使用时行为

## 风格

Google 对于主要语言都有提供风格指南（style guide），甚至包括大多数冷门语言，因此要确保CL遵循适当的指南上的说明。

如果你想改进CL中某些不包含在风格指南中的要点时，请在评论前加上Nit: ，让开发人员知道这是你认为可以改善代码的小问题且并非强制性的。但记住，不要仅根据个人风格偏好阻止提交CL。

开发者不应该在 CL内同时包含主要风格的改动与其他代码的修改，这样会导致难以看出CL到底做出什么改动。同时也会让合并和回滚更为复杂，并产生其他问题。例如，如果作者想要重新格式化代码的话，让他们将新格式在一个新CL里面重构。

## 文档

如果CL更改了构建、测试、交互、发布的时候，请检查是否也同时更新相关文档， 包括README，g3doc 页面和其他生成(generated) 的参考文件。如果CL 删除或弃用 了一些代码，请考虑是否应该删除对应文档，如果缺少文档时请询问。

## 每一行代码

仔细review分配给你的每一行代码。有些东西，比如资料文件(data files)、生成的代码(generated code)、大型数据结构(large data structures)，你可以稍微扫过。千万不要在扫过开发者写的 class、函数、代码区块 时，去假设它内部是没问题的。 很显然的某些代码需要比其他代码更仔细的review。这是必须由你做出的判断，但至少你应该确定你理解所有代码在做些什么。

如果阅读代码过于复杂并且减慢review速度时，那么你再继续review前，要让开发者知道这件事，并等待他们为这段代码做出解释、说清楚。在Google 我们聘请许多优秀的软件工程师，而你也是其中的一员。如果连你也无法理解的话，很可能其他人也不会。因此，你要求开发者去说清楚这段代码时，同时也在帮助未来的开发人员理解这些代码。

如果你能够理解，但觉得没有资格进行某部分的审核，请确保 reviewer中有一个适合(合格) 的人来review该部分。尤其是针对安全性、并发性、可访问性、国际化等复杂问题时。

## 上下文

在充足的上下文下查看CL通常很有帮助。一般来说，cr工具只会显示修改部分周围的几行代码而已。但有时你必须查看整个文件以确保改动是否合理。比方说，你可能只看到添加4行新代码，但实际上查看整个文件时会发现这4行是被加在有50行的代码里，此时需要将它拆解为更小的函数。

以整个系统的角度出发来思考CL也是很有用的。该CL是否改善整体系统的代码质量，亦或是会让整个系统更加复杂?是否缺少测试?千万不要接受会降低整体系统的代码质量的CL。因为大多数系统是由于许多小改动的积累而变得复杂，因此阻止新的改动引入复杂性(尽管很小)也非常重要。

## 优点

当你在CL里看到优点时记得告诉开发者，尤其是当他们用很棒的方式来解决你的评论时。cr通常只关注存在的错误，但它们也应该同时应该为良好实践提供鼓励与欣赏。这点在指导开发者时尤其重要:与其告诉他们做错什么，还不如告诉他们做对了什么。

个人认为并非不要指出错误，而是多以鼓励来代替指出错误，让其他开发者更有动力想将事情做好。其实透过简单的一句话让对方知道哪里做得很好，未来他们会将继续保持下去，并为其他开发者带来的正面影响



![img](https://user-gold-cdn.xitu.io/2019/9/18/16d4028708d451b5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

标明每个commit 修改什么，可以帮助reviewer快速进入状况





![img](https://user-gold-cdn.xitu.io/2019/9/18/16d402899ff008ac?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

此时就不要吝啬你的称赞了!



## 总结

在cr时，请务必确保：

- 代码经过完善的设计
- 功能性对于使用者们是好的
- 对于任何UI改动要合理且好看
- 任何并行编程的实现是安全
- 代码不应该复杂超过原本所必须的
- 开发者不该实现一个现在不用而未来可能需要的功能
- 代码有适当的单元测试
- 测试经过完善的设计
- 开发者对于每样东西有使用清晰、明了的命名
- 注释要清楚且有用，并只用来解释why而非what
- 代码有适当的写下文件(一般在g3doc)
- 代码风格符合style guide
- 确保你查看被要求review的每一行代码、确认上下文、确保你正在改善代码质量，并赞扬开发人员所做的好事与优点吧!

# 如何浏览CL

现在你已经知道review时要看些什么，但怎样才是review分散在多个文件中的改动最有效的方法?

1. 改动是否合理?他是否有好的描述(description)
2. 优先看CL 最重要的改动。它整体是否有完善的设计?
3. 用合理的顺序看CL 剩余的改动

> 步骤1: 用宏观的角度来看待改动，查看CL描述以及它做什么

该改动是否有意义、合理？如果在第一时间认为不应该发生这种变化，请立即说明为什么不该这样做的原因。当拒绝类似这样的更改时，向开发人员提供建议告诉他们应该怎么做什么也是一个好主意。

例如，你可以说：“看起来你已经完成一些不错的事情，谢谢!但我们实际上正朝着删除你正在修改的FooWidget系统的方向前进，所以现阶段我们不想对它进行任何新的修改。 不如重构我们的新BarWidget class如何?”

需要注意的是，reviewer在委婉拒绝该CL的同时也要提供替代方案，而且仍然保持礼貌。这种礼貌是很重要，因为我们希望表明即使不同意也会相互保持尊重。

如果你有几个CL里包含你不想这样做的改动时，你应该重新考虑开发团队的开发过程，或发布开发流程给外部贡献者知道，以便在任何CL被撰写前有更多的沟通。最好是在他们开始投入前说“不”，避免那些已经投入心血的工作现在必须被抛弃或彻底重写。

提供替代方案让对方知道该怎么做，而非让其自行独自摸索。



![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1200" height="788"></svg>)

指出问题，告知替代方案或指点方向



> 步骤2: 检查CL主要的部分

找到CL最核心的部分的那些文件。通常CL内会有文件包含大量的逻辑改动，而它正是CL的主要部分。因此我们要首先查看这些主要部分。这有助于为CL的其他较小部分提供适当上下文，而且这样通常可以提高review速度。如果CL太大导致于无法确定哪里是主要部分时，请向开发者询问首先应当查看的内容，或者要求他们将CL拆分为多个CL。

如果在主要部分发现存在一些主要的设计问题时，即使没有时间立即查看CL的其余部分，也应立即留下评论告知此问题。因为事实上，因为该设计问题足够严重的话，继续review其他部分的代码可能只是浪费时间，因为其他正在审查的其他代码可能都将无关紧或消失。

立刻发送关于主要设计的评论非常重要有两个主要原因:

- 通常开发者在送出CL后，在等待review过程中便已经开始着手基于该CL的新工作。此时如果正在review的CL存在重大设计问题的话，开发者将不得不重新写所有基于它的后续所有CL。因此你要在他们有问题的设计上投入之前阻止他们。
- 主要设计变更通常需要较长时间才能完成，但每个开发人员几乎都有自己deadline。为了赶上deadline并保证代码质量，开发者需要尽快开始或重做CL 任何的主要设计变更。

> 步骤3: 用合理的顺序看CL 其余的改动

一旦确认整个CL没有重大的设计问题时，试着找出一个有逻辑顺序来review剩余档案，并确保不会错过其中任何一个。通常在浏览主要部分后，最简单的方式是按照cr工具提供的顺序来浏览每个文件。有时在阅读主要代码前先阅读测试也是非常有帮助的，如此一来你就可以了解应该做、看些什么。

# review的速度

## 为什么review速度要快

在Google我们优化开发团队共同生产产品的速度，而不是优化个人开发的速度。个人的开发速度很重要，但它不如整个团队的开发速度重要。当cr很慢时，会发生以下几件事:

- 团队整体的速度下降。review慢的话，对于团队其他人来说重要的新功能与缺陷修复将会被延迟数天、数周甚至数月，只因为它们正在或者等待review。
- 开发人员开始抗议cr。若reviewer每隔几天才回应一次，但每次都要求对CL进行重大更改的话，那么开发人员可能会感到非常沮丧与觉得困难，而这通常会演变成抱怨。如果reviewer请求相同的实质性更改(且确实可以改善代码质量状况)，但在每次开发人员提交新的改动时都能快速反应的话，这些抱怨往往会消失。**大多数关于cr的抱怨，往往可以通过加快流程来解决。**
- 代码质量会受到影响。当review很慢时，会造成开发者提交不完全尽如人意的CL 的压力越来越大。评论的越慢也会阻止他人进行代码清理、重构、甚至是对现有CL 的进一步改进。

## cr应该要多快？

如果你并没有处于需要专注工作的时候，那么你应该在CL被提交后尽快进行review。review回复最长的极限是一个工作日。若遵循以上指南，意味着一般的CL应该在一天内得到多轮review(如果必要的话)。

## 速度vs中断

但有时候个人的速度优先度会胜过团队速度。如果你处于需要专注工作的时候(比方说写代码)，不要打断自己去做cr。

研究证实，若开发者在被打断后会需要很长时间才能恢复到原本顺畅的开发流程。因此，开发的时候，打断自己实际上会比让另一位开发者等待review来得更加昂贵。

取而代之的是，我们可以在投入到处理他人给的review评论之前，找个适当的时机点来进行cr。这有可能是当你的当前开发任务完成后、午餐、刚从会议脱身或从微型厨房回来等等。

## 快速回应

当我们谈论cr的速度时，我们关注的是回应时间，而非CL需要多长时间才能完成并提交。在理想情况下，整个过程应该是快速的。

总的来说个人回应评论的速度，比起让整个cr过程快速结束来得更为重要。即使有时需要很长时间才能完成整个流程，但若在整个过程中能快速获得来自reviewer的回应，这将会大大减轻开发人员对于缓慢的cr过程的挫败感。

如果真的忙到难以抽身而无法对CL进行全面review时，你依然可以快速的回应让开发者知道你什么时候会开始审核、建议其他能够更快回复reviewer，又或者提供一些初步的广泛评论。(注意:这并不意味着你应该中断开发去回复——请找到适当的中断时间点去做)

很重要的是，reviewer员要花足够的时间来进行review，确保他们给出的LGTM，意味着“此代码符合我们的标准”。

尽管如此，理想的个人的回应速度还是越快越好。

## 跨时区review

在面对时区不同的问题时，尽量在他们还在办公室时回复作者。如果他们已经回家了，那么请确保在他们第二天回到办公室前完成。

## LGTM 评论

为加快速度，在某些情况下reviewer可以给予LGTM或Approval，即便CL上仍有未解决的评论。类似情况会发生在:

- reviewer确信开发人员会适当地处理所有剩余的评论
- 剩余的评论是微不足道的或它们不需由开发者来处理
- reviewer必须清楚指明他们是指上面哪种情况

LGTM 评论对双方处于不同的时区时尤其值得考虑，否则开发人员会等待一整天才得到“LGTM，Approval”。

## 大型改动

如果有人要求reivew时，但由于改动过于庞大导致你难以确定何时才有时间review它时，你通常该做的是要求开发人员将CL拆解成多个较小的CL，而不是一次review巨大的CL。这种事是可能发生的，而且对于reviewer非常有帮助，即便它需要开发人员付出额外人力来处理。

如果CL无法分解为较小的CL，且你没有足够时间快速查看整个CL内容时，那么至少对它的整体设计写些评论，并发送回开发人员以便进行改进。身为reviewer，你的目标之一是在不牺牲代码质量的状况下，避免阻档开发人员进度，或让他们能够快速采取其他更进一步的动作。

## cr的能力会随着时间进步

如果你遵循这些准则，并且对于cr非常严格的话，后面你会发现整个cr流程会越来越快。因为开发者学到什么是质量好的代码，并于开头就提交一个很棒的CL，而这正恰好只需要越来越少的时间。而reviewer则学会快速回应，而非在过程中加入不必要的延期。 但不要为提高想象中的速度，而对cr标准和代码质量做出妥协，毕竟从长远来看它实际上并不会让任何事情发生得更快。

## 紧急状况

在某些紧急情况下，CL会希望放宽标准以求迅速地通过整个cr过程。但请看[什么是紧急情况](https://google.github.io/eng-practices/review/emergencies.html#what)来知道哪些情况实际上属于紧急情况，而哪些不符合。

# 如何写review评论

## 如何面对被推迟处理的评论

有时开发人员会推迟处理cr产生的评论。要么他们不同意你的建议，要么他们会抱怨你太严格了。

## 谁是对的

当开发人员不同意你的建议时，请先花点时间考虑一下他们是否是正确的。因为通常他们比你更了解代码，所以他们可能真的比起你来说对代码的某些层面具有更好的洞察力。他们的论点有意义吗?从代码质量的角度来看它是否合理?如果是的话，让他们知道他们是对的，然后让问题沉下去。

但开发人员也并不总是对的。在这种情况下，reviewer应该更进一步解释为什么认为自己的建议是正确的。一个好的解释通常展示了“对开发人员的回覆的理解”以及有关“为什么被要求对出做出改动”等信息。尤其是当reviewer认为给出的建议会改善代码质量时，便应该继续宣扬自己的论点。只要他们相信所需的额外的工作量最终会改善整体代码质量。提高整体代码质量这件事，往往是经由每个微小的改动来发生。有时往往需要几番解释一个建议才能让对方真正理解你的用意。切记，始终保持礼貌，让开发人员知道你有听到他们在说什么，只是你不同意该论点而已。

## 让开发者沮丧

reviewer有时会认为若自己坚持改进的话，会让开发人员觉得沮丧不安。的确开发人员有时会感到很沮丧，但这通常是十分短暂的，甚至后来他们非常感谢你帮助他们提高代码质量。一般来说，只要你在评论中表现得很有礼貌，开发人员实际上根本不会感到沮丧，这些担忧都仅存在于reviewer心中而已。沮丧很多时候是对于cr评论的写作方式有关，而并非来自reviewer对于代码质量的坚持。

## 晚点再来整理干净

一个常见的推迟原因是开发人员希望完成任务(这可以理解)。他们不想通过另一轮cr来批准这个CL。此时他们通常会说在以后的CL进行整理，所以你现在应该要给LGTM。当然部分开发人员非常擅长这一点，并且立即送出一个修复问题的后续CL (follow-up CL)，但根据过往经验，这种后续“清理行为”非常少见。除非开发者在CL获得批准之后立刻进行清理动作，否则这事根本不可能发生。这不是因为开发人员不负责任，而是因为他们可能有很多其他工作要完成，于是清理工作便会在成堆的工作中被遗忘。因此，通常最好坚持开发人员在代码在合并后清理它们。因为让人们「稍后清理」是致使代码库质量状况下降最常见的状况。

如果CL引入了新的复杂性的话，除非是紧急情况，否则必须在提交之前将其处理掉。如果CL导致暴露周围的问题且现在无法解决它们的话，开发人员应该将缺陷记录下来并分配给自己，避免后续被遗忘。又或者他们可以选择在代码中留下TODO的注释并链接到刚记录下的缺陷。

## 关于review严格性的常见抱怨

如果你先前以相当宽松的标准并转趋严格的进行cr的话，一些开发人员会开始大声地抱怨。一般来说，提高review的速度会让这些抱怨逐渐消失。这些抱怨可能需要数个月才会消失，但最终开发人员会看到严格的review所带来的价值，因为严格的review帮助他们产生的优秀代码。而且一旦发生某些事情时，最大声的抗议者甚至可能会成为你最坚定的支持者，因为他们会看到变得review变严格后所带来的价值。

## 解决冲突

如果你遵循前面所有操作，但仍然遇到无法解决的双方之间的冲突时，请参考前面的**cr的标准**以获取有助于解决冲突的准则和原则。

# 译者总结

**cr的标准**

- reviewer 有责任保证CL的质量，作为reivew的代码的owner
- reviewer不应追求完美，而应追求持续改进
- cr具有指导意义
- 代码风格应该与现有的一致。如果项目没有统一风格，那就接受作者的风格
- 解决冲突难以达成共识时，需要面对面或者拉起更大的团队讨论，带上leader

**在cr中要看些什么**

- CL的总体设计
- 功能验证，功能性对于使用者们是好的，对于任何UI改动要合理且好看
- 是不是很复杂，有没有过度设计
- 代码有适当的单元测试
- 测试经过完善的设计
- 规范命名，看见名字就知道是什么
- 合适的注释，注释应该是why而不是what
- 代码风格遵循style guide，如果需要改代码风格应该在另一个CL解决
- CL更改了构建、测试、交互、发布的时候，也要更新文档
- 仔细review每一行代码(除了资源文件、生成的代码、大型数据结构)。如果比较复杂得让开发者解释
- 安全性、并发性、可访问性、国际化等复杂问题时需要更合适的人来review
- 以整个系统的角度出发来思考CL
- review的时候，与其告诉开发者做错什么，还不如告诉他们做对了什么

**如何浏览CL**

- 用宏观的角度来看待改动，查看CL描述以及它做什么
- 检查CL主要的部分
- 用合理的顺序看CL 其余的改动

**review的速度**

- review速度慢会导致团队整体的速度下降、开发人员开始抗议cr、代码质量会受到影响
- 如果你处于需要专注工作的时候(比方说写代码)，不要打断自己去做cr。
- 个人回应评论的速度，比起让整个cr过程快速结束来得更为重要
- 在面对时区不同的问题时，尽量在他们还在办公室时回复作者
- 为加快速度，在某些情况下reviewer可以给予LGTM或Approval，即便CL上仍有未解决的评论
- 由于改动过大导致难以review时，通常该做的是要求开发人员将CL拆解成多个较小的CL
- cr的速度应该要越来越快，但不要为提高想象中的速度，而对cr标准和代码质量做出妥协

**如何写review评论**

- 当开发人员不同意你的建议时，思考一下谁是正确的，并解释清楚，保持礼貌。
- 如果review坚持己见，会让开发者沮丧。沮丧很多时候是对于cr评论的写作方式有关，并非来自reviewer对于代码质量的坚持。
- 如果CL引入了新的问题的话，除非是紧急情况，否则必须在提交之前将其处理掉。
- 如果现在无法解决review的评论的问题的话，TODO的注释并链接到刚记录下的缺陷。

**review太严格被抱怨在怎么办**

提高review的速度会让这些抱怨逐渐消失。这些抱怨可能需要数个月才消失，但最终开发人员会看到严格的review所带来的价值，因为严格的review帮助他们产生的优秀代码。