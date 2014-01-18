*By Andy Stewart*

In the [previous post](http://thedevengers.com/tdd-how-small-should-your-steps-be/) we talked about what size of steps you should take with TDD and I presented a small example. To remind you here is the example again:

    @Test
    public void testArea() {
        Square square = new Square(2);
        assertEquals("area of square", 4, square.area());
    }
    
This example is written in Java using JUnit. I assume that most developers are familiar with an xUnit framework in their langauge(s) of choice. The code is hopefully fairly self explanatory. 

What I want to talk about in this post is making your tests fail with a helpful error message. 

The interesting part of the test is the description String `"area of square"` passed as the first parameter to the `assertEquals` method. The 2nd parameter is the expected outcome and the 3rd is the actual result. 

If the expected and actual don't match then JUnit will fail the test as our assertion is incorrect. 

If we did not provide the 1st parameter (the method is overloaded so a version taking only the expected and actual is available) then we would get a stacktrace such as:

    java.lang.AssertionError: expected:<4> but was:<2>
	    at org.junit.Assert.fail(Assert.java:88)
	    at org.junit.Assert.failNotEquals(Assert.java:743)
	    at org.junit.Assert.assertEquals(Assert.java:118)
	    at org.junit.Assert.assertEquals(Assert.java:555)
	    at org.junit.Assert.assertEquals(Assert.java:542)
	    at uk.co.bssd.tdd.shapes.SquareTest.testArea(SquareTest.java:13)
        ...

From the stacktrace we can determine where in the test it failed and find the assertion but it is not immediately obvious why it failed. Also remember this example is very simple and we have a relatively short stack trace to examine (although I have trimmed it for brevity after the interesting bits). 

Now with the description provided the stacktrace looks like:

    java.lang.AssertionError: area of square expected:<4> but was:<2>
	    at org.junit.Assert.fail(Assert.java:88)
	    at org.junit.Assert.failNotEquals(Assert.java:743)
	    at org.junit.Assert.assertEquals(Assert.java:118)
	    at org.junit.Assert.assertEquals(Assert.java:555)
	    at uk.co.bssd.tdd.shapes.SquareTest.testArea(SquareTest.java:12)
	    ...

Now it is a bit more obvious. At the top of the stacktrace we can see the test expected the area of the square to be 4 but was 2. That extra hint helps us key in much sooner to where the problem might be.

This kind of facility should be available in most xUnit frameworks. When you write your failing test take care to ensure that it shows the failure you are expecting and it is clear why it failed. 

Whoever has to maintain your code months afterwards (and that might well be you) will thank you for it.

In the next post we will look at using Hamcrest Matchers with JUnit.