---
layout: post
title: "What’s the worst mistake I’m empowered to make?"
date: 2013-10-25 21:39
comments: true
published: true
categories: work
---

A few days ago a coworker of mine ordered a glass of beer in a pub while waiting for some pizzas to bring back to the office. After taking a big gulp, he immediately felt a burning sensation in his mouth and throat, and knew something was wrong. It turned out that the pub had recently cleaned the beer tap lines with a potent cleaning fluid, and it hadn't all been flushed out. The result was that my friend spent a night in the hospital, and was unable to eat anything for over a day due to the swelling and pain in his throat. It's still not totally clear what his path to recovery will be like.

When I told the story to another friend, his reaction was "Wow. It's almost like, as a barman, what's the worst mistake I'm empowered to make?" And it's true. Quite literally, there's not a thing you can do in the execution of a bartender's job that is worse than serving a customer highly caustic cleaning fluid in their drink.

And that got me thinking. What's the worst mistake _I'm_ empowered to make? Take the site down? Nope, pretty easy to have it back up in a few minutes, little harm done. Commit a bug to production? It would have to be a whopper. Really, the worst thing I think I'm empowered to do is lose data. If I break something such that, even if we recover, we've lost some user data, that's deadly. I can't think of any other single misstep that would be more damaging to the company.

As soon as I came up with that answer, I realized I actually spend quite a lot of time thinking about it. Ever since the first time I shelled in to a production server, and not infrequently since, this little voice at the back of my head asks: _Are you about to hose any data?_ Every time I'm running complex migrations, or implement a new feature that alters data in a major why, it's always there: _Don't do the wost thing you could possibly do._

Let me be clear: avoiding catastrophic fuckups isn't what I consider the hallmark of a great software developer, or a great anything for that matter. To be _great_, you need to succeed greatly, not simply avoid failure. But coming back to the example of the pub: this was a _really good_ pub. Fantastic selection of craft beers, best pizzas in the area by a wide margin, and a very welcoming atmosphere. But I still won't be going back there, because, well, obviously: they poisonned a friend of mine.

So it's probably worth spending a little bit of time, every once in awhile, asking yourself what the worst possible mistake you're empowered to make is. And putting in place whatever safeguards seem reasonable to stop yourself from making it.
