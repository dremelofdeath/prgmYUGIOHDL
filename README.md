![prgmYUGIOHDL](docs/title.png)

Yu-Gi-Oh! Life Counter for TI-83 and 84 calculators I made when I was in 8th-10th grade (mostly around 2002-2003).

![Startup and shutdown](docs/startup.gif)

A relic of my past, shared for the world to see. The most recent version is 1.4.4 -- something I actually tracked as I shared the app widely in the local WV Yu-Gi-Oh! calculator-wielding nerd community, small as it was -- being a small fix from 1.4.3 I wrote while I was in my pre-calculus class in 10th grade for a friend of mine who was still using it. (For those curious, the bugfix/request was that there was no way to set the number of a player's counters without pressing the add key that many times.)

You can look at it in Unicode directly by [clicking this link](unicode/YUGIOHDL.txt). (Yes, I bothered to translate it from the TI's encoding to Unicode by hand.)

It was written entirely in TI-BASIC using only the calculator itself, as for the longest time I didn't actually own a GRAPH-LINK. I'm operating on some pretty archaic memory here, but this is what I recall about the version history:

Each version number signified a finished change that I would share if I found someone using an older version. In the 1.X.Y scheme, each X was a major "feature" (which reset the Y) and each Y was a bugfix or small change.

* 1.4 - Added flashing player counters.
  * The only reason I remember this is because I remember where I was (in the backseat of my mom's red Cavalier in a particularly long McDonald's drive-thru) when I discovered by accident that by drawing something on top of the display midway through the main loop caused it to 'flash' before it got blanked out by the next redraw.
* 1.3 - Match tracking, concessions, cycle rules types.
* 1.2 - Save games, roll dice/flip coins.
* 1.1 - Moved to using 'Horiz' mode for point display.
* 1.0 - Initial version shared.
  * The initial version used Output() instead of Text() for the score display.

Also worth mentioning is the fact that I had started work on a 2.0 version (called YUGIOHD2) on 03/21/03 (there is a clever comment hack at the beginning that dates it) while on a trip with my mom (again) to the Polo Club in Parkersburg where I was horribly distracted from whatever was going on and ordered way too much food that night. It ditched the 'Horiz' mode in favor of 'Full' mode graphics that drew layered menus to the screen with hotkeys. I obviously never finished it.

## How do I use this thing?

You will need to have either a compatible TI-83 calculator with a GRAPH-LINK cable or an emulator. Once you have one, deploy YUGIOHDL.8xp to it (put it in RAM) and run it from the menu normally. (I imagine that you *might* be able to use something like MirageOS to run it from Archive, but that is out of scope here.)

Many, but not all, of the keys on the TI-83 are mapped a function to control the game. Pressing them will do something -- some of them will cause a prompt to be displayed, others will pause the main loop to print some text, a few do something right away, and one (the Version Info key) will display text without pausing the main loop.

They are, in descending importance:
* + key: (Prompt) Add LP to a single player.
* - key: (Prompt) Subtract LP from a single player.
* × key: (Prompt) Add LP to both players.
* ÷ key: (Prompt) Subtract LP from both players.
* CLEAR key: (Immediate) Quit the game without saving.
* ^ key: (Immediate) Quit and save the game.
  * Note: This is implemented by simply not cleaning up the RAM variables the program uses when it exits, specifically D, E, F, G, H, L, M, P, and Q. (N *is* used, but is set to 0 when quitting normally.) There were two considerations about these choices. First, by doing it this way, you could resume if the program crashed. And second, the variables where chosen in such a way as to dodge commonly-used math variables. (The TI-83 *is* still a calculator, and while in school I used it as such.)
* STO⇨ key: (Prompt) Set the LP total of both players to specific values.
* DEL key: (Immediate) Reset the current game (LP and counters only).
* MODE key: (Immediate) Reset the match (LP, win count, and counters).
* 6 key: (Pause) Roll a six-sided die.
* 2 key: (Pause) Flip a coin.
* LOG key: (Immediate) Concede Player 1. (Player 2 wins.)
* LN key: (Immediate) Concede Player 2. (Player 1 wins.)
* Y= key: (Immediate) If the current game is fresh, cycle the game type between starting LP totals. Otherwise, does nothing. The available options are:
  * 2000 LP (Anime, Duelist Kingdom rules)
  * 4000 LP (Anime, Battle City rules)
  * 8000 LP (Standard)
* x⁻¹ key: (Immediate) Add a counter to Player 1.
* x² key: (Immediate) Add a counter to Player 2.
* SIN key: (Immediate) Remove a counter from Player 1.
* , key: (Immediate) Remove a counter from Player 2.
* COS key: (Prompt) Set the number of counters for Player 1.
* ( key: (Prompt) Set the number of counters for Player 2.
* TAN key: (Immediate) Clear all counters from Player 1.
* ) key: (Immediate) Clear all counters from Player 2.
* GRAPH key: (Display) Show version info.

## Known issues

There were like, maybe two things weird with the final version. The most glaring thing is that the intro sequence takes FOREVER after a cold start. (The intro does not play if you are resuming a saved game.) I had tried to fix this several times, but gave up. I believe I tried making a key to skip it, but the call to GetKey() slowed down the main loop too much. I admit that there is probably an easy fix for this (Text() calls are pretty slow IIRC), but I just never put the time into fixing it.

It's not so bad on a TI-83 Plus Silver Edition (which I had), but its processor is 250% faster than the TI-83 Plus (15 MHz vs 6 MHz), and as a result, the intro is particularly bad on a TI-83 or TI-83 Plus. It probably runs nicely on a TI-84, but I haven't tested it.

Second, forcing the user to press Enter after doing things like rolling a die or winning the game is confusing because it stops the main loop. It is easy to not realize that the next action is "press Enter" because the display for "need to press Enter" is literally one pixel wide and at the top right of the screen, in the corner. Also, rolling a die doesn't display its text on the drawing screen, for presumably no good reason -- it could hypothetically even do what the Version Info does and use Output().

Oh, and yeah -- the last frame of the intro animation (from 7900 to 8000) takes way longer than the others do for unknown reasons (the game state is setting up?). I never bothered to solve that either, namely because the code is a tightly wound blob of Gotos that gleefully and unceremoniously jump both into and out of If statements with complete disregard to sanity.
