# BumbleBot

IP to connect to Wombat: https://192.168.125.1/


## Oct. 28
- See [Whiteboards Oct 28](Whiteboards/Oct%2028).
- Brainstormed behaviors for our robot to carry out, chose Pollination/Waggle Dance.
- Outlined goals and steps needed in programming.
- Separated potential goals into:
  -  Finding Flower & Basic Dance
  -  Actually Pollinating Somehow
  -  Making a Second Robot Capable of Following the First's Instructions

## Nov. 4
- Started editing the original source code.
- Got the Search and Flower modes working with [Search Flower V1](search%20flower%20v1).
- Then we tried to integrate our previous Braitenberg-style light seeking behavior to improve the flower-finding abilities. This proved tricky. See [Search Flower Braitenberg](search%20flower%20braitenberg).
  - Experimented with higher and lower light thresholds to see if that would fix the problem, ended up choosing to angle our sensors outwords (like the red robot in Robot Ethology Part 1, see [this photo]().
  - We then tried removing the Avoid Objects function and it started working really well. It did do a really cute pause before actually "attacking" the flower though...
  - Finally we got it working, turned out that we were once again passing an _int_ instead of a _float_ and had the numbers flipped so it would be avoiding light instead of seeking it (these were the same issues that we were having in RoboEthology Part 2, we copied the code from Lev's notes which did not have the final corrected code... oops...
- Did some research into how the beacon module will work, started foundations for integrating beacon code. Developed the idea of differential (braitenberg-style) navigation using the beacon system alone.


## Nov. 11
 - Continued working on waggle mode
 - To test, we made "return mode" = robot turns 180 degrees, then starts the timer and drives, then goes into waggle mode
 - Realized we can't call waggle mode until it's returned, but for this test we need to call it at a random point
 - Also learned about for loop formatting -  we had been declaring int i within the for loop, which KIPR didn't like
 - The robot was moving around and switching back and forth between returrn mode and waggle mode
    - We were calling waggle mode in main and at the end of return mode, which was wrong - changed it so end of return now sets arrived to true if it's been 6 seconds
    - we were constantly updating return_timer because it was in return mode and return mode was repeatedly called. We moved where we set it to flowermode so it would only be set once. 
    - We successfully called waggle mode! but we realized there was no way to get out of Waggle mode once we were in it so it waggled indefinitely. Makig a garage mode at the end of the main hierarchy so it can rest if its completed its waggle. 
    - Appears to be going through the for loop in waggle mode very quickly without actually triggering the drive function, not sure what's happening here. Probably a problem with the for loop, drive function is working everywhere else.
    - added at drive (0,0) i. garage mode to get it to stop
    - We added an if (timer elapsed()) to the for loop in waggle mode, getting it to wait for the drive function to run beofre continuing throught the for loop. This worked!!

## Nov. 18
 - Worked on construction of mounts for beacon using legos and learned about beacon operation
    -  Needed it to be higher than "flower" blocks
    -  Question: would weight of battery tip it too much backwards?
 - Connected pins and declared them in code, editing Arduino code found online
     -  Tested out sensor values - they're all 0, so something isn't working. Also the LED on the beacon sensor is on, but the one on the home beacon is not
     -  Tried grounding home beacon, didn't change anything
