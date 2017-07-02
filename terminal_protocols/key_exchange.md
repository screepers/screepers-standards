# key exhange protocol

This protocol lets AIs advertise portals the player knows about.

## Serialization Method

This protocol uses simple text for serialization, with whitespace as the delimiter between fields.


## Request Key

`key request [keyid]`

`keyid`: The identifier for the private key being requested. This should be a purely alphanumeric identifier.


## Send Key

`key [keyid] [keystring]`

`keyid`: The identifier for the private key being sent. This should be a purely alphanumeric identifier.

`keystring`: The actual private key text. This will be a blob of unicode text.
