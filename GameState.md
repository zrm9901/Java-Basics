# Intro
Hey, someone suggested I talk about shared state and its actually a really in depth topic to get into, so here goes, sit back and enjoy the ride.

# What is a shared state or game state?
So in most games, especially in object oriented languages, there is this idea of a shared state, almost like a hub of variables and functions the whole game can use, and its neccessary for the game to communicate with itself. This topic gets into modules, incapsulation, and how objects are handled. A shared state is almost akin to global variables in python, lua, etc. but in my last article i said Java doesn't really have local vs global variables, so how does that work? Well you basically make a class specifically for that purpose that you pass around throughout the game. In my last article I talked about how classes are basically containers, and they dont actually have to contain methods, they can be purely for object storage. For example, in one of my recent projects, I needed a way to share information between screens (basically different levels in LibGDX) and I was able to do something like this in the main constructor, then pass it into the other classes to acccess it, but im getting ahead of myself. A GameState class can look something like this at its most basic form
```
public class GameState {
  private int score;
  private boolean won;
  //continuing on
}
```
But you may be asking, why private, and this gets into a deeper concept that I haven't explained yet, encapsulation

# Encapulation
So, you may be looking at that private modifier and be wondering why or even what it is. That modifier for those who dont know makes it so that you can only access the variable from inside the class
```
GameState g = new GameState();
System.out.println(e.score); // wont work because score is private
```
Well now you may be wondering how you can access it if it can only be used from within the class, that gets into the idea of what are called "getter" and "setter" methods. These methods are the thing that makes encapsulation encapsulation. These are public methods from inside the class that can be used to expose the vars, but the key difference between raw public variables and getter setter methods is you gain a lot more control over your data and what can access it. For example, you could add a validation check to make sure the thing trying to access it is supposed to, and this is a key part of OOP. This makes it a lot more secure and modular, as you can output data in specific formulas and can basically modify it however you want to protect your data. If you ever go into professional software development, this will be a big part of it so you can protect your companies data, especially for closed source projects. 

# But why?
So the thing is, that is whats considered the "best practice", and it may sound stupid at first, but unless you need raw variables its a good practice to get into. I am guilty of not doing this but lately I have been making a concious effort to. The benifit is, as I've said before, you get a lot more control over your data. If you are doing a quick prototyping or a simple project you honestly dont need to do this, but think about a closed source game. Think Madden, if you had access to the raw variables (and this isnt exactly how it works), you could wreak havoc upon the game with hacked clients. Now do you see the use case?

# How to effectively do it
```
class Example {
	private String name;
	private int score;

	public Example(String name, int score) {
		this.name = name;
		this.score = score;
	}
}
```
right now, nothing can access these variables from outside the class. So heres what we want to do to make sure score is never directly updated and cant be manipulated in a way we dont want it to
```
//this is in the Example class
public void addToScore(int num) {
	this.score += Math.min(num, 50);
}
```
See the utility of it? we can directly control whats happening. Same thing applies to getting data out.
```
//still in Example
public int getScore(String password) {
	if (password == "1234" ) {
		return this.score;
	} else {
	return null;
	}
}
```
We can also gatekeep the info or do just flat out change data to obscure what is actually there.

# So how does this apply to GameStates
Basically, unless you are doing a simple prototype or a very very simple game, I would reccomend data encapsulation, it keeps things easier to understand as you only have one output / input, and makes it harder to exploit your game. Anyways, now that that tangent is over, lets get back to how we can create our gamestate. Look back at the gamestate code i gave earlier, now we can add getters and setters to the logic
```
public class GameState {
	private int score;
	private boolean won;
	
	public int getScore() {
		return (this.won) ? this.score : null;
	}
}
```
If you are curious what that was, look here on [W3Schools](https://www.w3schools.com/java/java_conditions_shorthand.asp). Anyways now that we know about encapsulation, how are we actually transferring the data around? Thats where the idea of class instances being object comes in handy. Take for example this block of code.
```
public Example e = new Example();
```
Notice how example is a type? that means we can pass that into methods and constructors. This is one key thing I've learned in my latest project, you want to have almost everything shared between classes in one or two gamestates. That ways instead of having this 
```
public Logic(Main game, Main.GameState state, int players, int handsize, int heapsize, Skin skin, TextureAtlas cards, float overlap)
```
you have this 
```
public Win(Main game, Main.GameState state)
``` 
(actual code from my first class to my classes now). Now you use the getter setter methods to manipulate data and only have to keep track of one object instead of 20. Anyways, now that I've shown you a gamestate in theory, what does it look like in practice? This is my gamestate from my last game, and yes it doesnt use private but that was becouse of convinience
```
public class GameState {
        HashMap<Integer, ArrayList<Card>> hands = new HashMap<>();
        int[] scores= new int[4];
        Skin skin  = new Skin(Gdx.files.internal("skin/uiskin.json"));
        TextureAtlas cards = new TextureAtlas(Gdx.files.internal("skin/cards.atlas"));
        boolean done = false;
        int handsize;
        int heapsize;
        int oghp;
        int oghd;
        int players;
        float overlap;
        int chips = 100;
        String[]  names = new String[] {
            "zach", "test", "jeff", "mabel"
        };
        
        public GameState(int players, int handsize, int heapsize, float overlap) {
            for (int i = 0; i < players; i++) {
                scores[i] = 0;
            }
            this.players = players;
            this.handsize = handsize;
            this.heapsize = heapsize;
            this.oghd = handsize;
            this.oghp = heapsize;
            this.overlap = overlap;
        }
        public Logic play() {
            return new Logic(Main.this, this,this.players,this.handsize, this.heapsize, this.skin, this.cards, this.overlap);
        }
        public Results results() {
            return new Results(this, Main.this, this.overlap, this.players, this.skin, this.cards);
        }

        public Win win() {
            return new Win(Main.this, this);
        }

        public void dispose() {
            this.skin.dispose();
            this.cards.dispose();
        }

        public boolean tick() {
            this.handsize--;
            this.heapsize++;
            this.chips += 100;
            if (this.handsize > 0) return true;
            return false;
        }
    }
```
FYI this is in LibGDX so the methods are creating new screens / levels. Its a bit complex, but it will lower the rest a lot. The cool thing about gamestates is you can save them to a file, which basically saves your progress in one easy step (I will cover files in a different article).

# Moral of the story?
Use encapsulationa and a shared gamestate class to securely store and pass around information from class to class, because thats really the only good way to do it. 

# Bonus tip
When you store a class instance in another one, its a reference so updating one will update **all** places it shows up, which makes it even more versatile.

# Outro
thanks for listenign, if you have any questions or concerns leave a comment because I have this feeling of dread im forgetting something. ttyl :)
