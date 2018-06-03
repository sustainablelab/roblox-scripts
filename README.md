# Develop Roblox scripts

## Table of contents
- [Repo links](#repo-links)
- [Develop a Roblox Lua script](#develop-a-roblox-lua-script)
- [First try](#first-try)
- [Install Rojo](#install-rojo)
- [Setup a project using Rojo server](#setup-a-project-using-rojo-server)
- [Use Rojo to unit test](#use-rojo-to-unit-test)
- [What is Lua](#what-is-lua)
- [Lua syntax](#lua-syntax)
- [References](#references)

---e-n-d---

## Develop a Roblox Lua script
### First try
- open *Roblox Studio*
    - The top of the IDE has:
        - FILE
        - HOME
        - MODEL
        - TEST
        - VIEW
        - PLUGINS
- Click on **VIEW**
    - select *Output* and *Command Bar*
- Click on **TEST**
    - select *Run Script*
    - browse to a script, like `learn.lua`

Unfortunately, *Roblox Studio* out of the box requires clicking all over the IDE
to manage a project. Even testing/running a script after making an edit requires
clicking a button. And if the test was edited in an external editor, like Vim,
you have to browse to the script.

### A better development environment
This is unacceptable, so the first thing to do is learn about *Roblox Studio*
under the hood. I have two goals:

- test/run an externally edited script without using the mouse
- directly access all symbols without using the mouse in the *Roblox Studio*
  Explorer window

### Write a plugin for running an externally-edited script
- https://wiki.roblox.com/index.php?title=Intro_to_Plugins

Hmmm... reading this, it looks like the Roblox team has already done this work!

- https://github.com/LPGhatguy/rojo

I cloned the *Rojo* repo:
```vim
!git clone https://github.com/LPGhatguy/rojo.git
```
And installed the *Rojo* plugin.
### Install Rojo
The developer's instructions are here: https://lpghatguy.github.io/rojo/getting-started/installation/

Here's what I did.

---
There are two pieces to install:

- the *Roblox Studio* plugin, written in Lua
- the *Rojo server*, written in Rust
#### Install the plugin
Google `roblox plugin rojo` and let your web browser start *Roblox Studio*.

Or install it from within *Roblox Studio*. It is *way more work* to find this
from within *Roblox Studio*, but here it is:

- Click on *PLUGINS*
    - Click on *Manage Plugins*
        - in the *Plugin Management* tab, click *Find Plugins*
        - click on the `Library` tab
        - click on the last item in the list, *Plugins*
        - search for `rojo`
        - select `Rojo Studio Plugin`

As of 2018-06-02, the latest version is `0.4.10`. The *Manage Plugins* tool lets
you know if the plugin is up to date.

The *rojo* plugin is now available in *PLUGINS*. Restart *Roblox Studio* for the
plugin to appear.
#### Install the server
Download `rojo.exe` from here: https://github.com/LPGhatguy/rojo/releases

I downloaded the release that matched the version the number of the plugin.

I put `rojo.exe` in my RobloxStudio folder in a folder I made up called
`rojo-server`:
```bash
Documents/RobloxStudio/rojo-server
```
Add the full path to the Windows `PATH`.
```powershell
#Add Roblox *Rojo sever* binary to the path.
$rojo_path = "xxxxxxxx\Documents\RobloxStudio\rojo-server"
$env:PATH = "$env:PATH;$rojo_path"
```
Test: open PowerShell and run `rojo`:
```powershell
rojo help
```
### Setup a project using Rojo server
Make a new project folder in your dev folder and run `rojo init`:
```bash
mkdir my-new-project
cd my-new-project
rojo init
```
This creates `rojo.json`:

=====[ `rojo.json` ]=====
```json
{
  "name": "my-new-project",
  "servePort": 8000,
  "partitions": {}
}
```
Add entries to the `partitions` table. Here is the example from
https://lpghatguy.github.io/rojo/getting-started/creating-a-project/

=====[ `rojo.json` ]=====
```json
{
  "name": "my-new-project",
  "servePort": 8000,
  "partitions": {
    "src": {
      "path": "src",
      "target": "ReplicatedStorage.Project"
    }
  }
}
```
From the documentation:

- `path` is the location on the filesystem relative to the `rojo.json` file.
- `target` is the location in Roblox relative to `game`.

Make the `src` folder in your project:
```bash
mkdir src
```
`my-new-project` now contains `rojo.json` and empty folder `src`.

Start the `rojo server`:
```bash
rojo serve
```
The server starts:
```bash
Using project "my-new-project" from xxxxxxxxxxxxx\Documents\RobloxStudio\dev\my-new-project
Server listening on port 8000
```
From *Roblox Studio*:

- click on *PLUGINS*
    - click on *Test Connection*

In the *Output* window:
```bash
Rojo: Testing connection
Rojo: Server found!
Rojo: Protocol version: 1
Rojo: Server version: 0.4.10
```
From bash or whatever, put the `learn.lua` script in `src`. Click on *Sync In*.
In the *Explorer* window, there is *ReplicatedStorage*, expanding this:

- *ReplicatedStorage*
    - `Project`
        - `learn`

Here are details on how the filesystem project folder contents show up in
*Roblox Studio* following certain conventions:
https://lpghatguy.github.io/rojo/sync-details/

What is next? How do I make a change in script `learn.lua` and see something
change in *Roblox Studio*?

### Rojo does everything you need for developing
Rojo is a *Roblox Studio* Plugin. It does *a lot* of stuff. Half of the modules
I don't even know what they are talking about. Here is the repo:
https://github.com/LPGhatguy/rojo

Learn more about *Rojo* here: https://lpghatguy.github.io/rojo/

There are two *Rojo* modules that caught my attention:

- *Lemur* to emulate the Roblox API
- *TestEZ* to unit test Roblox Lua scripts

### Use Rojo to unit test
#### Context
There are three contexts for unit testing described in the [TestEZ GitHub
repo][testez-github].

- Installation Script (Roblox Studio)
- Filesystem (Roblox)
- Lemur (CI Systems)

[testez-github]: https://github.com/Roblox/testez/

Reading the brief descriptions of each, it seems:

- *Installation Script* is for people who are going to use *Roblox Studio* as
  their development environment. I could not set this up. The instructions
  say to `download the latest release from the GitHub releases page`, but there
  are no releases there.
- *Filesystem* is the most general. This is what I'll use.
- *Lemur* is for people who really know what they are doing. Real developers who
  use *Continuous Integration*.


#### Install TestEZ
The instructions for *Filesystem (Roblox)* are:

> ### Method 2: Filesystem (Roblox)
> * Copy the `lib` directory into your codebase
> * Rename the folder to `TestEZ`
> * Use a plugin like [Rojo](https://github.com/LPGhatguy/rojo) to sync the files into a place

First clone the TestEZ repo:
```vim
!git clone https://github.com/Roblox/testez.git
```

Copy the `lib` folder from the clone to my local `dev` folder:
```bash
mkdir dev/my-new-project/TestEZ
cp -R testez/lib/* dev/my-new-project/TestEZ/
```

The project in your filesystem must be synched with the project in *Roblox
Studio*. Of course, syncing requires that *Rojo sever* is running.

Add the `TestEZ` folder to the `partitions` in `rojo.json`:

=====[ `rojo.json` ]=====
```json
{
  "name": "my-new-project",
  "servePort": 8000,
  "partitions": {
    "src": {
      "path": "src",
      "target": "ReplicatedStorage.Project"
    },
    "TestEZ": {
      "path": "TestEZ",
      "target": "ReplicatedStorage.Project"
    }
  }
}
```

Adding a new partition requires reloading the `rojo.json` file. Stop the server
with `Ctrl-C`, then restart it with `rojo serve` and click *Sync In*.

Synchronizing is copying between the filesystem and *Roblox Studio*:

- top-level items:
    - The top-level path brought in by an item from `partitions` is copied. If
      the project is not saved (i.e., `publish changes`) in *Roblox Studio*, the
      item will vanish after closing and reopening the project, but it will
      still be in the filesystem.
    - If the top-level path item is renamed or deleted in *Roblox Studio* (i.e.,
      in the *Explorer* window), it will be brought in again the next time the
      server is run.
- contents of top-level items:
    - click *Sync In* to bring these files in

## What is Lua
This one sentence sold me:

> Lua is implemented as a library, written in clean C, the common subset of
> Standard C and C++.

That is from https://www.lua.org/manual/5.3/manual.html.

## Lua syntax
### Filetype is .lua
Vim recognizes the `.lua` extension.
```vim
set filetype
```
Gives:
```vim
filetype=lua
```
### Whitespace
It seems Lua follows the usual C conventions for whitespace

- no whitespace in symbol names, use `_` instead
- code can use arbitrary whitespace, e.g. breaking a line of code across many
  lines, except that a string literal cannot contain a line break

Reference: https://en.wikibooks.org/wiki/Lua_Programming/whitespace
### Comments
```lua
--This is a comment.
```
### if
```lua
this_is_true = true
if this_is_true
    then print("this is true")
end
```
## References
### Lua language references
https://www.lua.org/pil/1.3.html
### Roblox basics
- https://wiki.roblox.com/index.php?title=Learn_Roblox
- in particular this link:
    - https://wiki.roblox.com/index.php?title=Intro_to_Plugins
### Roblox scripting tutorials
- https://github.com/Roblox/testez/#writing-tests
- https://wiki.roblox.com/index.php?title=Intro_to_Scripting
- https://wiki.roblox.com/index.php?title=Creating_a_Script
- https://wiki.roblox.com/index.php?title=Creating_Parts_via_Code
- http://wiki.roblox.com/index.php?title=Scripting_Book#Chapter_1
### Your personal Roblox developer page
- https://www.roblox.com/develop?close=1
    - go here to see the games you are making
    - games default to `Public`; click to change to `Private`
    - change game from `Public` to `Private` while under development
    - leaving a game `Public` might be a security risk, I am not sure:
        - click on *HOME*
        - click on *Game Settings*
        - click on *Security*
        - *Experimental Mode* needs to be *On* during development, but the pop-up
          tip recommends turning it off before opening the game to the public
        - *Allow HTTP requests* needs to be *On* for *Rojo* to connect to the
          *Rojo server*
# Repo links
Link to this repo: https://github.com/sustainablelab/roblox-scripts

Clone this repo:
```bash
https://github.com/sustainablelab/roblox-scripts.git
```
