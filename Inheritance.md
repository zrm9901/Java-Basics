# Intro
Hey, I'm back with another couple of tips for Java. This time I'm going to be talking about subclasses, abstract classes, and interfaces. (those are ranked in my opinion from least to most useful), the things that make up inheritance.

# What is inheritance?
Inheritance is the act of defining what is called a 'parent' class that other classes build off of. For example, you can make a class like this
```
public class Parent { //doesnt have to be named parent
    private int defaultInt;
    private boolean defaultBool;
    public Parent(int i, boolean b) {
        this.defaultInt = i;
        this.defaultBool = b;
    }
    public void default() {
        System.out.println("default");
    }
    //getter and  setter methods here
}
```
> if you dont know why I did private vs public look at this [Article](/GameState.md)

And while thats great and all, what if you wanted to build off of that class? make one or two modifications or add things? Well thats where inheritance comes in. You have three options. 

# Subclassing
This is the first and easiest to use and understand, and this is where some people stop learning of better ways. By using the 
> "extends"

modifier in class creation you can basically inherit all of the methods and variables from the parent class easily. This comes with benifits and drawbacks though, which I will go through in a second. In the meantime heres how to do it.
```
public class Child extends Parent { //using the extends keyword to inherit
    public Child(int i; boolean b) {
        super(i,b);
    }
}
```
And there we have it, this is now a child class. From here you can use all the methods from the parent class and even add more
```
public Child c = new Child(1,true);
c.default();
```
We call the methods just like we would the parent class. Now you may be wondering what we did a second ago, calling super. That is a key part of how subclassing and most inheriting works. When you extend a class, you keep a reference to its parent class in the form of 'super', and whenever you extend a class, remember that you get **all** its methods, so when you call 
```
super(i, b);
```
you are actually calling the parents constructor for your child class. Im not wording it the best but basically you **always** need to do super() and the parems for the parent class for it to work because you inherit the constructor too so you need to resolve that. You can then add any additional logic in the constructor that you need. Then we have the methods. The reason I dont particularly like subclassing and instead go for interfaces (which we will get to later) is that whenever you want to add logic to a parent method, you have to basically copy paste the parent methods logic in first. For example, take the default method from the Parent class. If we wanted to add logic to it we would do something like this
```
//in the child class
@override
public void default() {
    System.out.println("default"); // add the parent logic in by hand
    //new logic goes here
}
```
The reason we have to do this is how extending works. Whenever you extend a class, if you want to edit its methods you cant just add logic, you have to basically rewrite the method again, because it just overwrites everything. You are basically replacing the previous method with the new one. Its a bit annoying but if all you need to do is call the previous method there is a kind of hacky solution
```
public void newDefault() {
    this.default();
    //new logic
}
```
You can just call the function in another one to add new logic. This does come with the drawback of making the code a bit messy if you do this a lot. Especially if you extend a class, do that, then extend the new one, you have all of these extra methods you arent using, they just compound on eachother. The use of subclasses is the fact that they can be used **any** place the parent class is used. For example, say you have a method like this
```
public void example(Parent p) {
    p.default();
}
```
you can actually pass the child class into example.
```
Child c = new Child(1,true);
//given example is a method of object
object.example(c);
```
This is pretty useful and can be used for a thing called polymorphism (definitely did not spell that right lol) which basically means a bunch of child classes that are closely related and have the same method names. The intention is that you can pass any of the child classes into something and they all so different things with the same method name. The only thing to watch out for is whenever you do something like this 
```
public Parent modify(Parent p) {
    p.set(defaultInt, 20);
    return p;
}
```
be mindful that it is returning the child class put in, but in the parent form. this means that if you add any methods to the child class, you wont be able to use it unless you cast back to the child class.
```
Child c = new child(10, true);
// again, object contains modify
Parent p = object.modify(c);
```
Notice how I used the Parent type? if you tried to assign the returned parent to the Child class it would have thrown an error. Right now its basically a Child class hidden in its Parent class. You cant use any child methods or variables until you cast it back by doing this
``` 
Child newc = (Child) p;
```
This is how you typecast all types of variables, but you have to be absolutely sure that it is a Child class before you do this because it will throw an error. to do this you need to do something like this
```
if (p instanceof Child) {
    Child newc = (Child) p;
}
```
The sad part is this can only do so much. When you try doing some more advanced things with polymorphism you run into some stuff, thats when you turn to interfaces.

# What are interfaces?
If you've gotten to the point where you have multiple child classes or a bunch of tightly knit classes in general, you might want to look into interfaces. Interfaces are another type of encapsulation, they basically act as well, interfaces between the class and the rest of the codebase, they basically have a couple of empty methods that they define that **have** to be implemented for it to work, and act almost like a type of getter and setter, with added functionality on top of that. Interfaces dont have any variables, they only have empty methods (well you can make a fallback but more on that in a sec), and they are created with a new keyword. they depend on the classes implementing them to supply the code for those methods and the variables associated with them. Heres how to create one.
```
public interface Face {
    public int getVar();
    public void setVar(int i);
}
```
Nice and simple, this interface is now ready to be used like this
```
public class Example implements Face {
    private int var;
    public int getVar() {
        return var;
    }
    public void setVar(int i) {
        var = i;
    }
}
```
Note that you cannot create the base interface, it **has** to be implemented. Anyways, this is a very simple interface, but the beauty of it is you can use any class that implements the interface interchangebly. the only caviat is if you do the  same thing with subclasses and return a modified one you will have to cast it back if you need methods, but something about interfaces that differ from subclasses, you can completely change up how the method is carried out, and thats generally how they are used. It enhances secutity and also, unlike extending classes, you can implement multiple interfaces. This makes them one of the most versatile objects in Java (I might have said that about arrays but I mean it this time). One last thing, if you do want to have a fallback method for an interface you can do that with the default modifier
```
public interface Face {
    public int getVar();
    public void setVar(int i);
    default void sayHi() {
        System.out.println("hello world");
    }
}
```

# Abstract classes
So there is this third catagory of inheritance called abstract classes, and these are a combo of interfaces and subclasses. It takes the undefined methods and not being able to create a straight up object from interfaces and the constructors and super from subclasses. This can be useful if you have even closer knit classes where you can have shared methods you can do that with abstract classes. I was never able to really find a use for these that couldnt be filled by interfaces but if find one good on you. They can be created with the abstract keyword and if you want methods that the subclass has to implement you can use it there too
```
public abstract class Abs {
    public Abs() {

    }
    public abstract void doSomething();
}

public class child extends Abs {
    public child() {
        super();
    }

    public void doSomething() {
        System.out.println("did something");
    }
}
```
I dont really know what else to say about them other than you *may* be able to call super when overriding methods but i am unsure, never got it to work. 

Edit: **you can!** I just was doing it wrong, you can also do it on regular subclasses, with the super keyword
```
@override
public void saySomething() {
    super.saySomething();
    System.out.println("Additional thing");
}
```

# so what should I use for what?
If you want a lot of similar object with different methods you can call by the same name do an interface, if not i mean you can try abstract classes or subclasses.

# outro
Have fun, I did not explain this well, next I will be going over generics, which upgrades interfaces, so thats the reason I love them so much. ttyl