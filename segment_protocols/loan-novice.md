# LOAN novice / respawn zone protocol

A list rooms that are either novice or respawn areas with their end times.

## Serialization Method

This protocol uses json for serialization.

## Format

First indexed by the roomName and gives if its a novice or respawnArea (as the key) and the timestamp it ends
	Probably the most used to be able to lookup if a room is in a zone

```
{
	room1Name:{ [novice/respawnArea]:timestampX },
	room2Name:{ [novice/respawnArea]:timestampY },
	room3Name:{ [novice/respawnArea]:timestampZ }
};
```

## Example
```json
{
	"E11N2": {
		"novice": 1559489452000
	},
	"E11N21": {
		"respawnArea": 1559492186000
	}
}
```
