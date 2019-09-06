# LOAN portals protocol

A list of rooms with portals and their destinations.

This protocol is portals scanned by LOAN

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message is an array with portals consiting of an array of strings
* 0 = `source room` the portal is in, the shard depends on what shard you query LOAN for data on
* 1 = `destination Shard`: The shard that the portal leads too
* 2 = `destination Room`: The room that the portal leads too
* 3 = `decayTick` (optional): the exact tick the portal collapses (only available when the portal becomes unstable)

```json
[
  [
    "source room",
    "destination Shard",
    "destination Room",
    "optional decayTick",
  ]
]
```


## Example

```json
[
  [
    "E25N25",
    "shard0",
    "E55N85"
  ],
  [
    "E25N75",
    "shard0",
    "W85S75",
    1567065011488
  ],
]
```
