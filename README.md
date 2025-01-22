# opwolf.zip MAME Cheat
# Submachine Gun does not stop

I like this game a lot. Used to play it in C64 and at the arcade.
I found myself playing it again and decided to learn how it works at a deeper level.
It was frustrating not to be able to shoot faster - not even 

The game is from 1987 and works with the Motorola 68000 with 16/32bit operations.


# How to install

1) The Cheats Plugin needs to be turned on.
   1a) It's in the mame.ini file. "Core Misc Options > cheat". Flag needs to be set to 1 (instead of 0).
   1b) You can also turn it on from the user interface. "General Settings" > "Plugins" > "Cheats". 
2) Define MAME/Cheats folder
   2a) Defined in the mame.ini configuration file (option is "cheatpath"). If it's not you can create one configuration file running MAME from the command line interface, "mame -cc".
   2b) If you can't find it, you can define the MAME/Cheats in the User Interface. General Settings > Configure Folders > Cheat Files.
3) Suggest to find the Pugsy Cheat list. It has thousands of cheats for this and other games. It didn't have this cheat.
4) Drop this .XML file as is in the predefined folder for MAME Cheats. It will append to other cheats.

# More avenues of exploration

- Instead of touching the timer just find the subroutine that enables the weapon. I could not find it.
- Fix the buggy counter. I have an idea where it is. Took extensive notes.
