*By Andy Stewart*

In a [previous post](http://thedevengers.com/tdd-how-small-should-your-steps-be/) we touched upon triangulation as an alternative to faking an implementation or coding it up right away. This article is about how you would use triangluation to slowly move towards a general solution rather than taking one big leap.

The process of triangulation takes the form of:

1. Think of the simplest example you can to move you closer to the solution.
1. Write a test to cover that example, implement the solution and then refactor.
1. Repeat until you can think of no more examples.

The trick when thinking of examples is to make sure that each one introduces some new behaviour. Remember the goal is not to get your coverage or number of tests up but to move you closer to the solution.

To demonstrate triangulating to a general solution we will work through implementing Fizz Buzz. The requirement is fairly straightforward:

> "Write a program that prints the numbers from 1 to 100. But for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”. For numbers which are multiples of both three and five print “FizzBuzz”."

Sounds pretty straightforward doesn't it? However as [this post by Jeff Atwood](http://www.codinghorror.com/blog/2007/02/why-cant-programmers-program.html) and [this other post](http://c2.com/cgi/wiki?FizzBuzzTest) demonstrate most programming candidates would fail if presented with this test at interview.

I am going to implement a slightly more general solution of FizzBuzz that will return an array containing the number of values specified.

The first trap to avoid is **NOT** to write a test like this straight off the bat:

    @Test
    public void testFizzBuzz() {
        assertThat(FizzBuzz.fizzBuzz(5), is(equalTo(
            new String[] { "1", "2", "Fizz", "4", "Buzz" })));
    }
    
The temptation is to solve the whole thing in one go (except you would need to go upto at least 15 for a start) and then dive in. Remember that we are trying to take small steps.

#### The first test

The simplest example I can think of is to write the first number so that's the test we write:

	@Test
	public void testPrintNumberSingleNumber() {
		assertThat(FizzBuzz.fizzBuzz(1), is(equalTo(new String[] { "1" })));
	}
    
Let's fake up the result first:

	public static String[] fizzBuzz(int max) {
		return new String[]{"1"};
	}

Then we can generalise a bit by refactoring away the duplication of the string literal:

	public static String[] fizzBuzz(int max) {
		return new String[]{String.valueOf(max)};
	}

#### The second test
	
At this point I might be tempted to tackle printing Fizz as the 3rd number. However that is a bigger step than I want to take at this stage. The next behaviour I want to introduce is returning more than one number, so this is the test I write next:

    @Test
	public void testPrintMultipleNumbers() {
		assertThat(FizzBuzz.fizzBuzz(2), is(equalTo(new String[] { "1", "2" })));
	}

This forces us to add a loop, which has a little bit of complexity because the array is 0 indexed but we are dealing with numbers 1 to x. I actually got the implementation wrong when coding this up but the test caught my mistake. 

	public static String[] fizzBuzz(int max) {
		String[] result = new String[max];
		for (int i = 0; i < max; i++) {
			result[i] = String.valueOf(i+1);
		}
		return result;
	}
    
At this point we want to refactor and remove duplication. There isn't any duplication in the implementation but I can see that the tests have some. It is going to get pretty tedious if we have to type out the whole array for each test to check just one specific value. 

To help with this I introduce a helper method that will examine the particular part of the result that we interested in, giving us:

	@Test
	public void testPrintNumberSingleNumber() {
		assertThat(fizzBuzzValueAt(1), equalTo("1"));
	}

	@Test
	public void testPrintMultipleNumbers() {
		assertThat(fizzBuzzValueAt(2), equalTo("2"));
	}
	
	private String fizzBuzzValueAt(int max) {
		return FizzBuzz.fizzBuzz(max)[max-1];
	}
    
The tests read a lot better, they have less duplication and they focus on the particular behaviour we are interested in.
    
#### Adding the Fizz

Now that we return multiple results and our tests are reading nicely it is trivial to add the test for the Fizz:

	@Test
	public void testPrintFizz() {
		assertThat(fizzBuzzValueAt(3), equalTo("Fizz"));
	}

I must admit I dived in here and went straight to the modulo solution rather than faking it up. I think in this case it's trivial enough to take the jump.

	public static String[] fizzBuzz(int max) {
		String[] result = new String[max];
		for (int i = 1; i <= max; i++) {
			if (i % 3 == 0) {
				result[i-1] = "Fizz";
			} else {
				result[i-1] = String.valueOf(i);
			}
		}
		return result;
	}
    
We have a small amount of duplication in the way we set the value at the index in the array which can be refactored out to: 

	public static String[] fizzBuzz(int max) {
		String[] result = new String[max];
		for (int i = 1; i <= max; i++) {
			result[i - 1] = value(i);
		}
		return result;
	}

	private static String value(int i) {
		if (i % 3 == 0) {
			return "Fizz";
		}
		return String.valueOf(i);
	}
    
#### A little bit of Buzz

This makes adding the Buzz trivially easy, add the test first:

	@Test
	public void testPrintBuzz() {
		assertThat(fizzBuzzValueAt(5), equalTo("Buzz"));
	}

Then simply amend the value method:

	public static String[] fizzBuzz(int max) {
		String[] result = new String[max];
		for (int i = 1; i <= max; i++) {
			result[i - 1] = value(i);
		}
		return result;
	}

	private static String value(int i) {
		if (i % 3 == 0) {
			return "Fizz";
		} else if (i % 5 == 0) {
			return "Buzz";
		}
		return String.valueOf(i);
	}
    
#### The Final Flurry

Now all we have left to do is FizzBuzz. The test is simple:

    @Test
	public void testPrintFizzBuzz() {
		assertThat(fizzBuzzValueAt(15), equalTo("FizzBuzz"));
    }
    
First we hard code it:

	private static String value(int i) {
		if (i == 15) {
			return "FizzBuzz";
		} else if (i % 3 == 0) {
			return "Fizz";
		} else if (i % 5 == 0) {
			return "Buzz";
		}
		return String.valueOf(i);
	}
    
Then it strikes us that any number divisible by 15 is FizzBuzz and we refactor to:

	private static String value(int i) {
		if (i % 15 == 0) {
			return "FizzBuzz";
		} else if (i % 3 == 0) {
			return "Fizz";
		} else if (i % 5 == 0) {
			return "Buzz";
		}
		return String.valueOf(i);
	}

All of the behaviour is now covered off and we have finished implementing our FizzBuzz using triangulation.

	
    
    