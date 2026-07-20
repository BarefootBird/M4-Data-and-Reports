# Final Particle Cutoff Point

Data was gathered with the following code snippet placed in to birdaddon 

```
onReceive<ClientboundAddEntityPacket> { event ->
    val packet = event.packet
    if (packet is ClientboundAddEntityPacket) {
        if (
            packet.x > 4 && packet.x < 7 &&
            packet.y > 69 && packet.y < 71 &&
            packet.z > 4 && packet.z < 7
        ) {
            debugMessage("bear Spawned $y")
        }
    }
}

onReceive<ClientboundLevelParticlesPacket> { event ->
    val packet = event.packet
    if (packet is ClientboundLevelParticlesPacket) {
        val type = BuiltInRegistries.PARTICLE_TYPE.getKey(packet.particle.type)
        if (type.toString() == "minecraft:dust") {
            if (packet.y < 70.2) {
                debugMessage("packet.y ${packet.y}")
            }
        }
    }
}
```

The goal is to find a y cutoff point where particles dont spawn below said point

My theory before testing is that the final particle is above y = 69.875

data:

| Lowest Particle Y | Bear Spawn Y |
| ----------------: | -----------: |
| 69.69567108154297 |      69.6875 |
|  69.7176284790039 |      69.6875 |
| 69.74454498291016 |     69.71875 |
| 69.74718475341797 |     69.71875 |
| 69.75852966308594 |        69.75 |
| 69.79811096191406 |     69.78125 |
| 69.81616973876953 |      69.8125 |
| 69.84619903564453 |     69.84375 |
|  69.8725814819336 |     69.84375 |
| 69.90083312988281 |       69.875 |
| 69.90194702148438 |       69.875 |
|  69.9120101928711 |     69.90625 |
|  69.9178695678711 |     69.90625 |
|   69.926513671875 |     69.90625 |
|  69.9265365600586 |     69.90625 |
| 69.93772888183594 |      69.9375 |
| 69.94596862792969 |      69.9375 |
| 69.94615173339844 |      69.9375 |
| 69.94960021972656 |      69.9375 |
|  69.9639663696289 |      69.9375 |
|  70.0162124633789 |         70.0 |
|  70.0284652709961 |         70.0 |
| 70.03300476074219 |     70.03125 |
| 70.03414154052734 |     70.03125 |
|  70.0462875366211 |     70.03125 |

# Conclusion

From this data the it can be seen that bears spawn between 69.6875 and 70.03125 (69 + 22/32 to 69 + 33/32), however the real range may be larger than that due to the relatively small sample size

The difference between the maximum lowest particle Y and the minimum lowest particle Y is ~0.35

It would be possible to guess which particle is the final one by checking if the particle's Y value is within the observed range, however im not confident that this would be accurate
