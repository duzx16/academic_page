---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "读书感想：《哈利波特与理性之道》"
subtitle: ""
summary: ""
authors: [admin]
tags: []
categories: [阅读]
date: 2020-02-22T21:26:09+08:00
lastmod: 2020-02-22T21:26:09+08:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

为了记录自己最近看的书决定开两个专栏，一个是读书笔记，用来记录偏技术性的读书总结，一个是读书感想，用来发表非技术性的读书体会。本篇比较接近一篇安利贴。

这是一本《哈利波特》的同人小说。《哈利波特》的原著是我阅读次数最多的小说之一，当然都是在初中时期看的。我觉得这也是比较容易欣赏这本同人的要求：对原著小说有一定的了解，*但是又没有热爱到不允许出现任何和原著不符的情节和人物*。

当下大部分的《哈利波特》同人无外乎是两种类型，一种是把自己当成原著剧情的补充和扩展，专注于原著主线之外的情节和人物关系；另一种则是只保留原著的世界观，但是讲述完全不同的故事，甚至连主人公都变了（《舌尖上的霍格沃滋》）。而本书则恰好介于两者之间：本书的剧情完全基于原著第一部《魔法石》，没有引入任何新的人物，但是整个情节发展却完全不同。不同在于，如果说原著小说是给儿童的童话故事，而本书则是给成年人的奇幻小说。

### 用理性精神探索魔法世界

本书的世界线与原著的最明显不同在于，由于某些原因，哈利的姨妈并没有嫁给德思礼，而是嫁给了牛津大学的物理学教授万瑞斯。哈利从小就收到了万瑞斯教授的呵护和教育，收到了良好的理性精神和科学方法的训练。而在他十一岁的时候，他像原著一样收到了猫头鹰寄来的霍格沃滋的录取通知书。就这样，一个有着科学家头脑的男孩走入了充满不合理的魔法世界。

因为这种科学世界观被魔法世界冲击的感受，本书带上了浓厚的科幻色彩。当哈利见到麦格教授在他面前变成一只猫时，他因为能量守恒被打破和猫的大脑居然能承载人的思想而跳了起来。值得注意的是，本书并没有强行用现有的科学知识去解释魔法，而是很快承认了魔法的存在打破了现有的科学定理。但是，虽然已有的知识被打破了，但是发现这些知识的方法却没有失效。哈利的人生理想很快变成了用科学的方法论来探索魔法的原理，从而实现自己改造世界的目标。

在哈利不断接触魔法世界的过程中，作者也为原著中稍显不合理的设定增加了更为自洽和合理的解释。这一类情节的高峰就是时间转换器的相关情节。对原著比较了解的人都知道，时间转换器是原文中非常严重的一个bug。它的能力过于强大，以至于罗琳也不得不在第五部将它们全部毁掉。时间转换器涉及的是科幻小说中一个经典的主题：回到过去。这里作者采用的是“时间线唯一且过去不可改变”的设定，即不存在平行宇宙，并且回到过去无法改变过去。未来的人回到过去的所作所为构成了过去的一部分。如哈利所说，这意味着整个世界是一个超图灵机，因为当下的计算可以利用来自未来的信息。这一设定可谓科幻味十足，并且充满了可利用的空间。对于未来的自己来说，已经知道的事实是无法改变的，但是还不知道的事实却可以回到过去加以影响。因此只要自己不知道某个人是否已经知道了某条信息，就可以利用时间转换器回到过去，将这条信息告诉他/她。

对于受过科学训练的人来说，另一个剧情上的吸引力在于用科学方法去探索和解释未知世界。这一类情节的高峰同样是关于时间转换器。当得知关于时间转换器的上述设定之后，哈利立刻意识到，这种超图灵机结构完全可以被用来构建解决NP问题的高效算法。于是他进行了如下实验：请一个朋友随机选择100-999之间的两个质数，将两者的乘积告诉自己。然后自己回到房间，捡起未来的自己写给自己的纸条（称为纸1）。然后从笔记本上撕下一张纸（称为纸2）。根据纸1的内容在纸2上书写。如果纸1为空，则在纸一上写下“100 100”。否则将纸1上的两个数字相乘，如果乘积与之前的乘积相同，则将两个数字写在纸2上。否则将第一个数字加1，如果第一个数字超过999，则重置为1000，并将第二个数字加1。然后用时间转换器将纸2送回过去的房间，成为纸1。可以看出，唯一能够自洽的时间线就是纸1和纸2上都是当初选择的两个质数。这实际上就成了分解因数的一个多项式时间的算法。而分解因数一直被认为是NP问题。如果你对计算机比较了解的话，这一段剧情完全可以让你达到颅内高潮。顺便一提，这个实验的结局是，哈利在纸1上看到了自己的字迹写的“不要和时间开玩笑”，然后用颤抖的手抄到了纸2上。

### 扎根原著又超越原著的人物设定

虽然本书的时间线仅仅是原著第一部，但是作者将七本书的元素和人物都已经提前融了进去（毕竟在第一年末尾主角就打败伏地魔了）。这些出场的人物大部分都符合原著，比如海格、麦格、斯内普。而有的人物相比原著却做了大幅度修改。这其中最明显的一个是主角哈利，相比起原著中和他爸爸一样有点鲁莽的哈利·波特，这个哈利·万瑞斯要更加聪明。但是如果你又不得不承认，原著中哈利的天性如果在一个知识分子家庭中成长，最终得到的就是这样一个角色。

而彻底推翻原著设定的则是大反派伏地魔。原著中的伏地魔是一个只能存在于童话中的反派。他没有自己的理想，除了搞恐怖活动之外他的食死徒群体没有目标。而且以原著中魔法部的禁戒疏散，伏地魔这样不择手段的反派早就应该控制了整个魔法部（只需要对几个高级官员施展摄魂咒）。