# termcom protocol

Although segments can be used for most communication there are some cases where terminal communication is still important (for example, players may use terminals to exchange private keys used to decrypt larger segment data).

This protocol works as a discovery protocol to inform other players of which terminals are being monitored for communication.

## Serialization Method

This protocol uses json for serialization.

## Format

The protocol message should include an array of room names with active terminals that are being monitored for communication.


## Example

```json
["E23N43", "E43N39", "W84S12"]
```
