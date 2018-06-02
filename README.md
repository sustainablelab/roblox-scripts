# Develop Roblox scripts

## Table of contents
- [Repo links](#markdown-header-repo-links)
- [Lua in Roblox](#markdown-header-lua-in-roblox)
- [What is Lua](#markdown-header-what-is-lua)
- [Lua syntax](#markdown-header-lua-syntax)
- [References](#markdown-header-references)

---e-n-d---

## Lua in Roblox
- open *Roblox Studio*
    - The top of the IDE has:
        - FILE
        - HOME
        - MODEL
        - TEST
        - VIEW
        - PLUGINS
- Click on **VIEW**

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
Lua language reference:

https://www.lua.org/pil/1.3.html

Roblox scripting tutorial:

http://wiki.roblox.com/index.php?title=Scripting_Book#Chapter_1

# Repo links
Link to this repo: https://github.com/sustainablelab/roblox-scripts

Clone this repo:
```bash
https://github.com/sustainablelab/roblox-scripts.git
```
