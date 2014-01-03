# Version

Command line tool (and Go package) for keeping track of the versions of projects or directories.  Version creates and maintains a `.version` file in the directory containing the current version number, and provides a command line tool to easily get, and update the version number.

Perfect for:

  * Automated build/release scripts
  * Integration with GitHub tags

### Command line

The `version` command line has the following syntax:

    version [flags] path [option]

  * `flags` - Optionally any flags (see below)
  * `path` - Path to set the version for.  Use `./` for current directory.
  * `option`
    * No option will just read and return the current value and will not change it
    * `+` Increase the build number (`1.0.0` -> `1.0.1`) and return the new value
    * `++` Increase the minor number (`1.0.0` -> `1.1.0`) and return the new value
    * `+++` Increase the major number (`1.0.0` -> `2.0.0`) and return the new value

#### Supported flags

  * `-n` - Suppress the linefeed at the end of the output
  * `-v=false` - Do not print the v prefix
  * `-short` - Print the shortest possible representation of the version number, i.e. instead of `v1.0.0`, it will just output `v1`.

### Download

Pick one that matches your machine:

  * Version v1.1 for darwin 386 - [version-v1.1-darwin-386.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-darwin-386.tar)
  * Version v1.1 for darwin amd64 - [version-v1.1-darwin-amd64.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-darwin-amd64.tar)
  * Version v1.1 for freebsd 386 - [version-v1.1-freebsd-386.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-freebsd-386.tar)
  * Version v1.1 for freebsd amd64 - [version-v1.1-freebsd-amd64.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-freebsd-amd64.tar)
  * Version v1.1 for freebsd arm - [version-v1.1-freebsd-arm.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-freebsd-arm.tar)
  * Version v1.1 for linux 386 - [version-v1.1-linux-386.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-linux-386.tar)
  * Version v1.1 for linux amd64 - [version-v1.1-linux-amd64.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-linux-amd64.tar)
  * Version v1.1 for linux arm - [version-v1.1-linux-arm.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-linux-arm.tar)
  * Version v1.1 for windows 386 - [version-v1.1-windows-386.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-windows-386.tar)
  * Version v1.1 for windows amd64 - [version-v1.1-windows-amd64.tar](https://github.com/stretchr/version/releases/download/v1.1.0/version-v1.1-windows-amd64.tar)

Place it into your `/usr/bin` directory or equivalent.

### Tips for writing scripts

Use backticks to get the current version and use it in another command:

    echo `version ./`

... or increase the build version at the same time as getting it:

    echo `version ./ +`

To use the version multiple times, use variables:

    VERSION=`version -n ./ +`; echo $VERSION; echo $VERSION; echo $VERSION

#### Releasing in GitHub

    echo "Last version:" `version ./`

    # increase the version and keep it in the VERSION variable
    VERSION=`version -n ./ +`
    
    echo "New version: $VERSION"
    
    # get the human-readable version number
    SHORTVERSION=`version -short -n ./`

    echo "Tagging release..."

    # Tag the new release
    git tag -a `echo $VERSION` -m "Release SHORTVERSION"

    echo "Updating version file..."

    # Commit the new .version file, since it's changed
    git commit .version -m "Updated to version $SHORTVERSION"

    echo "Pushing changes..."

    # push changes and tags
    git push origin master
    git push --tags
    
    echo "Finished"

The above script will increase the build number, tag the repo with that version, commit the updated `.version` file and push it to the server.  Remember, you can do it in one line using a semi-colon `;` separator like this;

    VERSION=`version ./ +`; git tag -a `echo $VERSION` -m "Release $VERSION"; git commit .version -m "Updated version"; git push origin master; git push --tags
