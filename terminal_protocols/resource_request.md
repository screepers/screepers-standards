# resource request protocol

This protocol lets AIs advertise portals the player knows about.

## Serialization Method

This protocol uses simple text for serialization, with whitespace as the delimiter between fields.


## Request

`room [resourceType] [amount]`

`resourceType`: This should be one of the `RESOURCE_*` constants.

`amount`: This optional field can be used to specifiy an amount of resources needed.
