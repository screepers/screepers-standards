# SS2: Terminal Communications

Using the `description` field of terminal transactions allows messages be sent securely from player to player.

However, the amount of characters that can be sent in an individual message is limited. This requires the messages to either be held within that limit or to be broken up into multiple transmitions. This document seeks to standardize the method for sending and reconstructing messages across multiple transmissions.


## Goals

* Provide a protocol agnostic method for sending messages over the terminal.
* Allow messages to be reconstructed from multiple transmissions.

## Background

### Game.market.incomingTransactions

All transactions sent to a terminal owned by a player are recorded in the `Game.market.incomingTransactions` object.

```json
[{
    "transactionId" : "56dec546a180ce641dd65960",
    "time" : 10390687,
    "sender" : {"username": "Sender"},
    "recipient" : {"username": "Me"},
    "resourceType" : "U",
    "amount" : 100,
    "from" : "W0N0",
    "to" : "W10N10",
    "description" : "trade contract #1",
    "order": {
        "id" : "55c34a6b5be41a0a6e80c68b",
        "type" : "sell",
        "price" : 2.95
    }
}]
```

The `description` part of the transaction is how messages can be sent from player to player. It is only available when using the `terminal.send` function rather than `Game.market.deal`. This means that all transactions with an `order` object can be ignored as sources of communication.

Since the `incomingTransactions` object is supplied by the game engine it can not be manipulated by other players. This means that, short of hacking the game, it is impossible for a user to send a message pretending to be another user and that the `sender.username` object can be used as a source of authentication.


## Format

`msg_id|packet_id|{total_packets}|message_chunk`

* `msg_id`: The id of the message itself. This must be unique to the player who sent the message, but does not need to be globally unique. How the ID is generated does not matter as long as it is an alphanumeric string.
* `packet_id`: The id of the individual packet being sent. This should be a number, starting at 0, which represents the order in which the message can be reconstructed. The max value of this field is 98 (total packets minus one since packets start at zero).
* `total_packets`: The total number of packets sent as part of the message. This value is only available in the first packet. The max value of this field is `99`.
* `message_chunk`: The piece of the message that is being sent.

Message components are delimited by the pipe character (`|`).

Terminal Communication packages will always have three alphanumeric characters, followed by a pipe, folowed by at most two characters and another pipe (`abc|12|` or `abc|3|`). The regular expression `^([\da-zA-Z]+)\|([\d][\d]|[\d])\|.+` can be used to identify which transactions were sent that follow this standard (as well as capturing the `msg_id` and the `packet_id`).


The maximum size the header could be is nine characters. Although developers can utilize the full length of the message if they wish, for ease of implimentaiton it can be assumed that there will be 91 characters for the message piece in each packet.


## Full Example

> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur iaculis libero erat, sed laoreet nisl lobortis a. Suspendisse dignissim et leo vitae feugiat. Duis tincidunt fringilla nisl, eu facilisis orci euismod cursus. Aliquam tristique, eros quis hendrerit blandit, magna elit vulputate odio, vitae lobortis lacus dolor eget libero. Integer quis tempus lorem. Aenean lobortis purus eget nisi dapibus, semper rutrum ex elementum. Interdum et malesuada fames ac ante ipsum primis in faucibus. Cras cursus tempus leo at cursus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Quisque hendrerit dolor a dolor pharetra malesuada. Integer auctor ornare enim. Ut varius eros in metus tempus fringilla. Quisque accumsan at turpis nec fringilla. Aenean eget lorem cursus, dapibus nulla ut, porta mi. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.


The above message broken down and given the id `9f2` would be transmitted over ten packets like below.

`9f2|0|10|Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur iaculis libero erat, se`
`9f2|1|d laoreet nisl lobortis a. Suspendisse dignissim et leo vitae feugiat. Duis tincidunt frin`
`9f2|2|gilla nisl, eu facilisis orci euismod cursus. Aliquam tristique, eros quis hendrerit bland`
`9f2|3|it, magna elit vulputate odio, vitae lobortis lacus dolor eget libero. Integer quis tempus`
`9f2|4| lorem. Aenean lobortis purus eget nisi dapibus, semper rutrum ex elementum. Interdum et m`
`9f2|5|alesuada fames ac ante ipsum primis in faucibus. Cras cursus tempus leo at cursus. Orci va`
`9f2|6|rius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Quisque he`
`9f2|7|ndrerit dolor a dolor pharetra malesuada. Integer auctor ornare enim. Ut varius eros in me`
`9f2|8|tus tempus fringilla. Quisque accumsan at turpis nec fringilla. Aenean eget lorem cursus, `
`9f2|9|dapibus nulla ut, porta mi. Orci varius natoque penatibus et magnis dis parturient montes,`
`9f2|10| nascetur ridiculus mus.`

## Protocols

This standard defines the transmission level protocol for sending data, but it does not supply the higher level protocols needed for actual communication. This should be built by the individual projects as needed.

There are multiple methods to build protocols, but it is expected that the two most common will be by sharing formated text or by sharing json blobs.

An extremely simle example for sharing private keys could look like this-

Request - `key request keyid`  
Response - `key keyid keystring`  

Any message that starts with `{` can be assumed to be JSON. 

