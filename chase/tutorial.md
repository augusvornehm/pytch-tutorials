# Chase game

In this tutorial we will make a game where you control a bird that
chases a star around the stage.

In this project you will learn about making multiple Sprites, making
them move, and detecting when they come close together.

{{< run-finished-project >}}

### Detailed credits

{{< asset-credits >}}


---

## Setting up Pytch

As usual we have to make sure the Pytch add-ons are available. This
means writing an "import" line at the top of our program.

{{< commit import-pytch >}}

## Setting up the stage

Let's start by setting up the stage for the game. There is a nice
backdrop of some clouds so we define a _Stage_ that will look good
when the bird is flying around.

Making a Stage in Pytch is rather like making a Sprite, we have to
create a new "class" that says it is a Stage. Then we can define
_backdrops_ as part of that stage.

You can only have one class in your project for the Stage (there's
only one stage, no matter how many Sprites there are), but it can have
different backdrops if you need that. This project will only have one,
so Pytch will automatically select it when the project starts.

{{< commit add-Stage-with-backdrop >}}

You can try the project now — click the *green flag* to run your
program — and you should see the clouds as a nice backdrop.


## Making the Bird character

Next we will make the bird character that the player will control. The
first step is to make a new Sprite and give it a costume. The costume
we are using is already in the project assets so we just need to make
sure Pytch knows which costume is for this Sprite.

{{< commit add-Bird-with-costumes >}}

### Moving the bird to its starting position

The game will start when the player presses the green flag. As soon as
that happens I want the bird to move right to the middle of the
stage. So I *def*ine a new bit of code that will run when when the
green flag is clicked. I'm going to name this "start", but the name
isn't really important, it's the hat block that controls when this
will be run.

{{< commit initialise-bird-position >}}

