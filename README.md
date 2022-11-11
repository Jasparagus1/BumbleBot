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
