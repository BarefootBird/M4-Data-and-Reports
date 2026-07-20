Kotlin code added to birdaddon to log particles:

```
package com.barefootbird.birdaddon.utils
import com.barefootbird.birdaddon.mixin.SingleQuadParticleAccessorMixin
import com.google.gson.GsonBuilder
import com.odtheking.odin.OdinMod.mc
import com.odtheking.odin.events.TickEvent
import com.odtheking.odin.events.core.on
import com.odtheking.odin.events.core.onReceive
import net.minecraft.client.particle.DustParticle
import net.minecraft.client.particle.SingleQuadParticle
import net.minecraft.core.particles.DustParticleOptions
import net.minecraft.core.registries.BuiltInRegistries
import net.minecraft.network.protocol.game.ClientboundAddEntityPacket
import net.minecraft.network.protocol.game.ClientboundLevelParticlesPacket
import kotlin.math.sqrt


object ParticleLogger {

    private val gson = GsonBuilder()
        .setPrettyPrinting()
        .create()

    private var recording: Recording? = null

    fun isRecording(): Boolean = recording != null

    fun start() {
        recording = Recording(
            startSystemTime = System.currentTimeMillis(),
            startNanoTime = System.nanoTime(),
            startTick = M4State.timer,
            origin = Vec3(M4Mobs.thorn?.x, M4Mobs.thorn?.y, M4Mobs.thorn?.z)
        )
    }

    fun stop() {
        val recording = recording ?: return

        val json = gson.toJson(recording)

        mc.keyboardHandler.clipboard = json

        this.recording = null
    }

    fun recordParticle(x: Double, y: Double, z: Double) {
        val recording = recording ?: return

        if (recording.events.isEmpty()) {
            recording.events += ParticleEvent(
                x = M4Mobs.thorn?.x,
                y = M4Mobs.thorn?.y,
                z = M4Mobs.thorn?.z,
                systemTime = System.currentTimeMillis(),
                nanoOffset = System.nanoTime() - recording.startNanoTime,
                tickOffset = M4State.timer - recording.startTick
            )
        }

        recording.events += ParticleEvent(
            x = x,
            y = y,
            z = z,
            systemTime = System.currentTimeMillis(),
            nanoOffset = System.nanoTime() - recording.startNanoTime,
            tickOffset = M4State.timer - recording.startTick
        )
    }

    init {
        onReceive<ClientboundLevelParticlesPacket> { event ->
            val packet = event.packet
            if (packet is ClientboundLevelParticlesPacket) {
                val type = BuiltInRegistries.PARTICLE_TYPE.getKey(packet.particle.type)

                if (type.toString() == "minecraft:dust") {
                    val col = (packet.particle as? DustParticleOptions)?.color

                    val bearParticleColor = "( 9.804E-2  9.804E-2  9.804E-2)"

                    if (col.toString() != bearParticleColor) {
                        return@onReceive
                    }
                    recordParticle(packet.x, packet.y, packet.z)
                }
            }
        }
    }
}

data class Recording(
    val startSystemTime: Long,
    val startNanoTime: Long,
    val startTick: Int,
    val origin: Vec3,
    val events: MutableList<ParticleEvent> = mutableListOf()
)

data class ParticleEvent(
    val x: Double?,
    val y: Double?,
    val z: Double?,
    val systemTime: Long,
    val nanoOffset: Long,
    val tickOffset: Int
)

data class Vec3(
    val x: Double?,
    val y: Double?,
    val z: Double?
)
```

```
package com.barefootbird.birdaddon.commands

import com.barefootbird.birdaddon.utils.ParticleLogger
import com.github.stivais.commodore.Commodore

val particleCommand = Commodore("birdparticle") {

    literal("start").runs {
        ParticleLogger.start()
    }

    literal("stop").runs {
        ParticleLogger.stop()
    }
}
```

Script to convert this format in to a 2d plot:

```
import clipboardy from "clipboardy";
import fs from "fs";

const clipboard = clipboardy.readSync()

const points = JSON.parse(clipboard).events;

const start = points[0];
const end = points[points.length - 1];

const dx = end.x - start.x;
const dz = end.z - start.z;

const length = Math.sqrt(dx * dx + dz * dz);

const ux = dx / length;
const uz = dz / length;

let output = [];

for (const p of points) {
    const distance =
        (p.x - start.x) * ux +
        (p.z - start.z) * uz;

    output.push({
        distance: Number(distance.toFixed(4)),
        y: Number(p.y.toFixed(4))
    });
}

const table =
    "distance\ty\n" +
    output.map(p => `${p.distance}\t${p.y}`).join("\n");

clipboardy.writeSync(table);

console.log("Converted particle data → Desmos table copied!");
console.log(`Converted ${output.length} points.`);
```