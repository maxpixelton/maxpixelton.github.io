---
title: 工程思维：用软件工程方法解决开发难题
author:
  name: superhsc
  link: https://github.com/maxpixelton
date: 2022-04-09 11:30:20 +0800
categories: [系统设计, 设计模式]
tags: [工程思维]
math: true
mermaid: true
---

在工作中，有时候会遇到下面一些情形：
- 团队辛辛苦苦做出的软件产品，却被客户云淡风轻地评价一句：“这不是我想要的。”
- 在新的项目中，作为系统的主要设计者，采用了很多新技术，却发现团队成员的技术水平参差不齐，项目进度不断延期；
- 每次一到季度或年度总结的时候，明明做了很多项目，付出了很多努力，但是被问到这些项目具体取得了哪些收益、对业务有哪些提升时，却顿时语塞。

要解决这些问题，不应只是靠“我应该更努力工作”来解决，更重要的是：是否意识到这些问题背后的本质？是否也有自己的解决方案？
事实上，**在软件开发过程中，已经有了一套相对完善的方法论——软件工程**。

# 软件开发的特点
软件开发到底是一门科学，还是一门工程？这个在业界一直是一个被各方争论的热点问题。在我看来，软件开发兼有两者的特点：

- **从计算机科学角度看**，软件开发需要**关注软件本身运行原理**，比如**时间复杂度**、**空间复杂度**和**算法正确性。**我们需要学习这些基本原理，来帮助自身探索正确的分析和建模方法，从而精进开发技能；
- **从工程角度看**，软件开发更多的是**关注如何为用户实现价值**。我们需要在**时间、资源和人员**这三个主要限制条件下构建满足用户需求的软件，同时还得不断适应新的技术和新的需求变化。

可以这么说，我们并不一定每天都需要使用计算机科学的知识，但是每天都需要使用软件工程的知识。不过，因为常常受到技术思维"**打造尽量完美的软件**"的影响，而忽略了软件工程的真实效用。
比如，在以下这些软件开发场景中：

- 为了体现自身技术实力，项目中常常使用“自研框架”，而不愿采用现有开源程序或者购买稳定合适的组件；
- 在编码中喜欢使用各种最新技术，而没有考虑代码维护时，团队其他成员能否理解这些代码；
- 因为上线任务紧急，编码时很少写单元测试，而采用手工测试；
- 对产品设计和 UI 设计关注不多，需求评审时更注重技术是否好实现，而不太关注用户体验是否更好。

其实，这些问题的本质就在于**总是只关注局部的具体功能实现，而忘记了具体的功能之间要相互配合来完成一个系统的整体功能。**换句话说，**在软件开发时，总是容易太过于关注局部，而没能跳出局部去看整体。**
实际上，类似这样的问题在软件工程中已经有了很多具体解决方案，比如敏捷开发，只要能合理利用软件工程的知识，就能轻松解决这些问题。

# 软件工程的定义
软件开发：

- 从狭义上讲，指的是软件编程（常说的编写和维护代码的过程）；
- 从广义上讲，指的是根据用户要求，构建出软件系统或者系统中软件部分的一个产品开发过程，**是一系列最终构建出软件产品的活动**。

现在，常用“**软件开发过程**”这个词来表述。这个过程包含了 6 个关键活动阶段，用公式表达就是：$软件开发过程 = 定义与分析 + 设计 + 实现 + 测试 + 交付 + 维护$ 
不管是小到一个程序中某个模块的开发，还是大到行业级的软件项目，只要是软件开发，都离不开这个本质过程。
在实际工作中，很容易陷入狭义定义里：软件开发 = 软件编程，这是因为编程是实现最终代码交付的一个常重要的活动，也是耗费时间最多的一个阶段。
软件工程的定义如下:
> **软件工程（Software Engineering），是软件开发里对工程方法的系统应用。** 这里的软件开发指的是软件开发过程，也就是说，**软件工程研究的对象就是软件开发过程**，只不过是借鉴了工程方法来对这个过程进行优化。

