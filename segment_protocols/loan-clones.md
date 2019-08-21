# LOAN clones protocol

This is a list of all known 'clone' accounts. These are users who are running open source bots on their accounts. With additional information

## Serialization Method

This protocol uses json for serialization.

## Format

An object with the open source botname in lowercase as the key containing another object with the following fields
* "name": string - the name of the bot
* "abbreviation": string - the short name of the bot
* "members": array<string> - An array of usernames running the bot
* "primary": string - The Primary username e.g. the person maintaining it
* "url": string - A URL to the repository / code / bot


## Example
```json
{
	"overmind": {
		"name": "Overmind",
		"abbreviation": "overmind",
		"members": ["Muon", "cacomixl8"],
		"primary": "Muon",
		"url": "https://github.com/bencbartlett/Overmind"
	},
	"bonzai": {
		"name": "bonzAI",
		"abbreviation": "bonzai",
		"members": ["Lukethe4th", "ags131", "x3mka", "YanekM", "Komir", "taiga"],
		"primary": "bonzaiferroni",
		"url": "https://github.com/bonzaiferroni/bonzAI"
	}
}
```
