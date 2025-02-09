# Operation Wolf  Cheat (opwolf.zip)
## Cheat - Submachine Gun does not stop

I like this game a lot. Used to play it in C64 and at the arcade.
I found myself playing it again and decided to learn how it works at a deeper level.
It was frustrating not to be able to shoot faster - not even using turbo buttons (needed in my case).<br>
<br>
![alt text](itempowerup.png)
<br>
<br>
The game is from 1987 and works with the Motorola 68000 with 16/32bit operations.


# How to install

###
1) The MAME Cheats Plugin needs to be turned on.<br>
   1a) It's in the mame.ini file. "Core Misc Options > cheat". Flag needs to be set to 1 (instead of 0).<br>
   1b) You can also turn it on from the user interface. "General Settings" > "Plugins" > "Cheats". 

###
2) Define MAME/Cheats folder<br>
   2a) Defined in the mame.ini configuration file (option is "cheatpath"). If it's not you can create one configuration file running MAME from the command line interface, `mame -cc`.<br>
   2b) If you can't find it, you can define the MAME/Cheats in the User Interface. General Settings > Configure Folders > Cheat Files.

###
4) Suggest to find the Pugsy Cheat list. It has thousands of cheats for this and other games. It didn't have this cheat.

###
5) Drop this .XML file as is in the predefined folder for MAME Cheats. It will append to other cheats.

# How to Debug
###
Launch the MAME debugger.<br>
I used `-speed 0.2` to keep speed at a minimum in case I needed to let it run. I used: `mame -window -debug -speed 0.2 opwolf`

# How it works
###
Its straightforward. The instruction that checks for the timer is:<br>
<br>
`00A35A  dbra    D0, $a34a         51C8 FFEE`<br>
<br>
It's located in `$00A35A`. The command `dbra` - Decrease D0 and branch to `$a34a` where it will quickly return to decrease again.<br>
<br>
I change this to <br><br>
`00A35A  bra     $a34c        60F0`<br>
`00A35C  nop                  4E71`
<br><br>
Using the Cheat Engine:<br>
`<action>maincpu.mw@00A35A=60F0</action>`<br>
`<action>maincpu.mw@00A35C=4E71</action>`
<br><br>
With this I'm forcing the CPU to branch and go to an offset of `$F` bytes above but without decrementing the counter. The `$F` bytes is because the `DBRA D0, $A34A` jumps $F bytes so `$a35a - $a34a = $F`.<br>
Because this created a "shorter" instruction other than the double word instruction which it replaces, I added a 2 bytes `nop` - `4E71` so it doesn't mess up with the remaining code.<br>
<br>
# More avenues of exploration
<br>
- Instead of tampering the timer just find the subroutine that enables the weapon. I could not find it.<br>
- Fix the buggy counter which shows up for a few seconds until it gets stuck at 0. I have an idea where it is.<br><br>

![alt text](https://i.ibb.co/8XKJGXy/TimerBug.png)<br>
<br>
At $C08D88 - The memory area that holds the byte or word with the upper part of the timer count.<br>
At $C08E88 - The memory area that holds the byte or word with the lower part of the timer count.<br>
<br>

# Some of my findings in case it's interesting out there
<br>
- Timer for the Machine Gun changes every 90 frames. There's a `BSR $544` (Bitscan Reverse) at $000522.<br>
- The number of frames is around here: $100932.<br>
- The number of shots taken during a game is around here: $100CA6.<br>
- When you get the Power Up the text is coming from this memory area: $1027F0. Adding a Breakpoint anywhere here `17da 17f2 a29e a2b4 a2de a2c8` and you will catch it when it renders.<br>
- The timer counter shown on the screen is here $100562 either in byte or word.<br>
- A Breakpoint at $001128 will help and catch the exact instruction that helps debug what happens when "a shot" is taken - or when an interrupt happens.<br>
