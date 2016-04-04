---
layout: post
title:  "Hello Swift!"
date:   2016-04-03 16:47:16 -0400
category: swift
---
Swift has been around for a couple of years. Swift is a fast, safe, expressive
and open source. It is extensively used for creating iOS, watchOS, tvOS and Mac
applications. Further, there are also server-side frameworks been developed. Long
story short "Swift is everywhere". So it is worth and fun to get your hands
dirty. In this post I would like to share some links that is worth to take a look.

Swift Basics
---
To get started with Swift. I will highly recommend reading “The Swift Programming
Language” book available on [swift.org][swift-documentation]. The reason for
referring this book is that; it is the official book, and it is up to date with
the new version of Swift. I started from chapter 1 and while I was reading and
understanding each concept. I have also solved the experiment that came in-between.
It is fun to use the playground for solving this experiments. Further, if I were
feeling bit ambiguous then, I would just go to
[tutorialspoint.com/swift][tutorials-point], and if still unclear I will just
google it.

I was a bit confused about Optionals, `?`, `!` and `if let`. I found this
[article][optionals] much helpful and informative. It cleared much of my doubts.

I also came across a post by [Natasha Murashev][natasha-murashev] where she
explained faces of functions in Swift. Again, I did all this in the playground
and recommend you too. `DO NOT COPY PASTE CODE! TYPE & TYPE`

[FuckingSwiftBlockSyntax.com][fucking-swift-block-syntax] also shows the
combination of function and closure in swift. Function and closure are one of
the important concepts in Swift. So I spend little more time with this guys.

iOS with Swift
---
Once you are feeling a bit confident about Swift, I am sure you would be much
excited to give a try to iOS application development with Swift. But wait! Have
you prepared your environment, do you know which style guide to use, is there
any documentation plugin. **Note:** [Alcatraz][alcatraz] is required for XAlign,
Swimat, and VVDocumenter.

1. Style Guide `Must follow`

    Following a styling guide is necessary. Prolific Interactive maintains a
    repository ["swift-style-guide"][swift-style-guide] which is actively updated.
    I  prefer this guide because I know the dedication for improvement and quality
    of their work they have.

2. SwiftLint `Must have`

    So now we have started following style guide but, sometimes we can make
    mistakes. [SwiftLint][swift-lint] is a tool that will raise error and
    warning if we go off the track while we are busy coding.

3. XAlign OR Swimat `Must have`

    Many of folks might me knowing “XAlign” [XAlign][xalign] is used for aligning
    code or make code look beautiful. XAlign has good supports Objective-C but,
    has less support for Swift. [Swimat][swimat] is similar to XAlign. Swimat is
    new and frequently updated. At the same time, XAlign looks less promising for
    Swift.

4. VVDocumenter-Xcode `Must have`

    [VVDocumenter-Xcode][VVDocumenter-xcode] is a vital tool for documenting code.
    It has started supporting swift.

5. R.swift `Depends on requirements`

    [R.Swift][r.swift] is a valuable pod for handling static resources in the
    applications. It is much similar to `R.java` in Android. This tool has bit
    issues, but it looks promising for future use.


Conclusion
---
There are a lot of other tools and useful resources which I may missed. I am not
reviewing each tool here. I am just sharing my experience about Swift and iOS.
Any comments or thoughts are welcomed [here][git-repo].



[swift-documentation]:https://swift.org/documentation/
[tutorials-point]:http://www.tutorialspoint.com/swift/
[natasha-murashev]:https://twitter.com/NatashaTheRobot
[optionals]:http://www.touch-code-magazine.com/swift-optionals-use-let/
[fucking-swift-block-syntax]: http://fuckingswiftblocksyntax.com
[alcatraz]:http://alcatraz.io
[swift-style-guide]:https://github.com/prolificinteractive/swift-style-guide
[swift-lint]:https://github.com/prolificinteractive/SwiftLint
[xalign]:https://github.com/qfish/XAlign
[swimat]:https://github.com/Jintin/Swimat
[VVDocumenter-xcode]:https://github.com/onevcat/VVDocumenter-Xcode
[r.swift]:https://github.com/mac-cain13/R.swift
[git-repo]:https://github.com/ayushcshah/ayushcshah.github.io/issues
