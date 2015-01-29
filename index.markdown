---
layout: index
title: JUniversal
permalink: /
---

### JUniversal makes Java truly cross platform ###

Writing the same code from scratch for every platform is a pain & inefficient.   There must be a better way.
JUniversal lets you write code in Java (like you probably already do if you develop for Android) and take that code to places you never thought it could go.
It's primarily targetted to sharing code across mobile apps, but the technology can extend to non-mobile scenarios as well.

There are two core pieces of technology behind the JUniversal approach to cross platform:  source code translators (sometimes called transpilers) and
 a set of cross platform Java programmer friendly libraries, called JSimple.

### Here's how it works

1. Pick the code that you want to share cross platform.  It can be a little bit of code (maybe some hairy algorithm) or a lot (everything in your app except the UI).   Develop that in Java, with your favorite Java IDE.  To keep things tidy, you probably want to separate out shared code to a separate modules/JARs.  Ideally, create JUnit tests for it too.

2. In your build script invoke JUniversal to do <i>source code translation</i>, converting <b>Java</b> to <b>C#</b> (for Windows/Windows Phone) or, coming soon, <b>C++/Objective C++</b> (for iOS or performance critical code on Android NDK/Windows).   You can also use Google's j2objc converter, to produce <b>Objective-C</b>.<br>
JUniversal translated source is very human friendly, preserving comments and formatting.   It's intended to look almost like a human being wrote it manually.   That's handy when debugging and has other advantages.   Unit tests are translated as well.

3. Source translation is half the technology here, handling the Java language proper.   The other half is the [JSimple libraries](/jsimpledoc), providing core functionality needed by most mobile apps (OAuth, JSON, network & file I/O, logging, unit testing, etc), in a way that's familiar to Java developers but also cross platform friendly.<br>
For a typical mobile app, the intention is those libraries should provide enough support to share core app logic, server communication, and persistent client state cross platform, with just the UI native.<br>
Check out the [JSimple Javadoc](/jsimpledoc) for details.

4. For non-shared parts of your code, write those in the native platform language (C#, Swift/Objective-C, etc.).   JUniversal doesn't provide any support for UI today&mdash;the intention is that you write it natively.   That gives the best UX anyway.



From a developer experience point of view, you'd probably spend most of your time in your favorite Java IDE, getting shared code written & debugged there, plus doing Android specific dev.   Then switch to Xcode or Visual Studio for a smaller percentage of your time, to write & debug the iOS and Windows device specific stuff.

### Current status ###

JUniversal still has several rough edges, but it should work well enough to try out.  And we need you to try it out and tell us what the issues are so we can fix them.
The Java -> C# translator is basically feature complete.   Expect some translation issues, but they almost always cause C# build errors, and the course of action then is to either
tell us (so we can tweak the translator) or tweak your Java source code to avoid.   Also expect some issues with incomplete doc, etc.

When you have issues/questions, reach out to us through any of the mechanisms on the Contact page (issue tracker, email, even chat).
For initial JUniversal users, we want to provide very hands on support, both because you'll probably need it and because that's the way for us to get the best feedback.

Is JUniversal stable enough today to ship production apps based on it?   In many cases yes, assuming you get it working for your needs,
but please reach out before shipping so we can talk through any caveats.

We're also looking for feedback on the JSimple API.   Would you do anything different there in terms of API design, consistent naming,
Java collection class enhancements, etc.?   In some ways it's an opportunity to improve (at least modestly) on core Java APIs.   If that excites you, input is welcome.
Obviously, JSimple bug and feature requests are welcome too.

### Why source translation?  ###

Wouldn't native compilation be better or maybe a VM?  Actually, source translation offers several advantages:

* <b>Easily mix and match shared code with native</b><br>  It's easy to call from hand written C#/C++/Objective-C source to Java translated C#/C++/Objective-C source and vice versa.   It all looks the same to the compiler.  There are no language impedence mismatch issues, no glue layers to write.

* <b>Native IDE friendly</b><br>
 Develop & debug your Android app in Android Studio / Eclipse, your iOS app in XCode, and Windows app in Visual Studio.   It's easy to step through translated source alongside native code--it all looks the same to the debugger.   Stack traces look natural, both in the debugger and any logged exceptions, even with a mix of translated and native source.

 Javadoc being translated to C# doc comments means that method doc popups show up as nicely in Visual Studio as they did in Android Studio / Eclise for the same code.

* <b>Lightweight, with no framework comittment needed</b><br>
You can choose to start small, maybe just sharing code for a single particularly tricky component you're sure you don't want three separate devs each writing from scratch, in three different languages.   With some other technologies, that's hard because sharing any code requires bringing along lots of supporting framework&ndash;it's not the kind of thing you'd normally do unless you'd want to commit to using that technology for the bulk of your app.

But with source translation, 1000 lines of Java code (assuming no library dependencies) maps pretty much to 1000 lines of C# code.   And that code looks, pretty much, like hand written C# code, so it's not something a C# developer would be grumpy about using.

* <b>Transparency</b><br>

It's easy to see what JUniversal does--just look at the translated code.   In most cases, you'll see a very straightforward translation.   That helps you have confidence about correctness & performance characteristics.   No black boxes.

### Why Java? ###

Simply put, I wanted a cross platform language & toolset that would be comfortable to typical mobile devs who today build for Android in Java and iOS in
Objective-C or Swift.   That meant supporting something they already use.  And of those three languages, Java is arguably the best choice for cross platform.   Whether you develop on a Mac, Windows, or Linux, there are nice IDE choices.   If you use Java on the server too, all the better.

It may not be the hottest new language in town, though the JSimple version of the Java collection classes actually do allow you use Java 8 lambdas a fair amount if you want (even on Android), giving Java a more modern, functional feel.

<!--
### Comparison to other x platform technologies ###

There are several technologies out there supporting cross platform mobile apps, each with their pros and ons.

If you're a web whiz who loves Javascript, and want to build mobile apps that way, take a look at Apache Cordova.  If you love C# (and it's nice), then check out Xamarin.  If you think it'd be really cool to write for mobile in Clojure, RoboVM can help with that.

But if you'd rather stick with the building for Android, iOS, and Windows with their "normal" platform tools, but want a way to share code (and who wouldn't if it was easy), then check out JUniversal.  Among the cross platform technologies out there, JUniversal is arguably among the most
native-like, as it produces (through translation) code in the native programming language, built/debugged/packaged with native tools.
In some ways it's like porting an Android Java app to other platforms but having a computer do the porting rather than a human being.
-->

