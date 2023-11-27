# Documentation

## Table of Contents
1. [Effects](#effects)
    1. [Sample](#sample)
    2. [Observe](#observe)
    3. [Random](#random)
    4. [Weight](#weight-(factor))
    5. [Emit](#emit)
2. [Metropolis-Hasting](#metropolis-hastings)
    1. Tracing and trace handling (#tracing-and-trace-handling)

## Effects
### Sample
The default handler for the effect Sample draws a sample from a given distribution using the function `draw`. `draw` uses the Box-Muller method to draw two random samples from a Normal/Gaussian distribution using the effekt `Random` to generate two random doubles for input of Box-Muller.  
The effect `Sample` also is used for the tracing and trace handling for Metropolis-Hastings and gets handled in `handleTracing` and `handleReusingTrace`. The first handler constructs the trace while the second handler takes the first element of an existing trace.
### Observe
The default observe handler uses the effect `Weight` to weigh the density of the observed value and distibution.
### Random
With the bulit in function `random`, this effect generates a random value as a Double.
### Weight (Factor)
The first handler `handleWeight` performs rejection sampling and the second handler `handleWeight2` idk ???
### Emit
Emit is defined as an interface because of its type polimorphic properties but it acts just like any other effect in this program. The default handler performes a `println` of the current element, often used in loops. This effect is also used to limit possibly indefinetly running loops usign the `handleLimitEmit`. This handler counts the steps or loops of a given function it runs, and only resumes if the given number of steps is not jet surpased. Like the default handler, the limiter prints the current element in each cycle of the loop. 

## Metropolis-Hasting
The algorithm got split into two seperate functions. The helper function `metropolisStep`, which performes one iteration of the algorithm, and the main algorithm `metropolisHastings` which performs (possibly indefinete) cycles of the algorithm using `metropolisStep`.  
`metropolisStep` also makes use of the function `propose` which recursively adds Gaussian noise to an existing trace and uses the resulting trace as a proposal for the new trace.