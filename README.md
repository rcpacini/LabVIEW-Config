# LabVIEW-Config
Configuration file library written in LabVIEW; thread and application safe, no reference passing, auto file reload, built-in memory cache, minimal code.

## Gettings Started

Run the **Demo.vi** to create, read, write, list and remove INI configuration file keys.

```ini
[My Section]
My Key=3.14;Add a comment here

;comment
[section];comment
key=value;comment
```

The only VI needed is the **Key.vim**:

![ConfigKeyVimDbl](/docs/imgs/ConfigKeyVimDbl.png)

**Key features:**

- Thread Safe - supports async disk operations
  - Always reloads before each file operation
  - Writes keys to disk when modified (rather than caching)
- Supports primary data types:
  - I8, I16, I32, I64, U8, U16, U32, U64, SGL, DBL, EXT, EnumU8, EnumU16, EnumU32, String, Path, Boolean
- Data type validation - Raises type cast error if:
  - Invalid data type cast (e.g DBL("Hello World"))
  - Extra data available (e.g. I32("3.14") = 3 + ".14")
- Supports section, key & line comments (";" or "#")
- Built-in symbolic paths:
  - `%memory%` = In memory settings (don't save to disk)
  - `%settings%` = Application settings config path
    - Run-Time: `<AppDir>\Settings.ini`
    - Dev: `<ProjectDir>\Settings.ini`
  - `%temp%` = Temporary settings config path (i.e. for repo ignore)
    - Run-Time: `<AppDir>\Temp.ini`
    - Dev: `<ProjectDir>\Temp.ini`

## Overview

This configuration file library solves a number of issues when dealing with configuration files to load/save settings from an application. 
For starters, this library does not use any references (DVR, SEQ, FileRefs, etc.) so there's no need to manage Opening/Closing/Sharing references throughout the application.

Instead, the non-reentrant methods to read, modify and write (if modified) restrict access within the same application. 
As well as automatically reload the file on each call to ensure any asynchronous disk modifications are reloaded.

All operations to create/read/write INI keys are embedded in the **Key.vim** maleable VI, designed for modularity.

![ConfigLvlib](/docs/imgs/ConfigLvlib.png)

## Key.vim

One design decision was to avoid unknown type casting with LabVIEW specific data types (i.e. Refnums, Timestamps, Arrays, Clusters) and instead focus on the core INI data types (i.e. Int, Uint, Float, Enum, String, Path, Boolean). Far too often, the custom data types cause conflict on how to format/scan to/from one or more keys. This decision is left to the developer. The following primary data types are supported:

![ConfigKeyVimTypes](/docs/imgs/ConfigKeyVimTypes.png)

The **Key.vim** malleable VI adapts to the input data type:

![ConfigKeyVimStr](/docs/imgs/ConfigKeyVimStr.png)

![ConfigKeyVimDbl](/docs/imgs/ConfigKeyVimDbl.png)

The **Key.vim** maleable VI has an optional **Operation** input to:
- **Read or Create** - Reads or creates the key if not found (default)
- **Read** - Reads the key only (do not create)
- **Write** - Write or create the key (writes to disk)

As well as a comment input which adds an inline comment if the key is created to add additional notes.

## List & Remove Keys

Additionally, the List and Remove Key/Section VIs are included to manipulate the configuration files.

![ConfigListRemove](/docs/imgs/ConfigListRemove.png)

The goal for this library is to provide a standalone, referenceless, thread safe configuration library designed for modularity. Nearly every LabVIEW application needs to load/dump configuration data on startup/shutdown. This library tries to make that process easier.

## Testing

Run the `/tests/Test_Config.vi` to verify the configuration library functionality. If successful, the **All Passed** boolean should be true.
