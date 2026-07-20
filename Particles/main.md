# This section is all about the particles for spawning bears/bats/chickens in m4.

Plots of the curves of how particles spawn:

![plot of particles](/Particles/images/plot.png)

# Particle Main Testing Goals

## Bear Spawn Time Length Manipulation
Not started investigating

## Bear Spawn Time Accuracy Improvement
Not started investigating

# Particle Misc Testing Goals

## Figure out the exact formula for the curve
Not started investigating

## Is the particle path deterministic
Finished: [Results here](/Particles/)

**Conclusion:** yes

## Figure out where the final particle will spawn
[WIP](/Particles/finalParticle.md)

## Figure out how partcles impact the spawn location of the bear
Finished: [Results here](/Particles/SpawnLocation.md)

**Conclusion:** The bear spawns at the final particle's position, with each coordinate floored to the nearest 1/32 block.