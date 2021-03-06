SONOS HTTP API
==============

A simple http based API for controlling your Sonos system.

USAGE
-----

Start by fixing your dependencies. Invoke the following command:

`npm install`

This will download the necessary dependencies if possible (you will need git for this)

start the server by running

`node server.js`

Now you can control your system by invoking the following commands:

`http://localhost:5005/zones`
`http://localhost:5005/preset/[JSON preset]`
`http://localhost:5005/preset/[predefined preset name]`
`http://localhost:5005/{room name}/{action}[/{parameter}]`

Example:

`http://localhost:5005/living room/volume/15`
(will set volume for room Living Room to 15%)

`http://localhost:5005/living room/volume/+1`
(will increase volume by 1%)

`http://localhost:5005/living room/next`
(will skip to the next track on living room, unless it's not a coordinator)

`http://localhost:5005/living room/pause`
(will pause the living room)

`http://localhost:5005/living room/favorite/mysuperplaylist`
(will replace queue with the favorite called "mysuperplaylist")


The actions supported as of today:

* play
* pause
* volume (parameter is absolute or relative volume. Prefix +/- indicates relative volume)
* seek (parameter is queue index)
* next
* previous
* state (will return a json-representation of the current state of player)
* favorite

State
-----

Example of a state json:

	{
		"currentTrack": {
			"artist":"The Low Anthem",
			"title":"Charlie Darwin",
			"album":"Rough Trade - Counter Culture 2008",
			"duration":270
		},
		"relTime":29,
		"stateTime":1363559099394,
		"volume":11,
		"currentState":"PAUSED_PLAYBACK"
	}


Preset
------

A preset is a predefined grouping of players with predefined volumes, that will start playing whatever is in the coordinators queue.

Example preset:

	{ 
	  "players": [
	    { "roomName": "room1", "volume": 15}, 
	    {"roomName": "room2", "volume": 25}
	  ]
	}

The first player listed in the example, "room1", will become the coordinator. It will loose it's queue when ungrouped but eventually that will be fixed in the future.

preset.json
-----------

You can create a file with pre made presets, called presets.json. I've included a sample file based on my own setup. In the example, there is one preset called `all`, which you can apply by invoking:

`http://localhost:5005/preset/all`

Favorites
---------

It now has support for starting favorites. Simply invoke:

`http://localhost:5005/living room/favorite/[favorite name]`

and it will replace the queue with that favorite. Bear in mind that favorites may share name, which might give unpredictable behavior at the moment.

