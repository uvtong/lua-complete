# LuaComplete
LuaComplete is an experimental code completion helper that uses static analysis to determine variable information (like keys and types). It should one day be able to help text editors and IDEs do completion of lua code. It follows the client/server model used for auto-completing code and caches analysis for speed. 

# Hopes and Dreams
I'll try to keep this updated with what's currently working, what I plan to do in the future, and things that are kinda out there.
## Working
LuaComplete can currently help auto-complete:
 * Top-level module functions/variables
 * Lua standard library (except packages)
 * All levels of module information (sub-table values/functions)
 * Function parameters for Lua functions defined in modules
 * Completions of Lua's 'self' with colon operator (mostly, currently doesn't filter out "self" from function list)
 * Table completions (for basic tables)

## In the future
 * Function parameters for Lua functions defined in file
 * Add include paths (can then handle project-level modules)
 * Better cache invalidation. Re-analyze any modules that may have been updated since analysis.
 * UTF-8 support for variable names (server currently has a gmatch pattern that doesn't handle UTF-8.)

## Longshot
The following would require full file analysis as opposed to just module-level analysis (some testing would have to be done as to if there is a speed difference):
 * Return types for imported modules
 * Determine if a Lua module function arg is optional or not
 * Return types in current file (requires more of a type system)
 * Scoping of variables (depends on how hard this ends up being)

## Impossible
I have no idea if the following are even possible to do:
 * Determine a C function's arguments


# Installation
1. Clone this repo
2. `luarocks make lua-complete-dev-1.rockspec`

# Usage
1. Fire up the server 
  * `lua-complete server`
2. Send a file and cursor position (in bytes) to the server:
  * `lua-complete client -f <filename> -c <cursor_position>`

It currently returns the type of completion (either "table" or "function") and any values/types that it knows about.

Example:
```
table
cars: string
foo: string
bar: table
```

# Notes
This is still really experimental! Be careful using this as I'm currently changing how/what it outputs and things that may affect everyday use. I would be especially weary of interacting with the server without the client, as I may change the serde format in the future. The same general warning goes for using any of the analyze module functions. Your best bet is to use the commandline interface or import the client (which could be useful if you're writing a Lua IDE in Lua).
