# When to and when not to use arrays vs lists
Hey, I'm bored so i guess ill just write this thing.

So if you've come from a language like Lua or i believe Python, maybe even c, you probably know a lot about arrays. but from the people I've taught, you might not know about Lists. Arrays are commonly used in other programming languages but in java they are a bit different. In java, you declare an array and at initialization you specify its size, and that size is its final size. 
```
String[] stringarray = new String[10]
``` 
That's it, your array is now forever 10 long.
Arrays are okay, and very useful for fast indexed based searching, but if you need anything more flexible you may want to look at Lists or even HashMaps. 

#Arrays
Arrays are technically an object, but in a very minimalistic way. They provide nothing but the length and what is at an index. They can also directly store primitives and are a decent amount faster than Lists if you use them right.

# Lists
Lists are basically upgraded arrays but with a bit more overhead. Lists are a lot more flexible and some types (ArrayLists which we'll get to later) can even expand and contract at will. The main caveat is they store primitives wrapper classes (Integer vs int, etc.) instead of the base primitive, this gives it slightly more overhead from boxing and unboxing primitives to wrappers. Also, Lists can store more than jsut primitives, you can have a list of classes or other objects. 
```
ArrayList<Integer, Example> // where Example is a defined class
```

# Wrappers
if you didn't see my post earlier today, wrappers are basically, like lists, an upgraded version of primitives, with more functionality but less efficiency. TLDR if you don't need the extra methods, use primitives

#Use cases of arrays
Ok, so arrays can be very efficient especially for multidimensional arrays
```
String[][] s = new String[] {
   {"hello", "test"},
   {"nested", "tables"}
}
```
this is where arrays shine. That and when you have an extremely static dataset.  for example in my latest project I used an array just because I knew there was a max bound so it was more efficient to use an array. Arrays have really efficient look ups with indexing. 

# Use cases of Lists
Ok so this is going to be a lot longer than the arrays. There's this special type of list that gets used the most often across the most projects and that is the ArrayList. The most versatile tool in java in my opinion, this thing is a beast. It has the benefits of storing anything that comes from lists, and it has ironic ability to do the opposite of arrays and resize itself dynamically. This thing is a staple of my bigger projects and the reason I was able to get a project I had been working on for month in Lua done in days in Java. These things are mid performance, versatile godsends. They can be used in almost anything dynamic and even in semi-dynamic situations they aren't that costly for the benefits they give. The other type of list I've used is the linked list. These are really useful for dynamic insertions where you need a specific order. I was able to reliably iterate through it for a project i don't really remember but it was pretty useful.

# When to use Lists Vs Arrays
Basically when in doubt or when performance isn't stretched too thin to be a concern I would recommend using Lists. The only exception is multidimensional arrays, that's where it starts to get costly, especially in game dev where frames are called 60 fps and it accumulates. That's when arrays start being worth it. The key is with arrays, you want to get as close to the surface as you can otherwise it becomes debugging heck.

# Bonus: HashMaps
HashMaps are a great mix between Lists and arrays. They have efficient look up and are designed for key value pairs. if you need a quick look up table with a semi static dataset, use HashMaps.

# Outro
anyways, thanks for coming to my ted talk lol