随着开发技术不断演化，针对软件开发过程的开发模型也不断兴起，从早期的瀑布式开发模型到后来出现的螺旋式迭代开发，以及近年来流行的敏捷开发，都属于对开发本质过程的不同认识。与此同时，开发人员也在不断探索过程中对应的方法和工具，比如，建模、面向对象方法、面向服务的方法等。
所以，总结起来，**软件工程 = 过程 + 方法 + 工具。**

- 这里的“过程”，就是对软件开发过程的抽象而形成的过程模型，同时包含模型里所使用的方法。
- 而“方法”是指基于这些过程模型的方法集合。因为过程模型决定了软件开发过程是怎样的，进而才决定了应该使用什么样的开发方法。
- “工具”则对应的是过程模型不同阶段中的方法可能用到的工具。比如说，我们选用瀑布模型作为过程模型（如下图），那么对应的软件开发过程就应该是按照瀑布模型的分阶段来进行，对应的方法就是模型中的方法，像需求分析、架构设计……而对应就会使用一些需求分析工具、架构设计工具、文档管理工具。

![软件过程](https://maxpixelton.github.io/images/assert/design-patterns/software_process.jpg)

虽然近些年，云计算、微服务、AI 这些新技术产生，对软件工程产生了影响，但是软件开发过程本质并没有发生变化，依然能使用软件工程对过程模型进行合理抽象，不断优化方法和工具，制订更合适的解决方案。

# 应用软件工程带来的好处
除了能跳出局部看整体，在软件开发中，应用软件工程还有以下两个好处:
1. **提升项目交付的效率。**
   在实际的软件项目中，最想要解决的问题就是：**如何在有限的条件下完成软件开发，并完成交付 !**

   比如说，项目的开发时间是有限的，服务器资源有限，人员有限（哪怕是只有你一个人，时间、精力也是有限的）, 这就决定了在选择技术方案、组件模块、人员时，必须要有所取舍。
   
   此时, 就会出现一个矛盾：
   - 一方面，在软件开发中会遇见大量的技术性问题，如果一味压缩时间不解决问题，那么交付的软件产品必然会出问题; 
   - 另一方面，一旦开始着手解决技术问题，会很容易陷入技术思维（追求完美）的误区里，如果花费大量时间去解决问题，又会导致项目交付延期甚至失败。
  这个矛盾其实就是软件工程想要解决的问题之一，也就是，**如何通过科学的方法去帮助软件开发人员或项目管理人员合理地分配资源，并让资源发挥最大效用**。
  
  换句话说，在学习软件工程后，更能重视对资源的分配，提升思考的维度。就像，开发一款小程序页面，不一定非要自己搭建服务器，可以选择成熟的公有云来提供服务，或者使用一些第三方的小程序服务。这样，只需要基于成熟服务来开发程序，不需要再重新开发基础服务，最终的产品交付效率自然会提升。如果一开始便使用技术思维来做产品，去完善所有的功能需求，则很可能产品没上线，机会就错过了。

2. **提升项目交付的可靠性。**
   交付软件产品，除了要提升效率外，还应该保证产品可靠性。
   
   很多软件项目之所以频繁发生线上事故，都是因为在每一次发布前没有进行充分的测试而导致的，这实际上也说明了对软件开发过程认识不充分。
   
   软件开发过程中之所以会有一个重要的测试阶段，就是因为**软件开发是一件极易出错的事，而测试就是为了发现问题**。然而，很多时候为了赶时间，不断地压缩测试时间，甚至认为测试不那么重要，毕竟业务及时上线看到效果才是更重要的。但是，这就埋下了隐患，技术问题有一个很重要的特性就是会越积越多，然后当问题积累到一定程度后，就会突然爆发导致服务故障。
   
   比如，我曾在某一次迎新时亲眼看见某核心系统突然宕机，原因是该核心系统依赖了一个时间久远的非核心系统，在流量陡增时，非核心系统无法抗住流量而崩溃，导致中断了核心系统的链路处理进而引起雪崩效应。为什么会出现这种情况呢？就是在之前的备战测试中，核心系统的异常处理不够完善，但非核心系统的开发人员以为调用量很小，压根没有进行测试。
   
   通过这个例子，深刻体会到软件开发过程中为什么会专门有一个阶段来做测试了。这并不是为了做流程而做流程，而是因为软件本身的复杂性所决定的，软件一定要经过测试后才能交付和发布，不然极大概率会出现问题。
   
   所以说，**保证软件开发每个阶段的可靠性才能保证软件产品最终的可靠性**。软件工程对过程不断优化的理念，正是提升每一个软件项目交付可靠性的最佳解决方案。

# 应用软件工程的建议
软件工程的三要素：**过程、方法**和**工具。**这里应该重点关注**过程模型，**因为**软件工程的本质就是对软件开发过程的不断优化和反复实践。**

软件开发过程充满了复杂性，前面提到的效率难题、可靠性难题主要是一个项目本身的难题，而软件开发中还有另一个难题是：业务到技术实现。换句话说，就是如何通过组织人进行编码来实现产品和技术的连接。

关于这个难题，结合我的经验来看，总结了五点：
1. 都是项目；
2. 盯紧目标；
3. 提前计划；
4. 有效沟通；
5. 交付结果。
   
## 都是项目
用项目的视角去处理问题或业务, 会带来两点好处：

1. 培养从整体看问题的习惯；
1. 通过做计划，提高时间管理能力。

众所周知，性能优化在任何一个项目中都非常重要。假设在某个项目中，经过努力优化，系统响应时间缩短了 60%，TPS 提升了 2 倍。这个结果说明了什么呢？系统响应时间缩短、吞吐量提升是不是就意味着能满足需求呢？
实际上，这就是只看到局部而忘记了整体。的确，性能优化很重要，但是在当前的项目阶段中，性能优化是不是你需要达到的目标？还是说你的目标只是为了优化而优化？
而当你用项目视角去重新看待这个问题时，就会发现，优化只是一个手段和方法，在合适的阶段引入才是最重要的。比如，你应该提前为项目大促做准备，当大促流量暴增或者某接口超时比例过高时，如果不进行优化，系统可能会存在风险。这样，你不仅会预留一些时间来重新做代码审查，也会看到项目的成败关键并不只是每一次新增功能的完成与否。

## 盯紧目标

主要体现在两个层面：
1. 负责到底；
2. 不断调校。

负责到底是基本要求。 无论是写代码，还是做设计，只要是定下一个目标，就应该始终盯紧目标。比如，你最近要写一个文件上传的模块代码，调研了很多上传方法，然后觉得都特别有意思，想要都试验一次，结果发现用的底层框架并不支持这些方法；于是你转而研究起了框架，又发现这些框架也很有意思，想要做下源码分析，再动手试试……可时间一天一天过去，你却发现文件上传的代码还是没有写完。

不断调校是基于事实的合理改变。 有时，在项目的推进过程中，可能需要修改目标。比如说，需要临时修改紧急线上问题，而这对当前的项目可能会造成一些影响，这就需要敢于基于事实来做合理的评估，而不是固执地认为目标定好以后就不允许修改了。

## 提前计划
最常犯的一个错误就是：拿到需求直接开始编码，计划便是不用计划，用最快的速度去实现代码即可，而这往往会导致更多问题的出现。

在工作中，经常会发现很多开发人员都自诩自己很懂得时间管理，但是难度几乎相同的项目，有的人经常延期，而有的人每次都能按时上线。这是为什么呢？一个很重要的原因就是，这些按时交付的人做事往往具备一个共同特质：做事有计划。
做事有计划是说，要在有限的条件下提前做好分析，并在一个相对固定的时间周期内合理安排任务。简单来说，就是要搞清楚熟悉哪些、不熟悉哪些、哪些可控、哪些不可控……这样就能提前规避风险，并从整体上提升交付效率和质量。

## 有效沟通
如何把复杂的技术问题简单准确地表达出来，让别人听懂并且不产生误解，是每个开发人员都会面临的挑战。
比如，下面是一个研发 A 同学在遇到线上技术问题时，在群里面求助的情况：
> A 同学：大佬们，新架构上线后，数据库不动了。
> A 同学：业务方反馈说店铺活动有问题。
> 其他同学：各种问题讨论中，上下文信息很多……
> A 同学：数据库有问题，报备风险。
> B 同学：先把问题描述清楚。

A 同学此时的心理状态：产品都出问题了，大家赶紧来一起看一下，反正不是我的问题，我只是恰好被提及了，我也把日志和截图发群里面了，谁负责的问题谁自己看。
B 同学可能的心理状态：这啥问题啊，还得自己翻聊天记录看，现在没空，也没接到电话，先不处理。
这里恰好反映了两种典型的被动的沟通心态：

- A 同学的只抛问题不找关联人；
- B 同学的要看问题的轻重缓急再处理。

在平时的沟通群里有很多类似这样的对话，几乎都是在做无效沟通，甚至一个问题出现了几个小时，既没有搞清楚问题是什么，也没有找到责任人是谁。
实际上，当具备了软件工程思维后，提问方式可以变为：
> 从定义问题开始，到业务方期望的目标是什么、实际上发生了什么、收集到了哪些信息，再到自己初步分析的思路是什么，最后自己的计划是什么，都清晰而又系统地表达出来了，比如：
> “现在有一个问题需要解决，问题现象是 xxx，业务方的预期是 xxx，实际看到的是 xxx，不符合预期，从日志和报错看可能是 xxx 出问题了。由于 xxx 项目上线时间紧迫，急需解决，在线等。”

这就是一个典型的有效沟通过程。这种解决思路可以大大提升你的沟通效率，直至问题高效解决。

## 交付结果

在工作中，往往因为编码任务太重，以为代码只要写完，任务就算是做完了，领导或项目组的成员都会自动知道你的项目结果。这是对交付结果最大的误解。实际上软件工程早已对此提出了一种解决办法——发布你的产品，用现在流行的话讲，叫具备**闭环思维。**

比如这样的场景：参加一个需求评审，会上大家各抒己见发表意见却没有提出什么结论和待处理项，等到再次评审的时候，却发现问题越来越多，很多讨论过的问题需要从头再开始讨论。

如果把上面的需求评审会当成一个项目来看，这就是没有发布产品（也就是得到结论），形成不了闭环，导致项目无法完成，那必然也解决不了问题。

其实，交付结果意味着你需要主动结束项目，包括结束确认、结束反馈、测试验证是否通过等，这和等待项目自动结束是完全不同的一种思考方式。

总体来说，软件工程其实就是一种**有目的、有计划、有步骤地解决问题**的方法，并不是只有项目经理和架构师才需要具备，普通开发人员也需要学习和掌握，妥妥地提升编码能力和思考能力。

# 总结
软件工程的概念最早是在 1968 年被提出，为的是应对日益增长的“软件危机”——在软件开发及维护的过程中所遇到的一系列严重问题，距今已有五十多年的历史，现在已经逐渐发展成一门独立的科学学科。

如今，新技术和新方法的不断涌现，让高效地交付可靠的软件产品，变得越来越重要。一个质量不可靠的软件产品，一定会给用户和工程师带来麻烦，甚至造成无法挽回的经济损失，这是很多人都不愿意看到的。

而软件工程的重点研究对象其实就是过程，当过程随着外部技术的进步和发展在发生变化的时候，其对应的方法和工具也在不断发生变化。

换句话说，**这些年软件工程的知识一直在更新迭代中，其“过程+方法+工具”的本质并没有变，无非是各种方法和工具的推陈出新。**
为了更好地应用软件工程方法，应该牢记下面的公式：
- 软件开发过程 = 定义与分析 + 设计 + 实现 + 测试 + 交付 + 维护；
- 软件开发 ≠ 软件编码；
- 软件工程 = 过程 + 方法 +工具。

实际上，软件工程对过程的改进思维，能够应用于很多方面，包括工作中的代码编写、架构设计、需求分析、制订计划、项目管理等。无论你是一个普通工程师还是项目管理者，都应该学点软件工程方法。