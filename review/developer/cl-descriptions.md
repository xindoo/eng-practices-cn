# 写一个好的CL描述

一个CL描述是做了**什么**更改以及**为什么**的公开记录。
它将成为我们版本控制历史的永久组成部分，多年来除你的评审者以外，还可能有数百人会阅读它。
将来的开发者将会基于它的描述来搜索你的CL。
将来可能会有人由于没有现成的细节，而根据他模糊的记忆来寻找你的改变。 
如果所有重要的细节都在代码中而不是描述中，那么他们将很难定位你的CL。

## 第一行

*   言简意赅
*   语义完整 
*   空行隔开 

CL描述的**第一行**应该是对CL正在**做的***具体*工作的简短总结，紧跟一个空行。
在未来，这是大多数的代码搜索者在浏览一段代码的版本控制历史时会看到的，因此第一行应该足够有信息量，他们不必阅读你的CL或它的整个描述就可以大致了解你的CL实际做了什么。
按照传统，CL描述的第一行是一个完整的句子，就像它是一个命令（一个祈使句）。
例如，说\"**Delete** the FizzBuzz RPC and **replace** it with the new system."，而不是\"**Deleting** the FizzBuzz RPC and **replacing** it with the new system."。
不过，你不必把其余的描述写成祈使句。

## 正文内容丰富

其余描述应该是具有丰富的内容。它可能包括对正在解决的问题的简要描述，以及为什么这是最好的方法。 如果这种方法有任何缺点，就应该提到。将相关的背景信息（例如bug数目，基准测试结果），以及设计文档的链接。即使是很小的CL也值得关注细节。请将来龙去脉放入CL中。

## 反例
"Fix bug"是一个不充分的CL描述。是什么bug？你是怎么修复它的？
其他一些类似的不好的CL描述:

-   "Fix build."
-   "Add patch."
-   "Moving code from A to B."
-   "Phase 1."
-   "Add convenience functions."
-   "kill weird URLs."

这里有一些是真实的CL描述。他们的作者可能认为他们提供的信息是有用的，但他们并没有给出一个CL描述的意图。

## 好的CL描述 

这里有一些好的CL描述的例子.

### 功能改变

> rpc: remove size limit on RPC server message freelist.
>
> Servers like FizzBuzz have very large messages and would benefit from reuse.
> Make the freelist larger, and add a goroutine that frees the freelist entries
> slowly over time, so that idle servers eventually release all freelist
> entries.

前几个词描述了CL描述实际做了什么。其余描述在说明解决的问题，为什么这是一个好的方案，以及具体实现的细节。

### 重构

> Construct a Task with a TimeKeeper to use its TimeStr and Now methods.
>
> Add a Now method to Task, so the borglet() getter method can be removed (which
> was only used by OOMCandidate to call borglet's Now method). This replaces the
> methods on Borglet that delegate to a TimeKeeper.
>
> Allowing Tasks to supply Now is a step toward eliminating the dependency on
> Borglet. Eventually, collaborators that depend on getting Now from the Task
> should be changed to use a TimeKeeper directly, but this has been an
> accommodation to refactoring in small steps.
>
> Continuing the long-range goal of refactoring the Borglet Hierarchy.

第一行描述了CL做了什么，以及它是如何与过去发生变化的。
其余的描述将讨论具体的实现、CL的来龙去脉、解决方案的不理想以及未来可能的方向。
它同样是解释*为什么*要进行这个修改。

### 需要上下文的小CL

> Create a Python3 build rule for status.py.
>
> This allows consumers who are already using this as in Python3 to depend on a
> rule that is next to the original status build rule instead of somewhere in
> their own tree. It encourages new consumers to use Python3 if they can,
> instead of Python2, and significantly simplifies some automated build file
> refactoring tools being worked on currently.


第一句描述了实际做了什么。其余的描述解释了*为什么*要进行更改，并给了reviewer很多背景。

## 在提交CL之前，请检查描述

在Review期间CL可能会经历重大改变。在提交CL之前，有必要review CL描述，以确保描述仍然反映CL所做的事情。

下一章: [简短的CL](small-cls.md)
