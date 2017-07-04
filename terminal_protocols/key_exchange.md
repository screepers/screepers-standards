# key exhange protocol

This protocol lets AIs advertise portals the player knows about.

## Serialization Method

This protocol uses simple text for serialization, with whitespace as the delimiter between fields.


## Request Key

`key request [keyid]`

`keyid`: The identifier for the private key being requested. This should be a purely alphanumeric identifier.

### Responses

Responses to this message are not required.

For successful messages the `Send Key` message described below will be sent.

Keys that are restricted may respond with an unauthorized error. This is not required, and should normally be sent when debugging authorization list code.

`key error unauthorized [keyid]`

If the key does not exist a "not found" error can be returned.

`key error not found [keyid]`

## Send Key

`key [keyid] [keystring]`

`keyid`: The identifier for the private key being sent. This should be a purely alphanumeric identifier.

`keystring`: The actual private key text. This will be a blob of unicode text.
