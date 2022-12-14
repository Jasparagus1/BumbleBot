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
    - We successfully called waggle mode! but we realized there was no way to get out of Waggle mode once we were in it so it waggled indefinitely. Making a garage mode at the end of the main hierarchy so it can rest if its completed its waggle. 
    - Appears to be going through the for loop in waggle mode very quickly without actually triggering the drive function, not sure what's happening here. Probably a problem with the for loop, drive function is working everywhere else.
    - added at drive (0,0) i. garage mode to get it to stop
    - We added an if (timer elapsed()) to the for loop in waggle mode, getting it to wait for the drive function to run beofre continuing throught the for loop. This worked!!

## Nov. 18
 - Worked on construction of mounts for beacon using legos and learned about beacon operation
    -  Needed it to be higher than "flower" blocks
    -  Question: would weight of battery tip it too much backwards?
 - Connected pins and declared them in code, editing Arduino code found online
 - Tested out sensor values - they're all 0, so something isn't working. Also the LED on the beacon sensor is on, but the one on the home beacon is not
     -  Tried grounding home beacon, didn't change anything
     -  Tried swapping wires to see if they were the problem, didn't work
     -  Got an expert to help (The Elusive Phil), who determined that the home beacon seems to be expecting high signal though the user's manual labels it as optional, and he helped us fix the home beacon by connecting it to bread board
 - Started working on beacon navigation code
     -  It's mounted on the back of the robot, so had to switch N/S/E/W values
     -  Should turn according to which part of beacon has signal, until the beacon is facing south
 - Testing it out
     -  We forgot to take out the 6 seconds of drive
     -  It's also going the wrong way
     -  Problem: how to get it to know if it's back at the hive?
       -  We could make the hive really dark so that if the photo values are above a certain amount, it's home
       -  This kind of works, but we should adjust the sharpness of the turns???they're too wide right now,
       -  Waggle still works!
       -  Avoid obstacles is overriding navigate to beacon, so we took it out
       -  Also stabilized the home beacon with more lego connections since now it's on top of a cardboard box
       -  Now it works a lot better!
       -  And also drive is fine even though one side is heavier with the battery
     -  New problem: what if it approaches the hive from the side? 
       -  Turned hive into a shade overhand situation
       -  Also, we need the light off for the flower mode to work properly, but on for the robot to know it's home - otherwise it detects its own shadow and starts waggling
       -  New solution: use a switch to tell when it's home
       -  We don't have enough digital pins for this, so we sacrificed the back bumper
       -  Also had to construct a spot for it to be mounted at the front of the robot
       -  Once it was mounted, we looked at the digital sensor corresponding to the switch and confirmed that it was working
       -  Then we had to go through and comment out a bunch of back bump sensor-related code
 - Testing this out
     - Switch isn't triggered if it approaches the hive at an angle
     - Maybe this is actually because the hive pin wasn't getting read at the right time, so we fixed that
     - Required a lot of fine-tuning of the hive-top's height
     - Also put in a command to drive backward slightly after hitting the hive and before waggling
     - When it approaches from an angle, the switch doesn't get triggered
     - We added whiskers and now it works a lot better
     - Added cardboard to whiskers, circular

# Dec. 2
 - Tested it out in the dark - mostly works, but in the beginning before it detects enough light sometimes it spins around
     - Also, it takes a while to get around obstacles
 - Added cruise arc back in to the subsumption hierarchy above the seek light function in search mode. This is so that it doesnt get stuck in low light areas like the hive when it's first starting out 
 - Basic behavior is working pretty well. Drew a bee face on the cardboard.
 - Dicussed further options with KL, decided to try to record direction of flower relative to hive.
 - Considered options for path integration, ultimately decided to just record the direction of the beacon when the light is first detected, the hope being that this way it will record the direction of the flower while in between the home and flower locations, rather than recording it on the far side of the flower or something like that.
 - This is the "directional waggle" file...
 - We added a "home_direction" variable to be recorded when the light levels pass a threshold to be consistent with navigating toward a flower (in this case, if the sum of the two photosensors is less than 4000).
 - Got it to successfully print the home direction, and then tried to modify the waggle so that the bot first moves back and forth to signify directions (1 time for north, 2 for east, 3 for south, 4 for west) and then does the old waggle turning to signify time/distance.
 - Removed a lot of print statements to try to make debugging easier.
 - Modified the escape_front function so that it randomly chooses between backing up right and backing up left every time, hoping this will solve the getting stuck against a wall problem.
 - Wasn't working properly - tried different configurations of timer elapsed function
 - Eventually figured out we did not need the timer, but the problem was motor hardware
 - Home direction -> waggle function was working
 - Changed mapping in drive to compensate 
 
 ## DEC 7th
  - Test running it, moved the hive to the middle of the environment. This worked fine but it wasn't seeking the light well using braitenburg steering. 
  - We also realized one potential problem of having the hive in the center. The IR sensors don't detect it as on obstical and the front bumper wont activate because its so high off the ground. So when its in search mode, we just need to make the elevated hive detection switch be equivelant to the front bumper. 
  - Braitenburg mode is not sensitive enough, needs to steer harder. This is really weird because this was like the first thing we did and it worked great on Nov 4th. Trying to change the mapping function to map the values of the photosensors from 0 - 3000 instead of 0-4095. It's only active in this mode when the sum of the sensor outputs is less than 5000 combined anyway. This didn't seem to work.
  - new idea is to take the difference between the sensors and add that difference to one side of the difference ends up negitive and another if it ends up positive
  - Changed the mapping in drive on the right motor to use only the middle part of the range. This is to fix a hardware issue with a discrepincy between motor outputs
  - changing the threshold to get into flowermode. This makes it more likey to hit the flower using the IR object seeking, only downside is its more likly to hit a wall and think its a flower
