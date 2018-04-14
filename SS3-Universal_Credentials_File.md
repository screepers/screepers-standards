# SS3: Unified Credentials File v1.0

The proliferation of third-party tools and other projects related to Screeps has led to a significant number of different configuration formats for storing account information. Many users are now faced with the difficulty of having to manage the same information in many different configuration files.

This standard seeks to define a single file format for the storage of Screeps credentials, alleviating the difficulties of managing user credentials for multiple tools.

## Goal

Provide a single, user-editable file that stores credentials to multiple servers.

## Format

The file shall be encoded using a standard `ini` config file format.

Server "friendly names" are listed in brackets `[name]` at the beginning of each server section.

Properties specific to the server are specified in the format `property=value` within the section. 

The following properties are supported for each server:

### Connection Properties
* `host` is required.
* `port` is needed for servers using a nonstandard TCP port.

### Authentication Properties
* `token` should be set to the authorization token, if used.
* `username` is your username
* `password` is your password

Either a token, or a username/password combination are required.
Tokens are currently only available on the public servers.

## File storage & location

Projects seeking to implement the standard should look for a `screeps.ini` file:

1. in the project's configuration directory, if such a directory already exists for other purposes.

2. in the project's root directory, if such a directory exists. (e.g. `screeps-console/screeps.ini`)

3. in the current folder where the script is executed. (`./screeps.ini`)

4. in the current user's configuration folder (`~/.config/screeps.ini`)

## Full Example

```ini
[main]
host=screeps.com
token=########-####-####-####-############

[screepsplus]
host=server1.screepspl.us
port=21025
username=octopus
password=th3p4ssw0rd

[botarena]
host=thunderdome.screepspl.us
port=21025
username=dave
password=screeps123
```
