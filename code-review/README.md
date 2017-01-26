# 代码复审

一份你和他人互相复审代码的指南。



## 对每个人

- 要承认许多代码上的决策体现了不同的观念。把你赞同的观念拿来与人讨论权衡之策，并快速得到解决方案。
- 提问，但不是提出需求。（比如“对这个变量`:user_id`命名，你有什么想法？”）
- 要求澄清。（“我不理解，你可以解释一下吗？”）
- ？避免在代码的归属问题上挑三拣四。（“我的代码”， “不是我的代码”，“你的代码”）
- 避免使用会被认为与个人特质有关的词汇（“愚蠢”、“傻”），要假定每一个人都是聪明且善意的。
- 措辞明确。要记住在线的人并不是总能理解你的意图。
- 措辞谦逊。（“我不是很确定，让我们查阅一下”）
- 不要使用夸张的用语。（“总是”，“从来没有”，“不断地”，“什么也没有”）
- 不用讽刺的词。
- 保持真诚。如果你不是emoji党、卡通图片爱好者，或者并不喜欢幽默，就不用逼自己去干那些事。如果你是，那就泰然自若的去做。
- 当出现很多“我并不太理解”，或者“替换方案：”这样的评论，就同步沟通（比如，与人交谈，同他人共享屏幕），并跟进贴上对讨论的总结评论。



## 你的代码被复审

- 对评审人的建议要怀抱感恩之意。（“好建议，我会做出修改”）
- 不要把评审这事当作是针对个人的。评审的是代码，不是你。
- 向别人解释此代码存在的原因。（“很有可能是因为这些原因。如果我重命名这个类/文件/方法/变量，会不会更清晰一些？”）
- Extract some changes and refactorings into future tickets/stories.
- Link to the code review from the ticket/story. ("Ready for review:
  https://github.com/organization/project/pull/1")
- 推送提交要基于上一轮的反馈，独立地将此次提交推送到分支上。不能等到分支即将被合并了，才把积压的改动做一次提交。评审人要能基于他们早期的反馈读到个人的更新。
- 试着去理解评审人的观点。
- 试着回复每一条评论。
- 直到在分支上的持续集成（TDDium，TravisCI 等等）的测试通过后显示绿色，再合并分支。
- 一旦你对代码和它对工程的影响很有信心，请合并。



## 评审别人的代码

搞明白为什么一定要做这样的修改（修复bug，改善用户体验，重构已有代码），然后：

- 把你对代码中感受强烈和不赞同的地方与对方交流。
- 指出使代码变得简洁的方法，这也会解决问题。
- 如果在线的讨论变得很哲学或学术，就将讨论安排到平时周五下午的技术讨论会上讨论。与此同时，要使代码作者在多种可替代实现的方案中，做出最终的决定。
- 提供可选的实现方案，但要假定作者已经考虑过这些方案。（“你觉得在这里用自定义验证控件如何？”）
- 试着去理解作者的观点。
- 在拉取申请上用:thumbsup:或“Ready to merge”的消息标示。



## 评论式样

评审人应该按照这个[style](../style)指南，评论式样：Reviewers should comment on missed [style](../style)
guidelines. Example comment:

```
[Style](../style):
```

```
> Order resourceful routes alphabetically by name.
```

一个回复评论的的式样：An example response to style comments:

```
Whoops. Good catch, thanks. Fixed in a4994ec.
```

如果你不并不同意某个指南，请在这份指南库上新开一个任务，而不是在在代码审核时争论此事。同时，请遵守指南。If you disagree with a guideline, open an issue on the guides repo rather than
debating it within the code review. In the meantime, apply the guideline.





===========以下为英文原文===========


Code Review
===========

A guide for reviewing code and having your code reviewed.


Everyone
--------

* Accept that many programming decisions are opinions. Discuss tradeoffs, which
  you prefer, and reach a resolution quickly.
* Ask questions; don't make demands. ("What do you think about naming this
  `:user_id`?")
* Ask for clarification. ("I didn't understand. Can you clarify?")
* Avoid selective ownership of code. ("mine", "not mine", "yours")
* Avoid using terms that could be seen as referring to personal traits. ("dumb",
  "stupid"). Assume everyone is intelligent and well-meaning.
* Be explicit. Remember people don't always understand your intentions online.
* Be humble. ("I'm not sure - let's look it up.")
* Don't use hyperbole. ("always", "never", "endlessly", "nothing")
* Don't use sarcasm.
* Keep it real. If emoji, animated gifs, or humor aren't you, don't force them.
  If they are, use them with aplomb.
* Talk synchronously (e.g. chat, screensharing, in person) if there are too many 
  "I didn't understand" or "Alternative solution:" comments. Post a follow-up 
  comment summarizing the discussion.



Having Your Code Reviewed
-------------------------

* Be grateful for the reviewer's suggestions. ("Good call. I'll make that
  change.")
* Don't take it personally. The review is of the code, not you.
* Explain why the code exists. ("It's like that because of these reasons. Would
  it be more clear if I rename this class/file/method/variable?")
* Extract some changes and refactorings into future tickets/stories.
* Link to the code review from the ticket/story. ("Ready for review:
  https://github.com/organization/project/pull/1")
* Push commits based on earlier rounds of feedback as isolated commits to the
  branch. Do not squash until the branch is ready to merge. Reviewers should be
  able to read individual updates based on their earlier feedback.
* Seek to understand the reviewer's perspective.
* Try to respond to every comment.
* Wait to merge the branch until Continuous Integration (TDDium, TravisCI, etc.)
  tells you the test suite is green in the branch.
* Merge once you feel confident in the code and its impact on the project.



Reviewing Code
--------------

Understand why the change is necessary (fixes a bug, improves the user
experience, refactors the existing code). Then:

* Communicate which ideas you feel strongly about and those you don't.
* Identify ways to simplify the code while still solving the problem.
* If discussions turn too philosophical or academic, move the discussion offline
  to a regular Friday afternoon technique discussion. In the meantime, let the
  author make the final decision on alternative implementations.
* Offer alternative implementations, but assume the author already considered
  them. ("What do you think about using a custom validator here?")
* Seek to understand the author's perspective.
* Sign off on the pull request with a :thumbsup: or "Ready to merge" comment.



Style Comments
--------------

Reviewers should comment on missed [style](../style)
guidelines. Example comment:

    [Style](../style):

    > Order resourceful routes alphabetically by name.

An example response to style comments:

    Whoops. Good catch, thanks. Fixed in a4994ec.

If you disagree with a guideline, open an issue on the guides repo rather than
debating it within the code review. In the meantime, apply the guideline.
