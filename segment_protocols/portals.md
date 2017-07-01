# portals protocol

This protocol lets AIs advertise portals the player knows about.

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message is an object with room names as the keys to an object with the following-

* `destination`: where the portal leads.
* `unstableDate` (optional): the date a portal becomes unstable.
* `decayTick` (optional): the exact tick the portal collapses (only available when the portal becomes unstable)


## Example

```json
{
  "E45S35": {
    "unstableDate":1499327404034,
    "destination":"W85N85",
  },
  "W85N45": {
    "unstableDate":1499327404034,
    "destination":"E45S35",
  },
  "W15N75": {
    "decayTick": 19968297,
    "destination":"W55N45",
  },
  "W55N45": {
    "decayTick": 19968297,
    "destination":"W15N75",
  }
}
```
