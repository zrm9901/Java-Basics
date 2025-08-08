# Intro
Sup im back, today I'm going to be talking about a fun one, generics :)

# What are generics?
So generics are basically the idea that you can pass any type into a class or method. This can be useful if you have repeated code or just dont particularly know what the type is going to be until runtime.

# What does that mean though?
Basically, you take a place you would normally define a type and replace it with this
```
public <T> void iter(T[] arr) {
    for (T obj : arr) {
        System.out.println(obj);
    }
}
```
notice how I added the T after the access modifier, and then again where a type would go, the T is basically a placeholder for an actual type, and yes, I do believe it has to be T specifically. The cool part is this doesnt have to be limited to methods, you can do it with classes too
```
public class Example<T> {
    public T myVar; //public for later example
    //constructor...
}
```
So basically what just happened there is we told the compiler that this class has a placeholder type, and then we declare a var of that type. One thing to note is that this type is actually declared on object creation like so
```
Example<Integer> ex = new Example<>();
```
Btw the type **has** to be a non primitive, similar to ArrayLists and HashMaps, in fact they actually use generics to avoid casting. Oh yeah, one neat feature of generics is that you avoid casting all together when retrieving a T var
```
Integer i = ex.myVar;
```
This just works without casting. If you want to limit the types that T can be you can do that with the extends keyword like so
```
public class Example<T extends Number> {
    public T myVar;
    //constructor...
}
```
This limits it to just types that are considered numbers. My favorite use of this is basically recursively bounding a type, this is an example from one of my projects
```
public interface Entity<T extends Entity<T>> {
    public Vec getPos();
    public Double getRad();
    public Utils.Quadtree<T> getPar();
    public void setPar(Utils.Quadtree<T> par);

    default void Draw(ShapeRenderer renderer) {
        Double r = getRad();
        if (r != null) {
            Vec p = getPos();
            renderer.circle((float)p.x, (float)p.y, (float)(double)r);
        }
    }
}
```
this basically makes it so that T **has** to be a type that implements entity, and then that T can be used in other things that implement entity. For example, my quadtree logic looks something like this
```
public static class Quadtree<T extends Entity<T>> {
```
and this can do some pretty cool things, like basically accepting any type of class implementing entity. Its a bit confusing at first, but basically in my quadtree class im saying that T has to be something in the Entity's T, and the Entity's T has to be something that implements the Entity interface. This basically makes it so that you can use the quadtree with any class implementing Entity, and just makes your code cleaner in general. Heres how to actually create an instance of a class implementing Entity
```
public class Unit implements Utils.Entity<Unit> {
```
notice the semi-recursive nature of it? Same type of logic as the base Entity class. I'd reccomend playing around with it to figure out what stuff works and what doesnt, but the best use I've found for this is if you have a Utility class you want to reuse in multiple projects that you want to build off of. For exmample in Unit I can just add more stuff on top of Entity and still use Unit in stuff expecting Entity without casting. If this is confusing to you, dont worry it was to me too at first, it takes a bit to understand but once you do you can use it pretty much anywhere.

# Outro
Well thanks for reading this and hopefully you understood some of it, have fun coding :)