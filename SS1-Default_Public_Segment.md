# SS1: Default Public Segment

Public Segments were added to Screeps to provide a way for different players to communicate with each other. Each player can specify any of their segments as public as well as one segment as their default public segment.

In order for segments to be useful there needs to be an agreed upon protocol between different parties. This standard seeks to define a discovery protocol specifically for the default public segment which will allow players to advertise which protocols they support and how that data can be reached.


## Goals

* Enable the development of new protocols to communicate between players.

* Provide a way for players to programmaticaly identify communication channels.


## Full Example

```json
{
  "api": {
    "version": "draft",
    "update": 19939494
  },
  "channels": {
    "portals": {
      "segments": [43,87],
      "update": 19939494
    },
    "needs": {
      "protocol": "roomneeds",
      "segments": [48],
      "update": 19939431,
      "keyid": "CF53B61"
    },
    "termcom": {
      "protocol": "terminals",
      "data": "[\"E34N53\", \"E65N13\" \"W52N14\"]"
    },
  }
}
```

## Top Level Namespaces

### api

The `api` object contains metadata specifically about this standard.

* `version` The version of the standard, defined using [Semantic Versioning](http://semver.org/)

* `update` The tick when the public segment was last saved.


### channels

The `channels` object contains all the information needed to see which protocols a players is supporting and how to access them.

* Each channel is given a name as its key in the `channels` object which points to an object that stores the channel metadata.

* `protocol` (optional) If undefined then the channel name is used as the protocol.

* `version` (optional) Used to specify a specific protocol version for the channel.

* `update` (optional) The tick the channel was last updated on. This can be used by recieving parties to avoid loading segments and parsing data when it is not needed.

* `segments` (optional) A list of segment IDs used to reconstruct the channel message. To retrieve the message for the channel each segment should be combined together in the order defined by this array. If this field is used the `data` field should not be used.

* `data` (optional) contains the message for the channel. This data must be a string, as different protocols can have different methods for serialization. If this field is used the `segments` field should not be used.

* `compressed` (optional) If true then the message was compressed using [lzstring's](https://github.com/pieroxy/lz-string) `compressToUTF16` function (to decompress use `decompressFromUTF16`).

* `keyid` (optional) If the message is encrypted this string will represent the key needed to decrypt it. Key exchange and management is outside the scope of this document.

* For a message that is both encrypted and compressed it should be compressed before it is encrypted (end thus should be decrypted before it )

* Additional protocol specific options can be added as long as they are prefixed with `x-` so as not to conflict with future changes to this standard.

```json
{
  "api": {
    "version": "draft",
    "tick": <integer>,
  },
  "channels": {
    "<Name of channel>": {
      "protocol": "<name of protocol>",
      "segments": [
        "<segment numbers>"
      ],
      "data": "<message data>",
      "keyid": "<key label>",
      "compressed": true|false,
      "version": "<protocol version>",
      "x-custom": "protocal specific"
    }
  }
}
```


## Naming Protocols

Since two different protocols with the same name would cause conflicts it is important that protocols get registered to prevent collisions.

Private or in development protocols should be prefixed with `x-` and given as unique a name as possible.

To register a new protocol make a pull request with its documentation to the [protocols](./protocols) folder.
