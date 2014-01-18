*By Andy Stewart*

In [Test-Driven Development by Example](http://www.amazon.co.uk/Test-Driven-Development-Addison-Wesley-Signature/dp/0321146530) Kent Beck says that after writing that failing test we should get the test bar green as quickly as possibly committing as many sins as necessary to go as fast as we can. Examples include hard coding return values or maybe cutting and pasting code from somewhere else. 

His argument is that once the test is green then you can refactor with more confidence and concentrate on removing duplication. 

This can lead to a lot of resistence from people new to this way of working. Say we were writing some code to calculate the area of a square (a trivial example I know) then our first test might look like:

    @Test
    public void testArea() {
        Square square = new Square(2);
        assertEquals("area of square", 4, square.area());
    }

We have our test and now we want to make it pass as quickly as possible. We use our favourite IDE to quickly do the following:

1. Create a Square class.
1. Add a constructor which takes an int representing the length of side.
1. Create the area method which returns an int.

Now we have a decision to make, do we:

* Hard code the result to be 4?
* Implement the correct calculation (side * side)?

If we are sticking to the letter of the law then we should hard code it to 4.  Beck then has an argument to refactor the 4 to the real calculation you can say it is a form of duplication as it is repeated in the test. So you refactor to `2 * 2` and that is then a duplication of the side passed into the constructor and so on.

However in this case it's not needed, the answer is so trivial we can go straight to the correct solution. Afterall we are pretty sure we are going to get it right. 

As he describes in the book TDD can be a little fuzzy, a lot of it is taking judgement calls about what size of steps you should take. He has 3 strategies for this which are:

1. Fake it
1. Implement it
1. Triangulate

In the example hard coding 4 is faking it and returning the side multiplied by itself is the real implementation. Triangulation is something we will cover in a later post. 

The key is being able to take the smallest steps when necessary but being able to take bigger steps when on surer ground.

