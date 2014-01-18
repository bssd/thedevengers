*By Andy Stewart*

In the last two posts on TDD we had a look at [what it is](http://thedevengers.com/what-is-tdd/) and [some of the benefits and drawbacks](http://thedevengers.com/tdd-the-benefits-and-drawbacks/). Assuming you have read those and still interested in giving TDD a go then you are probably wondering what the best way to get started is.

At this point it seems appropriate to include a little personal history. I started my development career in 2000 and for the first 7 years didn't concern myself with automated tests all that much. The projects that I worked on were very traditional heavyweight waterfall style affairs where code would be literally thrown over the wall for the test team to wrestle with.

That all changed in 2007 when I joined a startup who had the [Extreme Programming](http://en.wikipedia.org/wiki/Extreme_programming) (XP)  methodology at the heart of their development process. This included automated unit and integration testing. In hindsight very little of it was TDD style but it was a start. 

It was here that I learnt some of the techniques around what makes a good or bad test. It was also here that I first encountered Mock objects. Initially I thought Mocks were a good way to create lots of brittle tests. It turns out that I simply wasn't using them correctly. Mocks are something we will come back to later in this series as over time I have learnt how to use them properly and find them invaluable when practising TDD.

Later I read the book [GOOS](http://www.amazon.co.uk/Growing-Object-Oriented-Software-Guided-Signature/dp/0321503627/ref=sr_1_1?ie=UTF8&qid=1389798078&sr=8-1&keywords=GOOS) and that was a bit of an epiphany for me. It highlighted areas where I could improve what I was doing and produce more maintainable tests and thus production code. From that point on I started adding more and more of the techniques into my daily coding and found the quality of delivered software improving. I reviewed GOOS back on my [company blog](http://www.bssd.co.uk/blog/?p=242).

I am guessing that you don't want to take the best part of 14 years to feel comfortable doing TDD though? So my advice to getting started is this:

1. Go and read some books on TDD. Pick up a copy of GOOS or [Kent Becks TDD by Example](http://www.amazon.co.uk/Driven-Development-Addison-Wesley-Signature-Series/dp/0321146530/ref=sr_1_1?ie=UTF8&qid=1389798405&sr=8-1&keywords=TDD+by+example).
1. Whilst I think Code Katas have a bad name (see [this blog](http://simpleprogrammer.com/2013/08/26/dont-code-katas/) for reasons why) they can be very useful for trying new things out. Pick some simple problems and try and TDD them through. In Kent Becks book he works through an example of building an xUnit framework using TDD. He says anytime he learns a new language he uses that to explore the language features.
1. Start applying it on real projects. This really is the best way to learn as you will find what things cause you pain and what things help ease the development burden.
1. Carry on following this series as we are about to dive into concrete techniques and examples in the next post.

