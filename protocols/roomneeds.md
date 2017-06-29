# roomneeds protocol

This protocol lets AIs advertise their individual room needs.

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message is an object with room names as the keys that point to another object which contains resources as a key with the value needed as a value.


## Example

```json
{
  "E23N43": {
    "power": 10000,
    "H": 10000,
    "K": 10000,
    "XGHO2": 10000,
    "XKH2O": 10000,
  },
  "E37N36": {
    "power": 10000,
    "L": 10000,
    "U": 10000,
    "XGHO2": 10000,
    "XKH2O": 10000,
  }
}
```
