# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---
## Reflection

### PID Components
**P** accounts for the current cte (Cross Track Error). So the further of the car is of the reference line the stronger
the output value will be. A high coefficient for the P-error will allow the car to react quickly but also cause it to
overshoot and oscillate.

**I** accounts for error values in the past. In this case it is just the sum over all errors. 

**D** accounts for future trends based on the current change rate. A higher coefficient reduces overshooting and
therefore helps the car to stay on the reference line.

### Parameter Tuning
To find working parameters for the P, I and D coefficients a manuel approach was chosen. In the beginning Ki and Kd 
were set to zero. Then Kp was increased until the car oscillated. In the next step Kd was increased until the car was
able to quickly reach it's reference. For Ki, a very small value was chosen since the simulator has basically no steering
bias.

The final values for the coefficients for a speed of 60 mph are:

Kp = 0.15</br>
Ki = 0.0002</br>
Kd = 2.0


Effects of increasing a parameter independently

| Response | Rise Time | Overshoot | Settling Time | S-S Error |
|----------|-----------|-----------|---------------|-----------|
| Kp       | Decrease  | Increase  | NT            | Decrease  |
| Ki       | Decrease  | Increase  | Increase      | Eliminate |
| Kd       | NT        | Decrease  | Decrease      | NT        |

Jinghua Zhong (Spring 2006). "PID Controller Tuning: A Short Tutorial" (PDF). Retrieved 2011-04-04.

Self-Driving Car Engineer Nanodegree Program

A link to the car making it around the track is posted below.
https://youtu.be/fAI-mBvov18


The parameters for the controller were adjusted by trial and error, although a twiddle implementation was built in as well it tended to be very slow to train. Also the twiddle cost function was only looking for the lowest error, which meant that it could favor a pid system that swayed alot in the center of the road over one that was much smoother but it verred off to the side of the road. The parameters also were greatly affected by the speed of the car, note the speed portion was also governed by a pid controller. The steer PID controller used the P,I,D parameters, -.1, -0.0005, -.5. The I term turned out to be pretty insignificant even though the simulated car had a baised steering angle. The baised steering angle meant that if the car drove with zero steer turning then it would veer off to the right. The speed PID had P,I,D parameters of 0.3, 0.002, 0.0, so it was just a PI controller.

The P term greatly impacted the amount of sway that the car had, the larger the value the larger distance it would osscillate. A high P value was important for the car to do tight turns, but too large and it would sway when going stright. The D term was very important for dampening the sway factor, and was set just by experimentation. The I term although not having a large impact to the overall system was being used to help offset the steering biasis.

Although the car could go up to 30 MPH with the parameters chosen, it was set back down to 20 MPH just to be safe.

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets) == 0.13, but the master branch will probably work just fine
  * Follow the instructions in the [uWebSockets README](https://github.com/uWebSockets/uWebSockets/blob/master/README.md) to get setup for your platform. You can download the zip of the appropriate version from the [releases page](https://github.com/uWebSockets/uWebSockets/releases). Here's a link to the [v0.13 zip](https://github.com/uWebSockets/uWebSockets/archive/v0.13.0.zip).
  * If you run OSX and have homebrew installed you can just run the ./install-mac.sh script to install this
* Simulator. You can download these from the [project intro page](https://github.com/udacity/CarND-PID-Control-Project/releases) in the classroom.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
