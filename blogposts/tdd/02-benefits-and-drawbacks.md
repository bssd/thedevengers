*By Andy Stewart*

My [post on TDD](http://thedevengers.com/what-is-tdd/) stirred up some lively debate with my fellow devengers. The main question was did I really insist on tests being written first to consider the process TDD? The answer is yes I do. It is worth re-iterating what the process is. To consider what you are doing as TDD then you must:

1. For each new feature or behaviour write the test first.
1. Run the test expecting it to fail.
1. Write just enough code to make the test pass.
1. Refactor the code to tidy up, remove duplication etc.
1. Repeat

Comments were made about this being "Strict-TDD" which made me think of Jason Gorman's [TDD Maturity Model](http://codemanship.co.uk/parlezuml/blog/?postid=1066). 

This leads into this post about the benefits and drawbacks of using TDD as a way of writing the code. I am not going to break them into two lists as sometimes I think the comments made about TDD can be argued either way, i.e. a benefit or a drawback depending on your perspective. Instead I will take each comment in turn.

### More Tests == More Productive

[According to wikipedia](http://en.wikipedia.org/wiki/Test-driven_development#Benefits) (always a reliable source of information) [developers who write more tests are more productive](http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&index=an&req=5763742&lang=en) irrelevant of programming technique employed. 

I would argue that if I write the tests first and I only produce code to satisfy those tests then proportionally I am going to write more tests than someone developing the same functionality but writing the tests afterwards. 


### Effective Test Coverage

Since I only write a line of code to satisfy a failing test then it stands to reason that I should have a high level of code coverage. 

Running a coverage tool as part of the build should simply be a formality rather than something you need to do to then retrofit tests to plug coverage gaps.

I used to find when writing tests afterwards that a number of things happened:

1. I struggled to remember/understand what the code did and as such would have difficulties adding the necessary tests to bring the coverage up.
1. My enthusiasm would wane. Getting the test coverage up the first 60% or so would be easy, after that it is the law of diminishing returns. It gets harder to increase the coverage at the same rate, as the design of the code doesn't lend itself to easy testing. By the time you are up to 70-80% then you are pretty sure that it works as you expect and you are eager to move onto the next thing leaving you exposed around that behaviour.

It is not uncommon to find codebases where people consider coverage of 60% to be acceptable and 70+% as something to aspire to, mainly because they are retrofitting the tests to the code. 

I don't believe in setting some arbitary coverage target as it can become an unnecessary distraction as people try and game the system. I do believe it is a useful indicator when used with other metrics. The point is with TDD you shouldn't need to set a target, the value should be high as a side effect.

Be careful that a high code coverage doesn't lead to overconfidence. Having a good test suite does allow you to refactor more safely but you should still move in the small steps encouraged by TDD.

### More Tests == Less Productive

One of the criticisms I have heard levelled at TDD by developers and teams is that in the beginning it was great but now it is slowing them down. They have a large test suite, high code coverage, they follow the practises. Now the project is further down the line it is harder to introduce new functionality. Changes cause multiple tests to break and they suffer what they call "Death by a thousand tests". Sometimes they plough through fixing the tests whereas other times they simply ignore or delete them removing their safety net.

I recognise the symptoms and have suffered them myself. I would counter that this is not necessarily a failing of TDD but more one of TDD done badly. 

TDD does have a steep learning curve associated with it and it took me years of reading and practise before I felt that I was getting better at it. It is easy to go crazy with Mocks, over test things, test at too low a level, too high a level, etc. 

Future articles will address some of the common pitfalls teams encounter when applying TDD and how to move to a better way of working. There are two important points here that are worth highlighting now:

1. Tests should be written just as well as production code. I have heard people say that tests aren't shippable so don't matter as much. This is simply not true and this attitude will definitely lead you to a nasty death by a thousand tests. 
1. The refactor step of TDD is really important. Most people don't do enough refactoring around the loop. If you don't keep the refactoring up then the code quality will drop and at some point you will slow down.

### Better Design
As you listen to the tests and refactor mercilessly as you go you should find that you pull the direction of the design towards higher quality, more maintainable code. This will be a topic of future posts.
### A Form of Documentation
Some people are very keen on documentation, I am less so. Writing detailed documentation for classes & methods is a form of duplication and we know what we should do with repetition in code don't we? 

Instead the tests act as a form of documentation. They show not only the behaviour of the system but the intent of the developer at the time. 

[Uncle Bob compares](http://unhandled-exceptions.com/blog/index.php/2009/02/15/uncle-bob-tdd-as-double-entry-bookkeeping/) this to the method of double entry book keeping in accountancy.