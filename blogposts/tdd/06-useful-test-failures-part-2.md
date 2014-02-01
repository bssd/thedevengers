*By Andy Stewart*

In the [previous post](http://thedevengers.com/tdd-useful-test-failures-part-1/) we talked about making sure we had helpful messages when our tests fail. In this one I want to talk about [Hamcrest matchers](http://hamcrest.org/) which are another mechanism for helpful error messages available with Java and JUnit (in fact they have now been ported to other languages such as Python).

Hamcrest matchers are used in conjunction with JUnits `assertThat` method. Taking the example we had previously :

    @Test
    public void testArea() {
        Square square = new Square(2);
        assertEquals("area of square", 4, square.area());
    }
    
We can rewrite this as:

    @Test
    public void testArea() {
        Square square = new Square(2);
        assertThat(square.area(), CoreMatchers.is(4));
    }
    
With static imports we can tidy up the reference to CoreMatchers are are left with:

    @Test
    public void testArea() {
        Square square = new Square(2);
        assertThat("area of square", square.area(), is(4));
    }

So what benefits do we get from this change? 

#### Code Readability

The first benefit is the code reads better, it almost reads like how you would phrase the condition being tested, i.e. "Assert that the area of the square is 4".

#### Improved Error Reporting

The second benefit is that the Matchers are implemented in such a way that they return helpful error messages (you can still include a descriptive message as before to add extra context). If the test fails this is what is reported:

    java.lang.AssertionError area of square: 
      Expected: is <4>
         but: was <0>
	   at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
	   at org.junit.Assert.assertThat(Assert.java:865)

Admittedly not much of an improvement on what we had before but it pays off more when we have assertions such as:

    assertThat(Arrays.asList(1, 2, 3), CoreMatchers.hasItem(4));
    
Giving the error:

    java.lang.AssertionError:
      Expected: a collection containing <4>
         but: was <1>, was <2>, was <3>
	   at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
	   ...
       
Imagine trying to do that using the traditional assertion methods, you would maybe have something like:

	assertTrue("item in list", list.contains(item));
    
If the test fails you will know the value that should exist in the list but have no insight as to what it actually contains. This increases the odds that you are going to have to break out the debugger and spend time stepping through the code.

#### Composibility

You can compose matchers together making them incredibly powerful. The simplest example is negating something, e.g.

    assertThat(square.area(), not(equalTo(0)));
    
Although if you know what a value should be then that should be the value you use in the test. In this example the test would pass as long as any value other than 0 is returned by the implementation.