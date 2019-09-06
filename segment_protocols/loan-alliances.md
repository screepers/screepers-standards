# LOAN alliances protocol

This contains a variety of metadata about alliances, including their members.

## Serialization Method

This protocol uses json for serialization.

## Format

An object with the alliance name as the key containing another object with the following fields
* "alliance_power_rank": number - The power ranking of the alliance
* "spawns_rank": number - Their spawn rank
* "combined_power_rank": number - Their combined power rank
* "abbreviation": string - The shorthand syntax of the alliance name
* "combined_gcl_rank": number - Their combined GCL rank
* "slack_channel": string - the slack channel for the alliance on the screeps slack workspace
* "name": string - the alliance name
* "logo": string - a filename of the alliance logo, https://www.leagueofautomatednations.com/obj/{logo}
* "members": array<string> - An array of usernames in the alliance
* "rcl_rank": number - Their RCL rank,
* "alliance_gcl_rank": number - Their GCL rank,
* "members_rank": number - Their members rank,
* "color": string - a HEX color string representing the alliance color

Fields ending in _rank is a ranking amongst all alliances, with 1 being the best.


## Example

```json
{
	"int_max": {
		"alliance_power_rank": 9,
		"spawns_rank": 7,
		"combined_power_rank": 9,
		"abbreviation": "int_max",
		"combined_gcl_rank": 8,
		"slack_channel": "int_max_general",
		"name": "INTEGER_MAX",
		"logo": "984738d61ab9bf8.png",
		"members": ["apemanzilla", "Dangermouse"],
		"rcl_rank": 8,
		"alliance_gcl_rank": 9,
		"members_rank": 7,
		"color": "#000000"
	},
	"YP": {
		"alliance_power_rank": 1,
		"spawns_rank": 1,
		"combined_power_rank": 1,
		"abbreviation": "YP",
		"combined_gcl_rank": 1,
		"slack_channel": "ypsilonpact-public",
		"name": "Ypsilon Pact",
		"logo": "367765f8bd52ea7.png",
		"members": ["adammada", "admon", "Zyzyzyryxy"],
		"rcl_rank": 1,
		"alliance_gcl_rank": 1,
		"members_rank": 1,
		"color": "#000000"
	},
}
```
