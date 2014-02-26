*By Andy Stewart*

At this months [Agile Yorkshire](http://www.agileyorkshire.org/) a few of my fellow Devengers and I were discussing various Java coding standards and bikeshedding. For those unaware as to what bikeshedding (or [Parkinson's Law of Triviality](http://en.wikipedia.org/wiki/Parkinson's_law_of_triviality)) is, it states that disproportionate weight is given to trivial matters. It is so called because everyone has an opinion on what colour the bike shed should be. 

We had a fairly robust and excitable discussion about some of the coding standards we see applied to Java and this is my opinion on a few of them. 

**Can't get enough of 'this'**

A few of us have been swapping back and forth between Python and Java recently and frequently reminded how verbose Java is. One Devenger commented that they have taken to generally not using "this" when accessing member variables. The main reason being it added to the noise without changing what the code does.

Now I would argue that you should still add "this" because it makes clear your intent and avoids errors such as:

    private String property;
    
    public void setProperty(String propertee) {
        this.propertee = propertee; // doesn't compile 
    }
    
Versus:

    private String property;
    
    public void setProperty(String propertee) {
        propertee = propertee; // compiles but has no effect
    }
    
The counter to this is the fact that the IDE will colour the member variables blue as opposed to black for parameters and local variables. Personally I don't find there to be enough of a contrast between them to notice it and think colour can be a poor way to draw peoples attention, for example someone might be colour blind. 

So I think you should use "this".

**Use braces liberally**

In Java it is perfectly legal to write:

    if (someBooleanCondition) 
       runningTotal++;
      
In the example above there are no braces, if the condition is met then the first and only the first line after is evaluated. 

Now if someone comes along and adds another line, maybe for some logging and they don't pick up on the lack of braces (and trust me it's easily done) then you end up with:

    if(someBooleanCondition) 
        System.out.println("Condition triggered");
        runningTotal++;
        
Now they will see the output in the console when the condition is met but the value for runningTotal won't get incremented. The behaviour of the program has been changed and the author doesn't necessarily realise. For this reason I always add the braces, again I think it is worth the increased verbosity for that extra bit of clarity.

**Make everything final**

I worked on a project where as a general rule we would make everything final. There are good reasons for doing this and it does encourage good habits such as never reassigning variables (often a source of subtle bugs). However over time I have come to think of using final everywhere as overkill and now have 3 general rules of thumb:

1. If you think you need to make a method parameter final then you don't understand how Java works and you should go and read up on how it does.
1. If you have a member variable and that represents a collaborater of the class  that must be present for it to fulfil it's role then that should be made final and instantiated somehow in the constructor (preferably through dependency injection). 
1. Generally never reassign a local variable but don't use final. It is in methods where the extra noise can really impact on the readability of the code.
1. If you think you need to make a method parameter final then you don't understand how Java works and you should go and read up on how it does.

Another point here is when I find myself having to make params and variables final to access them from anonymous inner classes then that is a good sign that a proper class should be extracted at that point.

**I is for Interface**

This was probably the most contentious one of the lot. In the Java world we see a lot of `EmailService implements IEmailService` where the one beginning with an I is an interface and the other is the implementation.

As stated in [GOOS](http://www.growing-object-oriented-software.com/) this is a form of repeating yourself. The implementation name conveys no more extra information than the interface. 

Note that I am not advocating `EmailServiceImpl implements EmailService` as the alternative.

A good example of how it should be done is the Java Collections library. They have interfaces such as `List` & `Set` and the concrete class name conveys the interesting detail of the implementation such as `ArrayList` or `HashSet`. 

When using the collection throughtout the code I care about the behaviour defined by the interface such as whether it is a List and allows duplicates or a Set which doesn't. At construction time I can choose the most appropriate implementation based on requirements for performance, storage or memory which I can make a judgement from the name.

