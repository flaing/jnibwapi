# [JNIBWAPI has moved to GitHub](https://github.com/JNIBWAPI/JNIBWAPI) #


# Old Information #

## JNI-BWAPI ##

_Build a [StarCraft](http://us.blizzard.com/en-us/games/sc/) AI in Java!_

## Updates ##
  * 2015-02-11: JNIBWAPI has moved to [GitHub](https://github.com/JNIBWAPI/JNIBWAPI)
  * 2014-05-13: JNIBWAPI 1.0 is released. Includes significant changes to the API to bring it closer to the C++ API and encourage type safety.
  * 2014-02-09: JNIBWAPI 0.4 is now [available for download](https://drive.google.com/a/soe.ucsc.edu/folderview?id=0Bxdd3L2Qj5lxamZUSUpOLVZ6c2M) on  Google Drive. Google Code no longer hosts new downloads.
  * 2013-07-18: JNIBWAPI 0.3 is released. C++ dependencies are included in the release so that the client-bridge DLL can be changed and recompiled. Main package renamed to JNIBWAPI.
  * 2012-06-24: Now compatible with BWAPI 3.7.X . Download the current release package or use source checkout to get the latest bugfixes / improvements.

## Overview ##

This project provides a JNI interface for the [Brood War API](http://code.google.com/p/bwapi/) using the shared memory bridge. Compatible with BWAPI 3.6 and available under the LGPL. This project provides a Java interface for developers interested in participating in the [StarCraft AI Competition](http://www.starcraftaicompetition.com/).

![http://users.soe.ucsc.edu/~bweber/web/bridge.png](http://users.soe.ucsc.edu/~bweber/web/bridge.png)

The JNI interface provides several advantages over the previous socket-based [ProxyBot](http://eis.ucsc.edu/StarCraftRemote) interface. First, the interface uses the BWAPI shared memory bridge which lessens the communication bottleneck between C++ and Java and prevents cheating using direct memory access. Second, the BWAPI utility functions (e.g. canBuildHere) can be now be called from Java. Finally, the use of JNI should result in easier project maintainability and extensibility.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=4yUy7j7skRQ' target='_blank'><img src='http://img.youtube.com/vi/4yUy7j7skRQ/0.jpg' width='425' height=344 /></a>

## Getting Started ##

Take a look at the [Getting Started](GettingStarted.md) guide.

## Developers ##

Interested in improving the project? There are several areas for improvement including:
  1. Improving the message passing protocol between the C++ and Java code
  1. Exposing additional BWAPI functions to Java agents
  1. Testing and documentation

If you have recommendations for improving the project or would like to help contribute, then feel free to use the issue tracker or join as a contributor.

## Projects ##

The following projects are using JNI-BWAPI:

  * [EISBot](http://eis.ucsc.edu/EISBot)

## Links ##

  * [StarCraft](http://us.blizzard.com/en-us/games/sc/)
  * [Broodwar API](http://code.google.com/p/bwapi/)
  * [2011 StarCraft AI Competition](http://www.starcraftaicompetition.com/)
  * [2010 StarCraft AI Competition](http://eis.ucsc.edu/StarCraftAICompetition)
  * [Competition news](http://twitter.com/StarCraftAIComp)
  * [Competition Videos](http://www.youtube.com/user/UCSCbweber)
  * [JBridge (deprecated)](http://code.google.com/p/bwapi-jbridge/)
  * [Java Proxy (deprecated)](http://code.google.com/p/bwapi-proxy/)

## Legal ##

[StarCraft](http://us.blizzard.com/en-us/games/sc/), Brood War and Blizzard Entertainment are trademarks or registered trademarks of Blizzard Entertainment, Inc. in the U.S. and/or other countries. ©1998 Blizzard Entertainment, Inc. All rights reserved.


---

|&lt;wiki:gadget url="http://www.ohloh.net/p/488513/widgets/project\_cocomo.xml" height="250" border="0"/&gt;|&lt;wiki:gadget url="http://www.ohloh.net/p/488513/widgets/project\_languages.xml" height="250" width="320" border="0"/&gt;|
|:-----------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------|