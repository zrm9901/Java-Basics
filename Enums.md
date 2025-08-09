# Intro
hey, I'm back :D. Today I'm going to be talking about enums, and how they actually are a lot more useful than you may think

# So what are enums?
Ok so if you are coming from a different language you might think of enums as a constant group of variables, all within one container, but they work a little bit differently in java. See in java, you can think of them as a special type of class, but keep the constant part. enums are defined and created on spot, and every **constant** inside is public, static, and final (cannot be overriden). This makes enums perfect for things that dont change on runtime. Oh yeah, enums actually are classes, they just cant extend another class because they alrealdy extend the class that makes them an enum. They can also implement interfaces which is fun.

# What do they consist of?
So there are three things an enum can hold: Constants, static blocks, and methods. Constants are the bread and butter of enums, they are the things publicly accessable to everyone, you can think of static blocks as a sort of mini constructor, and methods are well, methods. The cool part about enums is you dont really need to have static blocks or methods, they work just fine with just constants, so you can use them like a type block where you have enums you switch between. Oh yeah, one other thing that enums can have is private variables, which is most useful for things like charts that corrospond to the constants, but Im getting ahead of myself.

# So what do constants look like
So constants are plain and simple, they just look like this in an enum
```
enum Example {
    VAR1, VAR2, VAR3;
}
```
Btw it isnt mandatory per say to have them all capitalized, but just like classes its common practice. Now that you have them defined, you can use them like this
```
Example e = Example.VAR1;
```
notice how the type is Example and you access them with the dot syntax, just like in a normal class (almost like enums are a type of class). Basically, the enum constants at this level are glorified identifiers. You can do about as much as 
```
if (e == Example.VAR1) {
    //logic here
}
```
And thats as for as they go in other languages (at least as far as I can tell), but in java they can do a **lot** more.

# So, what else can they do?
Well, most of their functionality comes down to storing things or even executing certain methods. First off, lets get into static blocks. So I said before that enums can have private vars, and they can be veeeery useful for storing even more data inside enums. The most useful way I've found of doing it is using an EnumMap like so
```
//this is insude the enum "Example"
VAR1,VAR2,VAR3,VAR4,VAR5;

private static EnumMap<Example, Integer> nums = new EnumMap<>(Example.class);
```
(I have no idea why the .class thing at the end but if it works it works). Now you have a basically database tied to your enums, and how you fill it out is something called static blocks. I mentioned before that static blocks were kind of the constructors of enums, and this is what I meant
```
static {
    nums.put(VAR1, 245);
    nums.put(VAR2, 13);
    nums.put(VAR3, 78);
    nums.put(VAR4, -20);
    nums.put(VAR5, 5009);
}
```
they basically set up the databases with information, and you may be wondering, how do I get it out? thats where methods come in.

# How do methods work
So basically, you define a method on the same level as  your constants, like so
```
public enum Example {
    VAR1,
    VAR2,
    VAR3,
    VAR4,
    VAR5;

    public void printSelf() {
        System.out.println(this);
    }
}
```
The fun thing about methods in enums, you define them once, they work on **all** the constants. So you can just call it like this
```
//assuming Example is the one defined above
Example e = Example.VAR1;
e.printSelf();
```
Its that simple, this is a simple method that just prints the name of the enum, but you can do so much more. You remember the databases that we made earlier, the main purpose of them is to do something like this
```
//example code from a second ago

public int getInt() {
    return nums.get(this);
}
```
so basically, each constant behaves like its own miniclass, so it has access to the this var and all the methods of the enum, which is why your able to use "this". Oh yeah, you can also pass things into the methods to do some pretty cool stuff. At the end of this I'll include a link to the projct I learned all this so you can see what that looks like. So, the last thing I want to talk about is overriding methods in enums. Thats right, my *favorite* thing to do is a thing in enums. heres how you do it
```
//this is inside an enum
VAR1 {
    @override
    public void doSomething() {
        System.out.println("Something Else);
    }
},
VAR2;

public void doSomething() {
    System.out.println("Did Something);
}
```
Looks a bit clunky with only one being overriden but now VAR1's doSomething() and VAR2's doSomething() do different things, neat right?

# Interfaces with enums
So I just learned this like yesterday, but you can implement interfaces in enums, and have their methods called, you just either have to define the interface methods in the top level of the enum so it is in every constant or just in each constant seperately.

# So, what does this look like in practice?
Line 170 of [this project that I made](https://github.com/zrm9901/IdkWhatToCallThisONe/blob/main/core/src/main/java/com/mygame/Units.java) has a fully fledged enum logic block for lack of a better term, I'd reccomend checking it out, it baasically defines all of the logic for the project. 

# Outro
As always, have fun coding and good luck on your future projects :D.