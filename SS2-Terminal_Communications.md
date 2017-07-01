# SS2: Terminal Communications

Using the `description` field of terminal transactions allows messages be sent securely from player to player.

However, the amount of characters that can be sent in an individual message is limited. This requires the messages to either be held within that limit or to be broken up into multiple transmitions. This document seeks to standardize the method for sending and reconstructing messages across multiple transmissions.


## Goals

* Provide a protocol agnostic method for sending messages over the terminal.
* Allow messages to be reconstructed from multiple transmissions.


## Format

`msg_id|packet_id|{total_packets}|message_chunk`

* `msg_id`: The id of the message itself. This must be unique to the player who sent the message, but does not need to be globally unique. The ID can be generated from random
* `packet_id`: The id of the individual packet being sent. This should be a number, starting at 0, which represents the order in which the message can be reconstructed.
* `total_packets`: The total number of packets sent as part of the message. This value is only available in the first packet. The max value of this field is `9`.
* `message_chunk`: The piece of the message.

Message components are delimited by the pipe character (`|`). Terminal Communication packages will always have three alphanumeric characters, followed by a pipe, folowed by at most two characters and another pipe (`abc|12|` or `abc|3|`)

For ease of implimentaiton it can be assumed that there will be nine characters in the header of the transmission and 91 characters for the message (although developers can try to pack as much data into each segment as they can).


## Full Example

> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur iaculis libero erat, sed laoreet nisl lobortis a. Suspendisse dignissim et leo vitae feugiat. Duis tincidunt fringilla nisl, eu facilisis orci euismod cursus. Aliquam tristique, eros quis hendrerit blandit, magna elit vulputate odio, vitae lobortis lacus dolor eget libero. Integer quis tempus lorem. Aenean lobortis purus eget nisi dapibus, semper rutrum ex elementum. Interdum et malesuada fames ac ante ipsum primis in faucibus. Cras cursus tempus leo at cursus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Quisque hendrerit dolor a dolor pharetra malesuada. Integer auctor ornare enim. Ut varius eros in metus tempus fringilla. Quisque accumsan at turpis nec fringilla. Aenean eget lorem cursus, dapibus nulla ut, porta mi. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.


The above message broken down and given the id `9f2` would be transmitted over ten package like below.

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