If you try this code now — click the green flag to run your program —
you will see that the bird costume is actually rather too big for the
stage. I can fix this by telling the sprite to pick a smaller size
before it appears, using the ``set_size`` command. The number given to
``set_size`` controls the size, from 1 (full size) to 0 (so small you
can't see it).

{{< commit set-bird-size >}}

### Making the bird move

The next job is to let the bird move around the stage so that it can
chase something. I will let the player use the arrow keys to move it
around.

Before I do that I am going to set up a variable that I will use to
control the speed the bird moves at. I don't _have_ to set my project
up this way but it will make it easier to change how fast the bird
moves if I want to.

I create a variable in the Bird by adding this line after the costumes:

{{< commit add-bird-speed-variable >}}

Now I will make some code to move the bird to the right. Once this is
working nicely we can copy it to move in the other directions. I want
this code to run whenever the player presses the right-arrow key, so I
make a script (I have to give it a name, Python needs that), and I
mark it with some code that lets Pytch know this script gets run when
the right-arrow key is pressed.

I can make the Bird sprite move using the ``change_x`` function.
Notice how I'm using the ``speed`` variable that's defined as part of
the Bird to say how far the bird will move.

{{< commit bird-move-right >}}

If you try the project now then you will be able to make the bird move
in one direction by pressing the right arrow key. Excellent, we are
well on the way!

Now we should add three move methods for the other three
directions. We can make the bird move to the left by changing the
``x`` value in the negative direction (just put a minus sign in front
of the variable). To move up and down we change the ``y`` value of the
Sprite.

One thing to watch out for is that each method needs to have a
different name, or the later ones replace the earlier ones in the
Sprite (which would mean there is only one of them left, and some
directions wouldn't work!).

{{< commit move-bird-three-other-directions >}}

If you try the project out now then you should be able to move the
bird in all four directions.

If you think the bird is moving a little slowly then you could try
changing the number we store in the ``speed`` variable. Because each
of the moving methods uses the number they will all change ``x`` or
``y`` by the amount stored in the variable. Remember, you have to
build and run the project after making any changes.

## Giving the Bird something to chase

Now that the bird can move around the screen let's give it something
to chase. Make a new Sprite and set its costume to the "star"
image.

{{< commit create-Star-with-costumes >}}

### Moving the Star randomly

We want the star to move about, but not under the player's control
(that would make it a bit easy to catch!). The plan is to pick a
location on the stage at random and then head towards it for a
while. Once the star gets there we will pick a new location and move
towards that, and keep this up until the Bird catches it.

To start with we want to appear on the stage a little bit away from
the Bird. The star costume is also kind of big so I'll set the size at
the same time. This should all happen when the green flag is clicked.

{{< commit add-star-move-script-part-1 >}}

I can have the star keep moving as long as the game is running, just
moving from place to place. When it gets caught we can hide it for a
while and then have it reappear at whatever position it's randomly
moved to so that the bird can continue to chase it.

To make some code that repeats I can use the Python ``while``
command. A ``while`` loop has something it checks each time, and it
will repeat its code as long as that thing is true. Normally you put
something that could be different each time it's checked (for example,
something that checks the ``x`` position of a Sprite) but if you want
the code to repeat forever you can just write ``True`` there and it
will always be true!

Notice the ``:`` at the end of the line. Like classes and script
definitions, this loop will have more commands inside it (which will
be the stuff to be repeated over and over).  For now, we'll just make
sure the Star is visible, because we know that our program soon will
hide the Star when it gets caught, and we want to make sure it's
visible at the start of each movement across the stage.

{{< commit add-infinite-loop >}}

Now I want to pick a random location on the stage. Python has a way to
make random numbers, so if I just pick two `x` and `y` numbers they
can be the destination for the star to move towards. When the star
gets there the loop will repeat and a new destination will be chosen.

The ``random.randint()`` function in Python selects a random number
between two points (so ``random.randint(10, 20)`` will pick a number
between 10 and 20). The Pytch Stage has x coordinates from -240 to
+240, so we'll choose a random number in that range for `x`.  This
will allow the Star to stick out beyond the edge of the stage
slightly, but that's OK.  And we'll do a similar thing for `y`, which
we'll choose between -180 and +180.

Using this I set up two variables inside the loop:

{{< commit select-star-destinations >}}

Next I want to move the star to that point. But I don't want to just
have it hop there, because that will mean the star jumping around the
stage as fast as possible. It will just flicker around and the Bird
won't have any chance to catch it. Instead I want it to move smoothly
to the point over a few seconds.

Scratch has a handy block called "glide to" which does this. Pytch has
the same thing, in a method called ``glide_to_xy()``.  We'll use this,
asking Pytch to make the Star take 2 seconds to glide to its
destination.

{{< commit call-glide-to >}}

There is one bit of housekeeping to attend to. The ``random``
functions are not actually built-in to Python, like Pytch they are a
kind of add-on (called a "library"). We need to add a line to the top
of the project to get them included:

{{< commit import-random >}}

If you try the project out now you'll see it's almost complete! The
Star glides around and the Bird can chase it. The only thing we need
to do is have something happen when the bird catches the Star!

## Catching the Star

Pytch has a method called ``touching`` that checkes to see if two
Sprites are touching each other.

We could check this every time the Bird moves to see if it has caught
up with the Star. But we would also have to check it every time the
Star moved, in case the bird was standing still and the Star ran into
it!

Instead I will write a Script that does nothing except continuously
check to see if the Star has collided with the bird. That way no
matter which Sprite was moving we will detect it in one bit of
code. I'll use the same idea that the ``play`` method had to keep
checking.

This script should start up as soon as the game begins, so I give it
the same "green flag" hat as the ``start`` and ``play`` methods.

{{< commit bird-checks-for-catching-says-gotcha >}}

If you try the project now you'll see that this is _almost_ perfect,
but there are two problems:

### Hiding the star when it's caught

After the bird catches the star we want the star to vanish for a
time. Otherwise after the bird says "Got you" the star will be very
nearby (maybe even still touching the bird!) and it's too easy to
catch it.

One way we could do this is to have the Star also constantly check to
see whether it's been caught, and hide itself as soon as it
is. Because the loop in ``play`` starts with ``show`` the Star will
reappear when it has finished gliding to its destination, and the
game can continue.

{{< commit star-checks-for-catching >}}

### Fixing a start-up bug

You might notice that the bird catches the star as soon as we start
the program.  This is because the star hasn't had a chance to move
before the bird checks whether it's caught the star.  We'll work round
this problem by making the Bird wait for a tiny delay before starting
checking for touching the star:

{{< commit delay-detect-star-loop >}}


## Adding some sound

To make the game a bit more fun we'll have the bird make an excited
noise when it catches the Star.

We can add _sounds_ to a Sprite using the Sounds variable.

{{< commit add-bird-sounds >}}

And we can add a line to the bird's ``check_catch`` method that plays
this sound when the star is caught. The script will go on to the next
command immediately, but because of the ``say`` command it will
wait for about the right amount of time for the sound to finish.

{{< commit play-caught-sound >}}


## Add instructions

To tell the player how to use our game, we'll add some instructions at
the top of our code.  We'll do this using a Python *comment*, which is
a part of your program meant just for human readers — Python ignores
it.  In Python, a line starting with the `#` character is a comment.
We'll add a short explanation of the aim of the game, and how you move
the bird, using a comment line:

{{< commit add-player-instructions >}}


## Challenges

1. You could add a score to the game so that the bird says things like
"Got you 5 times". Create a variable in the Bird sprite, and add one
to it every time the star is caught. You can use a variable called
"score" in the say command like this: ``self.say("Score is " +
str(score))``. The ``str`` command converts a number to a string so
that it can be joined with another string using ``+``.

2. You could add a second Sprite to chase, which moves faster.

3. You could make the game harder by having the Bird slow down a
   little after it catches 5 stars (by changing the ``speed``
   variable). After a while you might like to have the Bird speed up
   again (maybe after some time passes, or after it catches the star
   again).