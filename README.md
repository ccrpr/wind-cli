<p align="center">
  <img src="https://i.imgur.com/iTjMe5x.png"><br/>
  <b>wind-cli</b>: Build a universal Lua script in seconds 🪁
</p>

---
**Features:**
* 🪁 **100/100 compatibility with all major execution platforms.**  
Our build tool uses [`wind.lua`](https://github.com/ccreaper/wind)'s function mappings to implement all common APIs one may use in a script.
* 🔬 **Blazingly fast builds, complete with optimizations, tree-shaking and linting.**  
Outputs are generated almost instantly*. This is because `wind-cli` is written in [Rust](https://www.rust-lang.org/) and compiles to concurrent (parallel) native code.
* 🪡 **Automatic bundling of all statically imported code.**  
You can now use `require` to import code from files like in vanilla Lua, which is a good productivity boost.
* 🛡️ **Security and stealth is built-in and adapts to your project.**  
Our build tool automatically analyzes your script(s) and imports any necessary security patches depending on need.
* 🗄️ **Filesystem virtualization to bundle all of your assets.**  
If desired, a script built with `wind-cli` can also bundle all of its file assets, subsequently accessible through normal FS libraries.
* 🕵️ **Free, heavy obfuscation for the masses.**  
Our obfuscation solution toggleable with a single flag is capable of competing with commercial solutions. It's not open source, but it's free.
* 🔐 **Integrates natively with `wind-cli/whitelist`.**  
Any authentication server built with `wind-cli/whitelist` can serve scripts built with our build tool.

*_Except if you use a naturally slower configuration, such as one with obfuscation._

_More information about `wind-cli`'s integration with Synapse's packaging system to come._

---
## Getting started
To get started, make sure you have [`nodejs` v16+](https://nodejs.org/en/) installed. Then, simply open a terminal and type in the following:
```sh
# Install the wind-cli tool globally.
npm install @ccrpr/wind-cli -g

# Create your first script titled "MyScript".
wind-cli init MyScript

# Navigate into the created directory.
cd MyScript

# Build the script for production.
wind-cli build
```

## Developing with `wind-cli`
### `require`
* `require` works as it does in Lua. This means if you have two files in your project titled `main.lua` and `other.lua`, then you can do:  
```lua
-- In main.lua
local myfile = require("other")
print(myfile) -- "1337"

-- In other.lua
return 1337
```
* `require` is also extended with a special module prefix for remote dependencies. These dependencies will be downloaded and cached during the build step, then built and bundled in your distribution. You can also list several URIs.
```lua
-- A single remote dependency.
local mylib = require("module { \"https://path/to/dependency.lua\" }")
print(mylib.a())

-- Multiple dependencies.
local lib1, lib2, lib3 = require([[
  module {
    "https://path/to/lib1.lua"
    "https://path/to/lib2.lua"
    "https://path/to/lib3.lua"
  }
]])
```
* `require` is also extended with a special auth-based syntax for proprietary libraries downloaded from your `wind-cli/whitelist`-based server after authentication. The library will **not** be packed in your source code.
```lua
-- "protectedLib" can only be retrieved from the server if authentication has been successful.
local protectedLib = require("protected { \"libraryNameSetOnServer\" }")

-- Multiple libraries.
local lib1, lib2, lib3 = require([[
  protected {
    "libraryNameSetOnServer1"
    "libraryNameSetOnServer2"
    "libraryNameSetOnServer3"
  }
]])
```
### Filesystem virtualization
`wind-cli` can optionally bundle your file assets into your distribution's Lua script using a virtualized filesystem. **All filesystem functions, such as `readfile`, will try to locate the passed path in the virtualized filesystem before accessing the disk.** This means you should avoid having duplicate filenames in your script's partition, as the virtualized filesystem comes first and anything on disk with the same location will _not_ be accessed by the default libraries.

**To access files on the local disk while bypassing the virtual filesystem,** you can use `rawreadfile`, `rawwritefile`, `rawappendfile`, etc. These functions will only be available in your environment if a virtual filesystem is available, and any unused functions will be tree shakened out of the distribution.

### Obfuscation
`wind-cli` can be configured to obfuscate any Lua scripts it transforms at build time. The obfuscation **does not** use a virtual machine, and relies exclusively on industry standard techniques for hardening code against analysis. Our obfuscation algorithm is closed-source, but is entirely free to use by anybody for any purpose. Your distribution's source code will be archived then sent to the `wind-cli` obfuscation server for the process, but it is not stored permanently, and the archive is immediately deleted after obfuscation.

We have subjected its outputs to both common and complex deobfuscation techniques, manual and automated, and it is currently undefeated. **However,** as with any obfuscation solution, it is not perfect and will most likely be deobfuscated one day. **Do not use obfuscation as a replacement for good programming practice, such as NOT including important values in the code distributed to the client!**

## Contribution
WIP (publish rest of readme when wind-cli is fully integrated in syn)
