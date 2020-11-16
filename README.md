# Mouse and Keyboard Detection for Consoles

This repo is intended to be a open library to aid fair online gaming.

The purpose is to create algorithms that can be embedded in the game code
so it doesn't matter which console the game is published for,
as long as the controller is joystick based.

Everyone is more than welcome to help.

# Motivation

I've been playing R6 Siege on the PS4 but I noticed there are a lot of players
using mouse and keyboard.

I searched for solutions all over the Internet and I've find out this
is actually a very common problem in many Titles on different consoles.

Videogames are supposed to be fun, gives us inspiration and a good time.

Since I'm not a game developer (yet) I don't know exactly where these
algorithms should be applied nor the programming language they should be
written, probably C++ or Java?.

For now, I'll be programming these experiments with Javascript since if one
of the languages I know the most and because it can manage time which is
needed for this to work.

# Real uses

Since all games are different, these algorithms should be adapted, of course,
a open-generic version will be published.

In the perfect world cheating players are banned forever but in ours,
I think a better approach would be to silently force MnK users to play
with another MnK users, avoiding unbalanced game experience for
console players.

This, along with a good ranking algorithm (planned for later) could make online
gaming a much better experience for everyone.

## Main/Wrapper Function Explanation

C  = 'Console Control'
T  = 'Trigger' (L2,R2)
J  = 'Joystick'
M  = 'Mouse'
K  = 'Keyboard'

S  = 'Cheating Suspected Point'
SS = 'Cheating Suspected Point (Double)'

The function works by gathering "S", these are issued by small functions that
constantly tests if MnK (Mouse and Keyboard) are used.

When the user gets enough S the function sends a true signal.

The amount of S required depends on the $tolerance given by the caller
along with other parameters

## Algorithms

### Lack of Bezier Curve Input

J input tends to generate Bezier Curves.
M input tends to generate Pointed/Closed Angles.

In proporion, if input has a strong tendency of generate angles
rather than curves, an S is issued.

### Lack of "V" Parabola

Moving J along it's diamether necessarily generates a "V" Parabola. 
This is hardly achieved with M.

Issues SS.

### Player Movement in Cardinal Angles
In K there are only 8 possible movement angles based in 4 basic angles:
[W] =  90°
[A] = 180°
[S] = 270°
[D] =   0°

Generate only these 8 angles with a J is almost impossible.
Issues SS.

### Too many Directional Input
J can't get >1 directions at the same time (e.g. left and right),
if >1 inputs are received an S is issued.

### Inconsistent Maximum Acceleration and/or Speed

J has a maximum "turning" speed at certain sensibility,
this speed should NOT be exceeded.

J has a maximum "turning" acceleration rate at certain sensibility,
this acceleration should NOT be exceeded.

NOTE: Max speed and acceleration can be calculated when the player is
turning around while walking off-action.

### Lack of Inertia

Some M adapters make little "jumps" when aiming down sights,
moving from 0 to x and from x to 0 without accelerating issues an S.

Some bad adapters generate "jitters" (constant random direction input).

One single jitter issues SS.

### Sight Sensibility Too High

If the substraction (difference) of normal and down sight sensibities
is too small, an S is issued.

This is because aiming in a console requires less sensibity down sight but
it's not a problem while using a M.

These numbers are based in max turning speed (inconsistent with M).

This one depends on the Title.

### "Pinch" Frag

Moving the cursor faster than the maxspeed calculated above, stop, fire and frag
it's a common patter while using a M.

Pinch fragging is more hard to get with a J.

### Input Sequence Too Fast

MnK let the player to click and type pretty fast compared with the controllers.

This is specially true with "R2" trigger-type buttons because they have
longer span.

M small spans and hand position let players to send input with great speed.
K small spans and hand position let players to send input with great speed.

### Lack of Pressure Sensibility

All buttons in a console control have certain ammount of pressure sensibity,
MnK doesn't have any.

If any input (walk, crouch, jump, shoot) are sent without proportional sensibity,
or this "sensibility" is emulated (always the same) an S is issued.

### Score

In certain games such as Tom Clancy's the score tends to be balanced.

If the score of a player is just too different from everyone else,
we can issue an S.

This is one of those functions than need to be customized per game.
