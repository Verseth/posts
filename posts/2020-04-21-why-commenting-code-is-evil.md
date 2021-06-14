---
created_at: 2020-04-21T11:52:48.259Z
author: Tomasz Wróbel
tags: []
publish: false
---

# Why commenting code is evil

Less inflammatory title: hidden costs of putting adding comments to code

<div class="tumblr-post" data-href="https://embed.tumblr.com/embed/post/9NYQOutKOEXi4aopdzCr9A/143319675260" data-did="a3fbf2de0fdc7813870b144667c226566dd2e2ac"><a href="https://classicprogrammerpaintings.com/post/143319675260/experienced-engineer-examines-comments-in-a-legacy">https://classicprogrammerpaintings.com/post/143319675260/experienced-engineer-examines-comments-in-a-legacy</a></div>
<script async src="https://assets.tumblr.com/post.js"></script>

I bet you've seen such codebases. Comments all over the place. You've got no idea which ones are still relevant. Most of them hard to understand or too obvious. Big chunks of code commented out. _Saved for later_. _Disabled for now_.

Random reasons why commenting code is bad for you and what can be done instead:

### Attention cost

You see one and you wonder - is it still relevant? Should I care about it? Should I stop now, focus and try to understand what the commenter meant? Good bye deep work. It leaves you with worries and guilt. It's like dead code, but worse - you cannot easily establish if it's still relevant.

I've talked with people outraged at this position. _What if there's a pitfall in that piece of code? Wouldn't you rather wanna know about that?_ If there really is a pitfall, there are so many ways you can do better than a code comment. Actually, why would you leave a pitfall in the code in the first place? Why not fix it instead? Putting a comment makes you feel justified - for a wrong reason, because you haven't improved anything. If it needs fixing, but you cannot do it at the moment - create a ticket for it. 

Do you wanna make sure that an assumption is valid? Write a test for it.

TODO: link that post with tests failing when a library gets the bugfix.

### Makes you feel better when you shouldn't

### Comments invite more comments

### Comments are ugly

<!-- Singapore and chewing gum. -->

## What can be done instead

### Self explanatory code, obviously

Name a method in a specific way, even if it's very weird, e.g. dangerouslySetInnerHtml instead of `// warning: dangerous`

### Make a test case

### raise an exception


## Caveats

* The rule may not be 100% but still you're far better off if you stick to it 100%, because you never wonder "should I write a comment".
* Ok, you work on a public api, library - documentation.

Got better reasons for commenting code?


TODO: "In fact, in 35% of the catastrophic failures, the faults in the error handling code fall into three trivial patterns/: (...) (iii) the error handler contains expressions like “FIXME” or “TODO” in the comments."
https://www.usenix.org/conference/osdi14/technical-sessions/presentation/yuan
https://arkency.slack.com/archives/C0E40RKUG/p1583089578000800

https://arkency.slack.com/archives/CF4S8LZFV/p1588079546005000

because they're ugly

https://arkency.slack.com/archives/C1LNH6UR1/p1588163647003600

to justify some bad code
to track an issue instead of a tracker
to not fix something you saw

it invites more comments


