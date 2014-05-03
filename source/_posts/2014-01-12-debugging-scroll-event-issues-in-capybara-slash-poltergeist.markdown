---
layout: post
title: "Debugging Scroll Event Issues in Capybara/Poltergeist"
date: 2014-01-12 08:22
comments: true
categories: rails TDD capybara
---

I came across a particularly tricky issue with one of my integration tests using [capybara](https://github.com/jnicklas/capybara) and [poltergeist](https://github.com/jonleighton/poltergeist) this week, so I thought I'd write it up for the benefit of anyone else encountering a similar problem.

The Issue
---------

A key step in the workflow under test was clicking on a "Publish" button located at the bottom of the page. This test had been passing for some time, but started failing when we added a floating toolbar to the site.

The toolbar was of the type that appears as part of the document layout until the user scrolls past it, at which point it attaches itself to the top of the viewport.

The original code for the toolbar looked something like this:

``` coffeescript
$(document).ready ->
  toolbar = $('.toolbar')
  header  = $('.header')
  start   = toolbar.offset().top

  $.event.add(window, "scroll", ->
    pos = parseInt $(window).scrollTop()
    toolbar.css("position", if pos > start then "fixed" else "static")
    header.css("padding-bottom", if pos > start then toolbar.height() else 0)
  )
```

When scrolling past the toolbar, the `position` attribute is switched from `static` to `fixed`. Because this yanks the content in the toolbar out of the document flow, it causes the rest of the page to jump upwards to fill the now-empty space, so we apply padding to the header equal to the toolbar's height to compensate.

With this code in place, the test started failing. Some quick debugging showed that the click on the publish button just wasn't happening (no `click` events were fired). Usually, if Capybara can't find the element you've told it to click on, it will throw an error, but in this case it was just failing silently. Remove the fixed toolbar, test goes green again.

The Solution
------------

The key insight towards solving this issue came when I added debug code to the `scroll` event handler shown above. The page was actually scrolling during the execution of the test.

[Poltergeist's documentation](https://github.com/jonleighton/poltergeist#mouseeventfailed-errors) explains why:

>When Poltergeist clicks on an element, rather than generating a DOM click event, it actually generates a "proper" click. This is much closer to what happens when a real user clicks on the page - but it means that Poltergeist must scroll the page to where the element is, and work out the correct co-ordinates to click. If the element is covered up by another element, the click will fail (this is a good thing - because your user won't be able to click a covered up element either).

The problem arises because of the way the fixed toolbar is implemented: _first_ it pulls the toolbar out of the document flow, _then_ it compensates with padding.

This results in two page redraws, and an imperceptibly fast jitter in the position of elements on the page. But since poltergeist is operating much faster than a human with a mouse, the time delay between scrolling the page and clicking the button (or, more accurately, clicking the coordinates where it expects the button to be) is negligible. As a result, it clicks while the page is jumping and misses the button.

There are a number of possible fixes here. It's possible to tell Poltergeist to just trigger a click event on the element rather than scrolling to it and clicking on its coordinates:

``` ruby
find_button("Save").trigger('click')
```

Instead of:

```
click_button "Save"
```

This solves the problem, but does away with one of the main benefits of using Poltergeist in the first place: it directly emulates the behavior of a user navigation the site. If an element is covered up by something else, Poltergeist will be unable to click on it, just like a user.

The solution I ended up with was to change the implementation: rather than apply CSS to two different elements in quick succession, I apply a class to the page body. Because the padding is variable and needs to be toggled on and off in CSS only, I moved it to a shim element that's hidden by default. The body class both applies `fixed` position _and_ unhides the shim:

``` coffeescript
shim    = $('.scroll-shim')
toolbar = $('.toolbar')
start   = toolbar.offset().top

$.event.add(window, "scroll", ->
  pos = parseInt $(window).scrollTop()
  shim.css("padding-bottom", toolbar.height())

  $('body').toggleClass('scrolling-header', pos > start)
)
```

And in CSS:

```
.scroll-shim {
  display: none;
}

.toolbar {
  position: static;
}

.scrolling-header {
  .scroll-shim {
    display: block;
  }

  .toolbar {
    position: fixed;
  }
}
```

Because it only redraws the page once, this version doesn't cause content to jump around and allows Poltergeist to click on the button without issue. Although there's no perceptible difference when viewing the page, it _is_ actually requiring the browser to do a bit less work, so it's even hypothetically possible that on slow hardware a user might see the benefit as well.

Summary
-------

When using Poltergeist in combination with any JavaScript code that triggers off of `scroll` events, it's important to keep in mind that Poltergeist _will_ scroll the page when required to get to an element you've asked it to interact with. This isn't a bug, it's a feature: it allows for a closer emulation of a user interacting with your site, and might enable you to pick up issues that other drivers wouldn't surface.

However, if you're modifying the page in response to scroll events, you run the risk of introducing subtle race conditions like the one I've described. In this particular case, there was a solution that improved both the performance of the page _and_ the test behavior.
