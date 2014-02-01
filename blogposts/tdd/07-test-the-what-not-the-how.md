*By Andy Stewart*

The last couple of posts on TDD focused on how to get useful feedback from the tests when they fail. Now we want to take the example a bit further and extend the functionality. 

In this post we will add a test to calculate the area of a rectangle and then refactor the tests to make them more resiliant to changes in the implementation.

The first thing we want to do is add a test since we remember that we shouldn't write any new production code unless there is a test to cover it.

#### Adding a test for the Rectangle

	@Test
	public void testSquareArea() {
	    Square square = new Square(2);
	    assertEquals("area of square", 4, square.area());
	}
	
	@Test
	public void testRectangleArea() {
		Rectangle rectangle = new Rectangle(2, 3);
		assertEquals("area of rectangle", 6, rectangle.area());
	}

So now we have our two tests, the first one still passes and the new one fails. Let's remind ourselves of what the code for the Square looked like:

    public class Square {

	    private final int lengthOfSide;
	
	    public Square(int lengthOfSide) {
		    this.lengthOfSide = lengthOfSide;
	    }

	    public int area() {
		    return this.lengthOfSide * this.lengthOfSide;
	    }
    }

The quickest way to get the test passing is just copy and paste the Square class and make the necessary tweaks:

    public class Rectangle {

	    private final int height;
	    private final int width;
	
	    public Rectangle(int height, int width) {
		    this.height = height;
		    this.width = width;
	    }

	    public int area() {
		    return this.height * this.width;
	    }
    } 

Now we have two tests that pass and we are ready to tidy up a bit. The first thing we want to do is introduce an interface for a Shape. The interface will define that all shapes have an area. 

#### Introducing an Interface

First of all we modify the tests:

	@Test
	public void testSquareArea() {
	    Shape square = new Square(2);
	    assertEquals("area of square", 4, square.area());
	}
	
	@Test
	public void testRectangleArea() {
		Shape rectangle = new Rectangle(2, 3);
		assertEquals("area of rectangle", 6, rectangle.area());
	}
    
Our interface looks like so:

    public interface Shape {
	    int area();
    }

Both the Square and the Rectangle class are made to implement the interface, we run the tests and they pass.

#### Hiding the Implementation

Our test is still exposed to the implementation of the Square and Rectangle. If we change how they are constructed then we will need to modify the tests. To overcome this we use the builder pattern. 

Now our test looks like so: 

	@Test
	public void testSquareArea() {
	    Shape square = Shapes.squareWithSideOfLength(2);
	    assertEquals("area of square", 4, square.area());
	}
	
	@Test
	public void testRectangleArea() {
		Shape rectangle = Shapes.rectangleWithHeightAndWidth(2, 3);
		assertEquals("area of rectangle", 6, rectangle.area());
	}
    
The implementation of the factory looks like:

	public static Shape squareWithSideOfLength(int length) {
		return new Square(length);
	}

	public static Shape rectangleWithHeightAndWidth(int height, int width) {
		return new Rectangle(height, width);
	}
    
#### Removing Duplication

Now that we have an interface defining the functionality we are interested in and a factory decoupling us from the implementation we can actually remove the Square class entirely.

We notice that a Square is simply a Rectangle that has it's width and height to be constrained to the same value. The calculation for the area is basically a duplication. We can delete the Square class and change the factory to return an instance of Rectangle:


	public static Shape squareWithSideOfLength(int length) {
		return rectangleWithHeightAndWidth(length, length);
	}

	public static Shape rectangleWithHeightAndWidth(int height, int width) {
		return new Rectangle(height, width);
	}
    
What we have been able to do here is change the underlying implementation and leave the tests unchanged because we have extracted an API and tested against that rather than direct against the implementation.