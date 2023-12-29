# Documentation

## Table of Contents
1. [Effects](#effects)
    1. [Sample](#sample)
    2. [Observe](#observe)
    3. [Random](#random)
    4. [Weight](#weight-(factor))
    5. [Emit](#emit)
2. [Metropolis-Hasting](#metropolis-hastings)
3. [Examples](#examples)
    1. [Linear Regression](#linear-regression)
    2. [2D-Robot](#2d-robot)
    3. [SIR model](#sir-model)

## Effects
### Sample
The default handler for the effect Sample draws a sample from a given distribution using the function `draw`. `draw` uses the Box-Muller method to draw two random samples from a Normal/Gaussian distribution using the effekt `Random` to generate two random doubles for input of Box-Muller.  
The effect `Sample` also is used for the tracing and trace handling for Metropolis-Hastings and gets handled in `handleTracing` and `handleReusingTrace`. The first handler constructs the trace while the second handler takes the first element of an existing trace.
### Observe
The default observe handler uses the effect `Weight` to weigh the density of the observed value and distibution.
### Random
With the bulit in function `random`, this effect generates a random value as a Double.
### Weight (Factor)
The first handler `rejectionSampling` performs rejection sampling and the second handler `handleWeight` **TODO!**
### Emit
Emit is defined as an interface because of its type polimorphic properties but it acts just like any other effect in this program. The default handler performes a `println` of the current element, often used in loops. This effect is also used to limit possibly indefinetly running loops usign the `handleLimitEmit`. This handler counts the steps or loops of a given function it runs, and only resumes if the given number of steps is not jet surpased. Like the default handler, the limiter prints the current element in each cycle of the loop. 

## Metropolis-Hasting
The algorithm got split into two seperate functions. The helper function `metropolisStep`, which performes one iteration of the algorithm, and the main algorithm `metropolisHastings` which performs (possibly indefinete) cycles of the algorithm using `metropolisStep`.  
`metropolisStep` also makes use of the function `propose` which recursively adds Gaussian noise to an existing trace and uses the resulting trace as a proposal for the new trace. This is also called independent Metropolis-Hastings.
Another version of `propose` called `proposeSingleSite` proposes only one new sample per iteration and resuses the other samples of the trace. Using this version of `propose` we get the Single-Site Metropolis-Hastings algorithm which is sometimes also called Random Walk Metropolis-Hastings.

## Examples
### Linear Regression
This example uses linear regression which is an analysis method to predict values based on values that are already given. Here we want to approximate the slope and the y-intercept of a linear graph given points the graph crosses.

### 2D-Robot
In this example we trace/approximate the path of a robot that moves over a cartesian plane. At the center of the plane (0,0) is a radar station that measures the distance to the robot with noise.
During one unit of time the robot changes its position based on the current trajectory and also changes its velocity in both x and y direction by adding gaussian noise. This is done via the `move` function. 
With `meassure` we can approximate the new position of the robot based on the new meassurement we get, but also considering the velocity of the robot.

### SIR model
Simulates a population with susceptible, infected and recovered patients. 
The function `step` imitates the transition of patients from e.g. susceptible to infected with the given transition probabilities. The transition probabilities do not correlate to any real live example but rather are just a way of visualization.
With `test` we can test the population for the diseas/virus but we also have to take into consideration that the tests can yield false positives(FP) or false negatives(FN).
