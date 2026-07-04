# Roblox Luau Logger

A lightweight, strongly-typed, and configurable logging utility for Roblox (Luau) projects. It automatically captures execution context (calling function and line number) to make debugging in the developer console straightforward.

## Features

- **Contextual Logging**: Automatically extracts the calling script/function name and line number using `debug.info`.
- **Debug vs. Warning/Error Levels**: Filter verbose logs on/off dynamically while ensuring critical warnings and errors always print.
- **Prefixing**: Optionally tag logs with specific prefixes (e.g., script name) for quick identification.
- **Table Serialization**: Print complex tables inline in logs with depth limits to prevent recursion hangs, or output them to the console as native expandable objects.
- **Luau Strict Mode**: Fully typed with `--!strict` for autocomplete and static analysis support.

## Installation

### Manual Installation
1. Go to the [Releases Page](https://github.com/Distracted-Games/Logger/releases/latest) and download the latest `Logger.rbxm` file.
2. Drag and drop the model file into your Roblox Studio project.
3. We suggest placing the `Logger` module inside:
   `ReplicatedStorage/Source/Utility/Logger`

---

## API Reference

### Configuration
```lua
type LoggerConfig = {
    Debug: boolean,       -- Toggle printing of debug messages and table outputs
    Prefix: string?,      -- Optional prefix to prepend to logs (e.g., "[MyScript]")
}
```

### Methods

#### `Logger.new(config: LoggerConfig?): Logger`
Creates and returns a new Logger instance.

#### `logger:debug(message: string): ()`
Prints a formatted message to the output only if `Debug = true`.

#### `logger:warn(message: string): ()`
Prints a warning message to the output, regardless of the `Debug` state.

#### `logger:error(message: string): ()`
Throws an error pointing directly to the script and line where `logger:error` was called.

#### `logger:logTable(tbl: any, message: string?): ()`
Prints a table (and an optional description) to the console. In the developer console, this will appear as a native collapsible/expandable table object. Only runs if `Debug = true`.

#### `logger:serialize(value: any, maxDepth: number?): string`
Serializes a value or table into a human-readable string. Automatically handles nested tables up to `maxDepth` (defaults to 3) to prevent infinite recursion. Useful for string interpolation.

---

## Usage Examples

```lua
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Logger = require(ReplicatedStorage.Source.Utility.Logger)

-- Initialize the logger
local logger = Logger.new({
    Debug = true,
    Prefix = script.Name -- Prefixes "[ScriptName] " before messages
})

local data = {
    name = "Player1",
    score = 100,
    inventory = { "Sword", "Shield" }
}

-- 1. Standard Debug Log (printed only if Debug is true)
logger:debug("This is a debug message.")
-- Output: [ScriptName] [MyFunction:16] This is a debug message.

-- 2. Warning Log (always prints)
logger:warn("Something might be wrong.")
-- Output: [ScriptName] [MyFunction:20] Something might be wrong.

-- 3. Serializing a Table Inline
logger:debug(`User login details: {logger:serialize(data)}`)
-- Output: [ScriptName] [MyFunction:24] User login details: { name = Player1, score = 100, inventory = ["Sword", "Shield"] }

-- 4. Printing Native Collapsible Tables in the Developer Console
logger:logTable(data, "Current user state:")
-- Output: [ScriptName] [MyFunction:28] Current user state: ▸ {...}
```

## License

This project is licensed under the [MIT License](LICENSE).
