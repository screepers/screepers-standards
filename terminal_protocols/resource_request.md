# resource request protocol

This protocol lets AIs advertise portals the player knows about.

## Serialization Method

This protocol uses simple text for serialization, with whitespace as the delimiter between fields.


## Request


`request [room] [resourceType] [amount]`

`room`: The room which needs the resources.

`resourceType`: One of the `RESOURCE_*` constants.

`amount`: Optional field that can be used to specifiy an amount of resources needed.
