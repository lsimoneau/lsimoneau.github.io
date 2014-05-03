---
layout: post
title: "TDD Saved my Life (or at Least my Job)"
date: 2014-05-03 09:01:34 +0800
comments: true
categories: TDD
---

There's been more than a little attention devoted lately to the value of TDD in software development, driven in large part by a [few](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html) [critical](http://david.heinemeierhansson.com/2014/test-induced-design-damage.html) [blog posts](http://david.heinemeierhansson.com/2014/slow-database-test-fallacy.html) by DHH.

I disagree with most of what's said in those posts, but that's not why I'm writing this. Others have already responded to the details of his arguments, for example the purported sacrifice of code clarity for testability, far better than I could. In particular, I can highly recommend [Uncle Bob's](http://blog.8thlight.com/uncle-bob/2014/05/01/Design-Damage.html) and [Gary Bernhardt's](https://www.destroyallsoftware.com/blog/2014/tdd-straw-men-and-rhetoric) responses.

But my personal experiences with TDD have highlighted a benefit of the technique that hasn't received much attention in the last few weeks' discussion, so I thought it worthwhile to share some of my experiences.

TDD and Working Memory
----------------------

I'll begin with a little bit of background: I have a 14-month-old son who has yet to sleep through the night. This means that, with only a handfull of exceptions made possible by heroic efforts on the part of my partner, I haven't had a full night's sleep in over a year.

The effects of chronic sleep deprivation are myriad, but most notable to me in my day-to-day life as a software developer is that my working memory is shot to shit.

While I don't really have much more difficulty remembering facts, or marshalling my skills and experience to solve a problem, my ability to hold information relevant to the task at hand in my mind while I work is greatly reduced. This makes it quite a bit more difficult to implement any remotely non-trivial software system, since a big part of that process is maintaining at least a partial representation of the whole system in your mind as you work through building out and composing its individual parts.

This is where TDD comes in. As Katrina Owen astutely points out in her excellent talk [Therapeutic Refactoring](http://www.youtube.com/watch?v=J4dlF0kcThQ), _refactoring with tests makes you smarter_:

> You offload a bunch of those little details, that under normal circumstances go into working memory, into your tests. Once you start refactoring, you start reclaiming your brain.

TDD greatly reduces the demands that developing software places on your working memory, since you're in effect planting signposts for yourself as you go along, each one showing you the next step you need to take.

Perfectly Spherical Programmers in a Vacuum
-------------------------------------------

I get it. You're smart. You're a good, nay a great programmer. On a good day, when your brain is firing on all cylinders, you can easily compose a complex subsystem without writing anything down, holding the whole thing in working memory as you effortlessly hop between collaborators, building out their integrations and implementations. Me too.

But even you, and definitely I, have lots of days that aren't so good. Sleep deprivation is only one of many possible reasons you might suffer from a temporary deficit of working memory. You could be distracted and worrying about a situation in your home or work life that has no bearing on the task at hand. You could be uncomfortable, too hot, too cold, hungry, sick, thirsty, upset, overfull, or god forbid hungover.

And in those situations, on those days, benefitting from what Katrina cleverly calls an _exobrain_ is a life saver. You have a series of small, fast tests that have led you down the path of implementing your system right to the point you're at right now, in this second, deciding whether you need to write `>` or `<` in this conditional. So if you lose your train of thought or something slips your mind because you're not quite 100% today, don't worry. "This is still red. Why is this red? Oh, I need to _X_." And you're right back at it.

It's no hyperbole for me to say that there's no way I could have kept myself employed over the past year without the help of this exobrain.

I truly believe that TDD helps you write better, less tightly coupled software. I believe that it protects you from bugs and regressions, and that it can be a huge help in refactoring your code to be more expressive and readable. But for me, this past year, I would still have practiced TDD even if none of those things were true. Because TDD is a reliable methodology for producing code that does what it's supposed to do. Because if you have sufficient knowledge of the language you're working in, the patterns of software design, and the domain of your application, you can apply that process and make good software even on the kind of day where you have a hard time making breakfast.

And on days when you _are_ your very best, you still win. That extra working memory you now have available can be used to think about the high-level design of your system, the expresiveness of the method you're writing, etcetera. Because you've grown accustomed to not trying to remember everything about the class you're working on and its collaborators, because _that information is in your tests_, you're free to use that extra mental RAM on more rewarding efforts.

Keep your Skills Sharp for When your Brain Isn't
------------------------------------------------

Based on my experience this year, I now see this as one of the key benefits of TDD: it allows you to keep operating at a very nearly optimal performance level under much less than optimal circumstances. It's like a redundant backup system for your ability to do your job.

Even if you don't plan on having children, (or if you're one of those horrible people whose children sleep at night), there's a good chance you'll hit a point in your life when you're not your best, sometimes for weeks or months on end. Even if you've tried TDD and thought it wasn't for you, even if you feel like you "don't need it", I'd urge you to get a bit more practice with it, because it just might come in handy someday if your brain lets you down.
