# SS3: Unified Credentials File v1.0

As more third party tools are developed users are finding themselves with a variety of different project specific credentials files they have to manage, often with the same details.

This document seeks to standardize a single file to store Screeps credentials for third party tools.


## Goals

* Provide a single file to store credentials.
* Support storing the credentials to multiple servers.
* Let users easily edit this file.


## Location

Project should look for credentials files in the following locations, using the first ones they find.

* Env Variable (`$SCREEPS_CONFIG`)
* Project Root (Optional) - (`project/.screeps.yaml`)
* Current Working Directory - (`./.screeps.yaml`)
* XDG Config Directory - (`$XDG_CONFIG_HOME/screeps/config.yaml`)
* APPDATA (Windows Only) - (`%APPDATA%/screeps/config.yaml`)
* Home Directory - (`~/.screeps.yaml`)


## Format

The file shall be encoded using `Yaml`.

It should have a top level `servers` object that contains the individual credentials for each servers.

The follow values are supported for each connection:

* `host` is required.
* `port` is needed for servers using a nonstandard TCP port.
* `secure` should be set to `true` if SSL is available.
* `token` should be set to the authorization token (currently only supported on the public server).
* `username` is the screeps username, and is only supported on private servers.
* `password` is the screeps password, and is only supported on private servers.

It may have a top level `configs` object that contains individual app specific configs.


## Full Example

```yaml
connections:
  main:
    host: screeps.com
    secure: true
    token: '35a345b9-bc6b-4855-8566-66b341913f9b'
  ptr:
    host: screeps.com
    secure: true
    token: '17f70980-eceb-46ba-a4c3-9677a1570f06'
    ptr: true
  screepsplus:
    host: screepspl.us
    secure: true
    port: 443
    username: bob
    password: password123
  myserv:
    host: 127.0.0.1
    secure: false
    username: bob
    password: password123
configs:
  screepsconsole:
    maxHistory: 20000
    maxScroll: 20000
  screepsplus-agent:
    token: screepsPlusToken
    checkForUpdates: false
  nuke-announcer:
    slack:
      webhook: https://....
      channel: #thewarpath
  nuke-announcer-sp:
    slack:
      webhook: https://....
      channel: #screepsplus-warpath
```