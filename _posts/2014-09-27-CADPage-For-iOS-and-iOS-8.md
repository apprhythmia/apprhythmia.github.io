---
layout: post
title:  CADPage For iOS and iOS 8
date:   2014-09-27
categories:
---

<div class="h4" align="center" style="color:green">Status: Available In The App Store</div>
<div class="h4" align="center">2014-09-29 15:48:00 -0500</div>


As I'm sure you're all well aware, Apple recently pushed out a shiny new update
to its mobile operating system, iOS. If you've already upgraded to iOS 8, and you're
reading this blog, you're probably also well aware that it broke CADPage For iOS,
specifically that, when you try and tap on the `cell` (or row in the list) for a
page to see the details, it does _absolutely nothing_.

I've submitted an update with a fix, and was granted an "expedited review", so
hopefully it will make it through the review process quickly, and appear in the
App Store soon. If you're curious what happened, read on, and I'll do my best to
explain it.

### What Happened

Somewhere in the switch from iOS 7.1 to iOS 8, the way I was displaying new screens
from a `cell` quit working. In the end, it actually only took a _single_ line of
code to fix. Transitions from one view to another in iOS are often handled by
something called a `segue` (pronounced like "segway"...that took me a while) and,
prior to iOS 8, creating a segue was as easy as a right-click (or cmd-click) on
the `button`, `cell`, or `view` you wanted to trigger the transition, and then
dragging a line to the view you wanted to transition to. Give that `segue` a unique
name, and you're done, all done without actually writing a single line of code.
It seems that now, when it comes to presenting a new view from a `segue` presented
by a `cell` in iOS 8, in addition to creating the `segue`, you now have to explicitly
tell the `tableview` to trigger it by calling its `tableView:didSelectRowAtIndexPath:`
delegate method.

For me, the most confusing part is that I cannot find any documentation explicitly
stating you cannot use a "drag and drop" style `segue` from a `cell`. Since `cell`
objects inherit from `UIView`, where this is possible, I just assumed it was an ok
thing to do.

The good thing is, the problem has been fixed, and the app is waiting to be reviewed.
I'll post a status update at the top if anything changes.
