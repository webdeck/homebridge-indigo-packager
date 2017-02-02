# homebridge-indigo-packager
Creates a distribution of [homebridge-indigo](https://github.com/webdeck/homebridge-indigo) suitable for embedding within an [Indigo home automation server](http://indigodomotics.com/) plugin.

# Usage

The makedist script will create the distribution file dist.tgz (a gzipped tarball of the dist directory.)  It will contain the latest version of nodejs, npm, homebridge, and homebridge-indigo, along with scripts to simplify control of multiple homebridge instances.

# Layout of dist.tgz

 ```
    homebridge/
        com.webdeck.homebridge.plist.template
        createdir
        homebridge -> ../node/bin/homebridge
        installhomebridge
        load
        node -> ../node/bin/node
        npm -> ../node/bin/npm
        runhomebridge
        unload
        updatehomebridge
	node -> node-vX.Y.Z-darwin-x64
	node-vX.Y.Z-darwin/
		(embedded installation of nodejs, including globally installed homebridge and homebridge-indigo)
```

Scripts:
* createdir: Creates a subdirectory for an instance of homebridge-indigo, including its launchctl plist and data storage directories.  Does not create a config.json homebridge configuration file.
* load: Tells launchctl to start the instance of homebridge-indigo in the specified subdirectory.
* unload: Tells launchctl to stop the instance of homebridge-indigo in the specified subdirectory.
* runhomebridge: Runs an instance of homebridge-indigo in the specified subdirectory, logging to homebridge.log in that subdirectory.  Intended to be invoked by launchd.
* installhomebridge: Installs homebridge and homebridge-indigo globally in the embedded nodejs instance.  Only used for the initial installation by makedist.  Requires that the XCode Command Line Tools are installed.
* updatehomebridge: Updates the installation of homebridge and homebridge-indigo globally in the embedded nodejs instance.  Requires that the XCode Command Line Tools are installed.

The subdirectory structure supported by the scripts is intended to provide a structured means to work around the HomeKit limit of 100 accessories per bridge.  For all of the scripts that take a subdirectory parameter, it can be a relative or absolute path; if relative, it is relative to the homebridge directory.
