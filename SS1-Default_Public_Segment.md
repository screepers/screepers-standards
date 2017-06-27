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
  },

  "channels": {
    "alliance": {
      "segments": [43],
    },
    "needs": {
      "protocol": "market",
      "segments": [48],
      "x-type": "sell"
    },
    "overflow": {
      "protocol": "market",
      "segments": [25],
      "x-type": "buy"
    },
    "terminals": {
      "protocol": "terminals",
      "data": ["E34N53", "E65N13" "W52N14"]
    },
  }
}
```

## Top Level Namespaces

### api

The `api` object contains metadata specifically about this standard. It can be used to mark version changes for compatibility purposes.


### channels

The `channels` object contains all the information needed to see which protocols a players is supporting and how to access them.

* Each channel is given a name as its key in the `channels` object which points to an object that stores the channel metadata.

* If there is no `protocol` field set then the channel name is used as the protocol.

* If the `segments` field is defined it should contain an array of segment IDs. To retrieve the message for the channel each segment should be combined together in the order defined by this array.

* If the `data` field is defined it will contain the message for the channel. This data must be a string, as different protocols can have different methods for serialization.

* Additional protocol specific options can be added as long as they are prefixed with `x-` so as not to conflict with future changes to this standard.

```json
{
  "api": {
    "version": "draft",
  },
  "channels": {
    "<Name of channel>": {
      "protocol": "<name of protocol>",
      "segments": [
        "<segment numbers>"
      ],
      "data": "<message data>",
      "x-custom": "protocal specific"
    }
  }
}
```


## Naming Protocols

Since two different protocols with the same name would cause conflicts it is important that protocols get registered to prevent collisions.

Private or in development protocols should be prefixed with `x-` and given as unique a name as possible. Once the protocol has been solidified and is ready to be made public a simply make pull request to this document with the proposed name and a link to its documentation.
