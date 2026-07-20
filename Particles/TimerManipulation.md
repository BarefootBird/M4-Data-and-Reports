```
on<TickEvent.Server> {
    val mid = net.minecraft.world.phys.Vec3(5.5, 69.0, 5.5)
    val thorn = M4Mobs.thorn ?: return@on
    val thornPos = thorn.position()

    val dist = mid.distanceTo(thornPos)
    if (dist !in 27.0..29.4) {
        debugMessage("dist: $dist, thorn: ${thorn.x} ${thorn.y} ${thorn.z}")
    }
}
```

Closest Point:
Bird Addon » dist: 26.964340584482677, thorn: 12.454833984375 84.0 26.800374348958336

Farthest Point: Bird Addon » dist: 29.60245912049226, thorn: -17.170166015625 84.0 -6.220458984375

These points are roughly here (green is closest, blue is farthest):

![Closest and Farthest Points](/Particles/images/closestandfarthestpoints.png)

Here's some data recorded from bears that spawned when stun was in those two positions. Recorded using the [particle logger script](/Particles/particleLogger.md)

(First data point is thorn's position not a particle)

[Close Stun](/Particles/data/closeStun.json) (blue)

[Far Stun](/Particles/data/farStun.json) (green)

![Close And Far Stun Graph](/Particles/images/closeAndFarStunGraph.png)