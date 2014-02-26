*By Andy Stewart*

I attended a presentation at [Agile Yorkshire on Devops](http://www.leedsdevops.org.uk/post/76756623063/agile-yorkshire-presentation-11-02-2014-devops-101) and one of the things mentioned was [Poka yoke](http://en.wikipedia.org/wiki/Poka-yoke). The definition from wikipedia is:

> A poka-yoke is any mechanism in a lean manufacturing process that helps an equipment operator avoid (yokeru) mistakes (poka). Its purpose is to eliminate product defects by preventing, correcting, or drawing attention to human errors as they occur

An example in manufacturing is to produce parts that can only be assembled together one way. 

This sounds like an awfully good thing to do when it comes to building software doesn't it? If we write our code in such a way that it only goes together in ways that are correct then we reduce the chance of introducing bugs by mistake. 

Imagine a scenario where you have been asked to integrate with a 3rd party system for Foreign Exchange (FX) Trading. You are handed a Java library which has no source or documentation. All you have to go on are the method signatures presented in the IDE. 

The first piece of functionatity to implement is placing an order. Let's take a look at the API for order placements:

    String placeOrder(String arg0, String arg1, BigDecimal arg2, int arg3);

Hmmmm, that really isn't very helpful is it? The parameter names have been lost on compilation so all we have to go on is the types. We could take a guess that the BigDecimal is the price and the int is the quantity. However what could the Strings possibly be? How do we interpret the return value?

Passing basic types around is a very good way of hiding or losing information, I often refer to this kind of code as "Stringly typed". A much better solution is to introduce a Microtype or Value object. 

What if the library we were given had this interface instead:

    PlacementResult placeOrder(Side arg0, CurrencyPair arg1, Price arg2, Quantity arg3);
    
That is much better, even though we don't have any documentation or source code we can see what those parameters are. 

* The Side is whether the Order is a buy or sell representend by an enumeration. 
* The Currency Pair is the two currencies being traded, i.e. USD/GBP.
* Our guess about Price and Quantity is confirmed. 
* We can decipher the result now as it is an enumeration representing all the possible outcomes rather than some unknown value.

Initially these value objects might just be a very thin wrapper around the underlying representations. Over time they represent somewhere appropriate to put behaviour. 

At some point in the future we might want to calculate the total value of an Order.  We decide to add a multiply method on the Price object which takes a Quantity object. This would return a Total or Money object which would represent both the value and currency. 

In summary Microtypes (or Value objects) limit the opportunity for methods to be called incorrectly. They also define the system in terms of the problem domain making the code clearer in terms of it's behaviour. 
