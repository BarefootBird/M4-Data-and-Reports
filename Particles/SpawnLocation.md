# Impact of particles on bear spawn location

Packets were sniffed using [sniffcraft](https://github.com/adepierre/SniffCraft)

---

Bear particle from the same tick as the bear was added:
```
{
    "always_show": true,
    "count": 1,
    "max_speed": 0.0,
    "override_limiter": true,
    "particle": {
        "option": {
            "color": -15132391,
            "scale": 1.0
        },
        "particle_type": "dust"
    },
    "x": 6.53249,
    "x_dist": 0.1,
    "y": 70.2631,
    "y_dist": 0.1,
    "z": 5.42983,
    "z_dist": 0.1
}
```
---

Bear particle 1 tick after bear spawns:

```
{
    "always_show": true,
    "count": 1,
    "max_speed": 0.0,
    "override_limiter": true,
    "particle": {
        "option": {
            "color": -15132391,
            "scale": 1.0
        },
        "particle_type": "dust"
    },
    "x": 6.21634,
    "x_dist": 0.1,
    "y": 69.8763,
    "y_dist": 0.1,
    "z": 5.45131,
    "z_dist": 0.1
}
```

---

Armor Stand above Bear Add Entity Packet (bear nametag):

```
{
    "data": 0,
    "entity_id": 479586,
    "movement": {
        "x": 0.0,
        "y": 0.0,
        "z": 0.0
    },
    "type": 5,
    "uuid": [
        69,
        152,
        146,
        22,
        192,
        97,
        230,
        244,
        60,
        234,
        163,
        141,
        168,
        237,
        162,
        73
    ],
    "x": 6.1875,
    "x_rot": 0,
    "y": 72.0,
    "y_head_rot": 0,
    "y_rot": 4,
    "z": 5.4375
}
```

---


Bear Spawn Add Entity Packet:


```
{
    "data": 0,
    "entity_id": 479585,
    "movement": {
        "x": 0.000488311,
        "y": 0.0,
        "z": 0.0
    },
    "type": 155,
    "uuid": [
        66,
        232,
        230,
        129,
        76,
        253,
        47,
        64,
        171,
        92,
        97,
        233,
        44,
        115,
        72,
        230
    ],
    "x": 6.1875,
    "x_rot": 0,
    "y": 69.875,
    "y_head_rot": 0,
    "y_rot": 0,
    "z": 5.4375
}
```

---

## Sample 1 (packets shown above)

|   | Spawn tick particle | 1 tick later particle | Bear spawn | Armor stand |
| - | ------------------: | --------------------: | ---------: | ----------: |
| x |             6.53249 |               6.21634 |     6.1875 |      6.1875 |
| y |             70.2631 |               69.8763 |     69.875 |          72 |
| z |             5.42983 |               5.45131 |     5.4375 |      5.4375 |


The particle that spawns 1 tick after the bear spawns is rounded to the nearest 1/16 block matches with the bear spawn location

## Sample 2

|   | Spawn tick particle | 1 tick later particle | Bear spawn | Armor stand |
| - | ------------------: | --------------------: | ---------: | ----------: |
| x |             6.39318 |               6.12516 |      6.125 |       6.125 |
| y |             70.2701 |                69.889 |     69.875 |    72.03125 |
| z |              4.8954 |               5.07682 |     5.0625 |      5.0625 |

Again, the bear spawn position matches the particle position after rounding to the nearest 1/16 block.

The armor stand (nametag) shares the same X and Z coordinates as the bear, but its Y coordinate is offset by approximately 2.125–2.15625 blocks (68/32 or 69/32). The reason for the differing offset is currently unknown and is outside the scope of this investigation.

## Sample 3

|   | Spawn tick particle | 1 tick later particle | Bear spawn | Armor stand |
| - | ------------------: | --------------------: | ---------: | ----------: |
| x |             4.95254 |               5.11349 |    5.09375 |     5.09375 |
| y |             70.2776 |             69.902016 |     69.875 |    72.03125 |
| z |             5.48006 |              6.191937 |     6.1875 |      6.1875 |

Here we can see the x value 5.11349 becomes a 5.09375 bear spawn. This shows us two things:
 - The bear spawn is on a 1/32 block grid not 1/16 block
 - The values are floored not rounded (5.11349 is closer to 5.125 than 5.09375)


# Conclusion

Based on the limited samples collected so far, the bear's spawn position appears to be determined by the position of the final particle, with the resulting coordinates quantized to a 1/32-block grid through flooring.

This relationship has been observed consistently across all three axes in all captures analyzed.

These findings suggest that the bear's spawn location may be predictable from particle positions observed before the bear spawns. However, this prediction is currently only possible if the final particle in the sequence can also be identified, which remains an unresolved aspect of the investigation.

# Future Investigations

In all three samples analyzed, the bear spawn Y position was recorded as 69.875. Further investigation would be valuable to determine whether this value is consistent across a larger sample size
