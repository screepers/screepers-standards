# militarysupport protocol

This protocol lets AIs advertise their need for military support.

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message is an object with room names as the keys that point to another object which contains type (attack being a request for someone to attack a room, or defense being a request for assistance at a room), threat on a scale of 1-5 (1 being minimal assistance needed and 5 being send everything), and tick requested on.


## Example

```json
{
  "E23N43": {
    "type": 'defense',
    "threat": 2,
    "requested": 20155510
  },
  "E26N43": {
    "type": 'attack',
    "threat": 1,
    "requested": 20155351
  },
}
```
