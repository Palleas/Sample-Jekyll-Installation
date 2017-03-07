---
layout: post
title: Make your testers' lives easy and handle errors
---
Let me know if this sounds familiar: your QA engineer just installed a fresh version of your application. 2 minutes in, you already got your first issue reported: they’re stuck because nothing happens when they press the login button. They're literally stuck at the entrance of your app and they can't do their work until you send them another build fixing this issue. That also means stopping whatever you were doing.

One of the main advantages of continuous integration and automated deployment is the quick feedback you get from your testers, your boss, your QA engineer… Although, even with the best continuous integration suite, one simple thing to always think about is **error handling**.

# Error handling is not optional

This may seem like something obvious to you, but a quick search on Github will show you that’s it not the case for everyone.

{% figure /assets/2017/02/error-handling-01.png|5444 places where errors will be handled at some point %}

{% figure /assets/2017/02/error-handling-02.png|23367 places where errors handling is **definitely** for this sprint! %}

It’s quite easy to be focused on the result when you’re working on an application. You got that list of tweets showing up in your app, you can move on to the next feature. You’ll keep a day or two in your sprint to tackle all this boring error handling. When Swift 2.0 introduced `guard` and started advocating for the “exit early” approach, we’ve probably all written the following code.

{% embed error-handling-01.swift|swift %}

Problem is, we often forget that even if something is not supposed to happen, **it probably will**. QA engineers usually have this effect on apps.

# It’s ok to crash
The problem with a simple `return` is that nothing will happen and there is nothing more frustrating than that for the user. As an alternative, why not just crash the app? First, the tester will know that something definitely went wrong and won’t keep pressing that button hoping for something to happen. Even better, when using a crash reporting tool, you’ll know everything about the crash and won’t have to squeeze the details out of your tester.

{% figure /assets/2017/02/error-handling-05.png|Crash reporting in Buddybuild %}

Most of the Crash reporting tools, such as [Buddybuild](https://www.buddybuild.com/) have a `crash` method you can use, but a `fatalError` will work just as well. 

{% embed error-handling-02.swift|swift %}

# Make sure you handle all the cases
There is one more pattern I want to talk about. It was common in Objective-C but even then it wasn’t a great solution.

{% embed error-handling-03.swift|swift %}
 
The problem with this kind of code is, once again, that a situation that should never happen probably will. When in the hands of the users, nothing will happen when the data **and** the error are both nil. Rob Nappier gave [one of my favorite talks](https://realm.io/news/tryswift-rob-napier-swift-legacy-functional-programming/) covering this pattern, why you should avoid it and what to use instead (TL;DW: use a [Result enum](http://github.com/antitypical/Result)).

# Worst case scenario: log the error
If crashing is not a option for you, at least make sure to log has much information as you can. It will help you when you need to investigate. [Castro](https://appsto.re/ca/XafBab.i), my favorite podcast application, is a great example. When you want to send them some feedback, logs can be automatically attached to your message. That way, if you come to them with a bug, they should be able to retrace your steps in the apps quite easily and understand what happened.

Help testers help you: make sure they are able to send you **valuable feedback**.

{% include ck_big_form.html %}
