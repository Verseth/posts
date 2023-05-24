---
created_at: 2015-09-30 10:00:00 +0200
publish: true
author: Bartosz Krajka
tags: [ 'framework', 'emergency', 'fuckup', 'responsible rails' ]
newsletter: responsible_rails
img: "framework-for-handling-emergencies/header.jpg"
---

# Do you have a framework for dealing with emergencies?

<p>
  <figure>
    <img src="<%= src_fit("framework-for-handling-emergencies/header.jpg") %>" width="100%">
  </figure>
</p>

In my pre-Arkency life, I worked for a corp.

Keep in mind that it was my _very beginning_ of the commercial-IT world. I sometimes felt lost inside this huge DevOps machine. Lost and alone - as usually, when something breaks, there was nobody to ask for help.

It had to happen. One day, **by accident, I ran some kind of a DB-clean-up script, with the wrong date, on production**. The script's job was to delete records that are _old enough_. While fiddling with the script, I changed the definition of _records old enough_ to _records visible today (not closed)_. And accidentally ran the script.

<!-- more -->

My reaction - a complete paralysis. I wasn't even sure what the script exactly did. Unfortunately, I had never found time before to analyse the script line-by-line. Also, I have never seen unit tests, so most likely they didn't exist.

**So I had no idea how much damage I caused.** Maybe the script had a protection from so accidental usage as mine? It should have, right?

I took a look at the frontend to see if everything is ok - it wasn't. Literally no data for today. It was present a minute ago but disappeared. Cool. They will fire me.

One of the managers visited me quickly and brought to book: 

> Where are my data? I need it NOW for my work. When will you restore it?

I already realized that I had to call an ex-coworker of mine (he was currently on vacation, but I had no choice...) to get all needed information. So my answer was:

> I don't know. In the worst case scenario, when he is back, so next week [4 days].

**The end of the story was pretty lucky, though.** I immediately got an accurate instruction from my coworker-on-leave - there was a daily backup of the data, so _probably_ all we need is a casual restore from `/a_directory`.

A half-hour of preparation, with an extra pair of eyes and triple-checking if this time everything is ok - and voilà! The records are back! Only a few of them were lost forever (those inserted after the backup), but come on! You can insert them again! We saved the day!

## Sounds familiar?

Today, I'm not very proud of this situation. There were two good reactions, though:

* The paralysis that mastered me after my _fuckup_. When you don't really know how to fix things, it's better to do nothing than to do _anything_ with a panic and, most likely, only cause more damage. For instance, read [the story about Unix recovery in 1986](https://news.ycombinator.com/item?id=10160417) and the comments.
* Pair-fixing the issue.

What should I have done better?

Of course, easiest to say: _You should simply don't make such stupid things_, but I'm afarid it's impossible. We, in Arkency, are agreed - everyone makes mistakes. Smaller, larger, more or less foolish ones - but everyone does them. The professionalism doesn't mean _no mistakes_, but rather _as little mistakes as possible, asymptotically to zero; zero is unreachable_.

Speaking of the art of professionalism - **the true value of the developer is not how many mistakes they do, but how they recover.**

What is a shame today?

## Communication

**It was my duty to tell the managers what happened.** They shouldn't have needed to visit me and inquire about the accident. I should have told them immediately:

* What happened (_all the data is lost because of my inattention_)
* I'm working on it
* That's my top-prio
* However, I need help
* I'm sorry 

An apology is nice here (it was clearly my fault), but useless without all above.

If necessary, I should keep them informed about the progress. Luckily, the recovery was so quick that the next message was, at the same time, the last one: _Hey, it's ok now, we only lost today's records_.

**But staying quiet is never a good option.**

## Estimation

> I don't know. In the worst case scenario, when he is back, so next week [4 days].

I didn't lie. The problem is - such an information doesn't tell very much. At the end of the day, the incident was solved after less than an hour! Why were the numbers so different?

Much better approach is to estimate with 3 values:

* Optimistic - if literally everything goes without complications
* Expected
* Pessimistic - if literally everything goes wrong

So I should have told:

* Optimistically, half an hour.
* Realistically, 3 hours (_The ex-coworker is on vacation, why should he bother?_)
* Pessimistically, 4 days (_I can never catch him on the phone, never find a solution by myself, and solve it only after he comes back to work._)

All I did was telling only the third value. This was not the end of the world, if treated as an obligation (what the management often does). But the other two values were much less frightening, what should have been spoken.

Surprisingly, this time the most optimistic estimation was true. **This happens very rarely**, I consider this as a luck. Surely, next time my coworker won't break his sunbathe.

## A framework

That would be a dream. You have a list of things, step-by-step, what to do (and what not to do) in hard times. The list is very specific, but at the time it gives you some level of freedom in designated areas.

Handling emergencies is an individual matter. In depends very much on you as a programmer and as a developer, your habits, your team and your project. There are some hard rules here, like _be verbose_ or _give 3 values in estimations_. However, it's not exhaustive. There's always a whole lot of unknowns.

You should have your own _framework_ for such cases. Otherwise you are exposed to cases like this one above.

We described ours in _Responsible Rails_ book.

<section class="product product-responsible_rails">
  <div class="text-left">
    <a href="https://arkency.dpdcart.com/cart/add?product_id=116819&amp;method_id=123459" data-event-category="product" data-event-action="responsible_rails" data-event-label="after post">
      <h1 class="text-center my-8">Responsible Rails</h1>
    </a>
    <a href="https://products.arkency.com/responsible-rails">
      <img src="<%= src_original("responsible-rails/cover.png") %>" alt="" style="float: left; max-width: 220px; margin-right: 3em; margin-bottom: 1em">
    </a>
    <p>Tired of weekend works? Missing a framework for handling emergencies? Looking for advice how to mitigate the number of production accidents?</p>
    <p><strong>Try Responsible Rails!</strong> The book contains:</p>
    <ul class="list-inside">
      <li>over 100 pages</li>
      <li>anwsers to the most crucial questions about being a responsible Rails developer</li>
      <li>Arkency’s step-by-step execution on how to act when accident happens</li>
      <li>13 real-life stories writen by different authors</li>
      <li>discussion about the stories, from diverse points of view</li>
      <li>a portion of advice how to avoid emergencies as much as possible</li>
    </ul>
  </div>

  <div class="product-bottom text-center font-bold py-2">
  <a href="https://arkency.dpdcart.com/cart/add?product_id=116819&amp;method_id=123459">Grab your copy!</a>
  </div>
</section>

## Summary

Sometimes you can observe that you change the habits during emergency - maybe it's time to reconsider your beliefs?

* If you usually TDD, but drop it in emergency times - you don't really trust TDD.

* If you need a partner to fix an issue - well, maybe _pair programming_ is a good idea also in the quiet times?

* If you turn the music off to gain the maximum mana - why don't you do it all the time?

* If an acident shows that you can delay all the needless things (casual talks, facebook checkings, etc.) - take a look at [Pomodoro Technique](https://en.wikipedia.org/wiki/Pomodoro_Technique).
