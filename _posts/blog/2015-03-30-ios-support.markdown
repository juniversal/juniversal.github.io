---
layout: post
title: "iOS support"
date: 2015-03-30 02:01:00
author: Bret
categories:
- blog
img: post03.jpg
thumb: thumb01.jpg
---

I was intending to post more often, but you know how those things sometimes go.   One reason for the delay is that we've been spending
a lot of time with the launch of our two Garage apps:   [Join Conference](http://www.windowsphone.com/en-us/store/app/join-conference/0264f9a0-689a-40d9-86ad-b20b70ebaad8) and [SquadWatch](http://www.windowsphone.com/en-us/store/app/squadwatch/2ba351b4-3dd8-4d36-823a-45c015a2acfa).   Check them out.

Anyway, I've been continuing to work on iOS support, via Java -> Objective C++, conversion.   And folks sometimes about the rationale for that, as opposed to say Swift conversion.   So I wanted to write up the evolution of my thinking, below:

1. At first (like a  year ago) I was all keyed up to use the Google j2objc translator.   Then a couple things happened:
  * I used it and wasn’t that crazy about the output code not being all that human friendly—that’s not a huge deal, but it goes against the JUniversal philosophy I trumpet of the output code being very human friendly / nearly idiomatic.
  * More importantly, Swift got announced last summer, so it was clear then that Objective C would eventually fade away, though it may take years.
2.	So then I was ready to implement a Java -> Swift translator.   And I actually started that & did a fair amount of implementation there.   It’s in the JUniversal code base now & I may end going back to it & finishing.   But one key issue there is that Swift doesn’t support exceptions.   There’s enough push back around that in the community (as Objective C code can throw exceptions but Swift can’t catch them) that I think there’s a good bet that Apple will add exception support in the next year or so.   But they haven’t yet & I didn’t really want to map Java exceptions to error returns, especially if it’s just a temporary thing.   So I want to see how it all settles out with Swift.   Plus, at least last year, Swift clearly had some more maturing to do—it was kind of buggy & had performance issues.   It’s better now, I think, but not perfect.
3.	So then I thought, why not just do C++.   That’s well supported in XCode (even more so than all other platforms, because ObjC and Swift are natively compiled by the same tool chain).   And most importantly it’s a standard, so it can be used in several other scenarios too beyond iOS (like Windows or Android for performance critical code and even other platforms like running on Raspberry Pi).   So there’s a lot of bang for the buck there, with C++.   From programming language syntax perspective, C++ is (arguably) about as friendly to Java developers as Swift is to them, at least when they just have to look at the code & not author.   It’s certainly better than Objective C at least.   And I had done a lot of work on a Java -> C++ translator earlier, so I went back to finish that. 
