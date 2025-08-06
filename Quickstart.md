# Intro
Hey, I'm Zach and I'm creating a curriculum for teaching Java, and this seems like a nice site to start on. 

For anyone starting out from this tutorial, I do want to say that this will be the basics for now, things you need to do basic tasks like statistics and data analysis, but I will have a game dev post relatively soon.

#### Requirements
* A capable code editor and compiler
* The JDK (a runtime kit that makes compiling the code possible)
* Basic problem solving skills and a critical thinking mind

You can run java code on some web based compilers but for the sake of this tutorial I'm going to assume you have VSCode.

#### Resources
* [W3 Schools](https://www.w3schools.com/java/default.asp)
* [ChatGPT](https://chatgpt.com/?model=auto)

# What is OOP?
Ok, so one key thing to know getting into Java is that it is an object oriented language. That means that you cant just have standalone functions, they need to be contained in an object. In java's case its classes. Basically, each class is a template, think of it like a cookie recipe, you have instructions to do something and out pops a cookie (a class instance). Now these instructions can have variations, think of those like instance variables. We will get into this more when we talk about classes, but basically, each object can hold variables and methods, and to access the variables and methods you have an object that you create to hold them.

# Syntax
So in java, you **need** semicolons after every declaration. By that I mean every time you declare a variable, call a method, anything other than conditionals. 
```
String s = "hello world"; //correct
String s = "hello world" // no semicolon, will throw an error
```
Java also is peculiar about statements. If you've come from lua or python you might be familiar with indentation defining scope and blocks, in Java its {}. the last relevant thing about syntax is conditionals go in parentheses. A simple project could look like this 
```
if (true) {
   System.out.println("hello world");
}
```
Notice how everything is nice and neat compared to indentation based languages. 

#Variables
In java there are two types of variables, primitives and what I call 'wrappers'. primitives are the simple storage variables, things like 'int' , 'char', etc. and dont have any extra functionality. Wrappers are basically upgraded versions of primitives, with the drawback of being more costly. 
```
int i -> Integer i;
boolean b -> Boolean b;
double d -> Double d;
```
and more. If you aren't familiar with the types of variables, a boolean stores a true or false value, and is used in conditionals. A string stores words, and ints -> floats -> doubles are numbers with more decimals as you get further down the chain. A key thing about Java is that it is statically typed. That means you **have** to declare what type of var it is on its initialization, and it cannot be changed.
```
s = "hello world"; // no way for the compiler for it to know its a string

int s = 1;
s = "hello world"; // you cannot change what type a var is

String s = "hello world"; // perfect
```
Technically there is a 'var' type that inferences the type at initialization, but you still cannot change the type afterwords and in my opinion its safer to statically type so you can debug easier.

#Comments
Before I forget, because I forget to do this often, its good practice to use comments to annotate your work so you and others can go back later and know what you did, and how it works. I will try my best to comment my code so you know whats happening but no promises :)
```
//this is a single line comment

/* 
   this 
   is
   a
   multiline
   comment
*/
```
whenever you comment something out, it will effectively be ignored by the compiler so it is purely for convenience for later. I fall prey to forgetting to comment my code and it made it very hard to come back to an old project after a break.

# Classes
Now we are getting into the whole point of java. Classes are templates or blueprints for building an object. Each top level class needs to be defined in its own file but you can subclass (more on that later). You define a class with the "class" keyword
```
public class Main {
//your class code here
}
```
in that example notice I use the public modifier. There are levels of access that these modifiers give. If you make the class public anything can access it, if you make it private, only the corresponding file can access it. If you leave it blank anything in the same module can access it (more on that later). So I keep saying that each class is a template right? Here's what that means. Whenever you use a class you do something called instantiating it, which basically mean you are creating a local version of it and it becomes its own thing. You will never use raw classes, you will always use instances of it. Heres what creating an instance of it looks like.

```
//Example.java
public class Example {
   String s = "hello world";
}

//Main.java

public class Main {
   public static void main(String[] args) {
      Example e = new Example();
   }
}
```
So that was a lot of new things, all the stuff in Main.java is basically saying its the entry point for the compiler, without it the compiler wouldnt know wehre to start. I will get into all the modifiers for main in a second, but for now notice how I created an Example instance. You first use the class name as the type, you give the instance name, then new then the class name followed by parentheses. Each class has its own instance variables, so if i were to do 
```
Example a = new Example();
Example b = new Example();
```
each of those instances would have different references to the variables, that basically means you change one the other one is unaffected. Thats another point, in java, whenever you create a class, you are creating a reference to it in memory, so if you do something like this
```
Example a = new Example();
Example b = a;
```
if you changed something in b you are also changing something in a, so be careful of that when trying to copy things. A class has three parts, it has instance variables (variables that are specific to the instance), methods, and constructors. The most important one is the constructor. This is what makes you able to assign variables to classes. think of it as additional ingredients you add into your recipe, everyones going to do it differently. heres how they work
```
public class Example { // make a class like normal
   int i; // declare variables
   String s;
   public Example() { // you use the public modifier then the class name
     i = 10; // this is where you assign your variables
     s = "hello world";
   }
}
```
The real function of constructors is the fact that you can pass variables into it when creating a function
```
public Example(String s, int i) { // declare vars in the parentheses
   i = i; // assuming you already put int i; and String s; in the top of the class
   s = s; // not required but recommended for vars passed in and assigned to be the same name for clarity

}
```
The nice thing about constructors is they allow us to dynamically manage data. For example, you pass in a string to a constructor, you can use it wherever you need throughout the class. One drawback to this is you **must** put in exactly what types the constructor is expecting. You can access the variables with the 'this' reference, which is basically pointing to the parent class' reference, basically whenever you want to operate on a classes variables you can access them with this.variable
```
System.out.println(this.varible); // assuming this.variable is a string
```
next I want to talk about methods. If you are coming from another language you can think of methods as functions bound to a class. each method **has** to have an instance of the parent object to use it, unless you use the static modifier (more on this later). you declare method inside a class like this
```
class Example {
   public void test() {
      // your code 
   }
}
```
heres a rundown on what needs to be there. The method has to have an access modifier (technically optional but good practice), a return type, and the method name. With the same logic of constructors, you can pass data into methods in the parentheses. Methods can also access the parent class unless you add the static modifier. The static modifier is made for methods that, like the name suggests, are mostly static. The benefit is without it you would have to create an instance of the parent class first
```
Example.test(); /will fail

Example e = new Example();
e.test(); // normal call
```
but if you make it static just by adding the keyword static after the access modifier, it loses its parent class variables, but you can call it without making an instance
```
Example.test(); //works because test is static
```
Next lets move on to method overloading. This is a very useful mechanic for when you have code that you want to use different types on (think same logic for double and float). to do this you would declare the first method as normal, then just declare it again with a different type.

```
public void test(int i) { // fist type

}

public float test(float f) { //you can even change return types

}
```
This may look a bit scuffed at first but it is extremely useful when you are dealing with multiple data types or want dynamic code.

# Outro
Thats about it for the first chunk of the lesson, stay tuned for arrays and lists, and if you see anything wrong in my work make sure to notify me so i can fix it. thanks for reading and good luck on your coding
