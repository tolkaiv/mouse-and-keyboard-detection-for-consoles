# Mouse and Keyboard Detection for Consoles

Game Developers say "XIM" or MnK adapters are impossible to detect but I think they are detectable.
This repo is intended to be a open library to aid fair online gaming.
Everyone is more than welcome to help.

# Motivation

I've been playing R6 Siege on the PS4 but I noticed there are a lot of players using mouse and keyboard.
I've been searching for solutions over the Internet and I've find out this is actually a very common problem
in many titles on different consoles.

Videogames are supposed to be fun, gives us inspiration and a good time.

Since I'm not a game developer (yet) I don't know exactly where these algorithms should be applied nor the programming language
they should be written, probably C++ or Java?.

For now, I'll be programming these experiments with Javascript since if one of the languages I know the most and
because it can manage time which is needed for this to work.

# Real uses

Since all games are different, these algorithms should be adapted, of course, a open-generic version will be published.

In the perfect world cheating players are banned forever but in ours, I think a better approach would be to silently force MnK users
to play with another MnK users, avoiding unbalanced game experience for C players.

This, along with a good ranking algorithm could make online gaming a much better experience for everyone.

## Main/Wrapper Function Explanation

C  = 'Console Control'
T  = 'Trigger' (L2,R2)
J  = 'Joystick'
MK = 'MnK'
M  = 'Mouse'
K  = 'Keyboard'

S  = 'Cheating Suspected Point'

The function works by gathering "S", these are issued by small functions that
constantly tests if MK are used.

When the user gets enough S the function sends a true signal.

The amount of S required depends on the $tolerance given by the caller along with other parameters

## Algorithms

### Lack of Bezier Curve Input

J aiming    always create bezier curves in the screen, otherwise S is issued.
J movements always create bezier curves in the player, otherwise S is issued.

### Too many Directional input
J can't get 2 or more different directions at the same time (left and right),
if more than 1 input is received, S.

### Inconsistent maximum acceleration or speed

J has a maximum speed at certain sensibility, this speed should NOT be exceeded.
J has a maximum acceleration rate at certain sensibility, this acceleration should NOT be exceeded.
NOTE: Max speed and acceleration can be calculated when the player is turning around while walking off-action.

### Lack of Inertia

Some M adapters make little "jumps" when aiming down sights, accelerating from 0 to x
and from x to 0 without accelerating gives us an S
Some bad adapters make M "jitters" (constant random direction input), if one single jitter is detected, S.

### Perfect aiming

When a player aims ~45deg or more and get a frag in a very short period of time without "preshooting", and S is issued.
With a J, this can only be achieved a with a great ammount of luck.

### Great speeds at Single Fire

M let the player to click pretty fast compared with the T, in many games "R2" buttons are used
because they have longer span to be activated and are more similar to the trigger of a real weapon
M small spans let players to shot with more ease and speed, when the player has the habit of
fast-shot we give him an S

### Lack of control sensivility
All buttons on a console control have certain ammount of pressure sensivity, M and K doesn't have any.
If any input (walk, crouch, jump, shoot) are sent without proportional sensivity, an S is issued.
Also, if sensivity is "unnatural" (coded) or it's always the same, S is issued.

### Score

In certain games such as Tom Clancy's, "Rambo" style players are mostly impossible,
if the score of a player is just too different from everyone else, we can issue an S.
Of course, this is one of those functions than need to be customized per game
