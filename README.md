# LabVIEW-Config
Configuration file library written in LabVIEW; thread and application safe, no reference passing, auto reloading, built-in caching, minimal code.

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

- No references!!! (uses a functional global VI to ensure thread safeness)
  - No more passing references all over the application!!!
- Preloads the INI file to memory (faster performance)
- Supports async disk operations (automatically reloads the file if changed on disk via CRC-16)
- Writes keys to disk when modified (rather than waiting till close)
- Supports all primary data types:
  - I8, I16, I32, I64, U8, U16, U32, U64, SGL, DBL, EXT, EnumU8, EnumU16, EnumU32, String, Path, Boolean
- Data type validation - Raises type cast errors for:
  - Invalid data type cast (e.g DBL("Hello World"))
  - Extra data available (e.g. I32("3.14") = 3 + ".14")
- Supports section, key & line comments (";" or "#")

## Overview

This configuration file library solves a number of issues when dealing with configuration files to load/save settings from an application. For starters, this library does not use any references (DVR, SEQ, FileRefs, etc.) so there's no need to manage Opening/Closing/Sharing references. Instead, all methods within the same application call a functional global to ensure the latest values are read/written, this means that there's only one VI needed by all threads to read/write from the same (or different) configuration files.

The functional global engine VI also checks (via CRC-16) if a file was modified on disk and automatically reloads the file prior to any operation. This ensures that the configuration files are synced between threads as well as applications.

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

As well as a comment input which adds an inline comment if the key is created to add optional usage information for the end user.

## List & Remove Keys

Additionally, the List and Remove Key/Section VIs are included to manipulate the configuration files.

![ConfigListRemove](/docs/imgs/ConfigListRemove.png)

The goal for this library is to provide a standalone, referenceless, thread safe configuration library designed for modularity. Nearly every LabVIEW application needs to load/dump configuration data on startup/shutdown. This library tries to make that process easier.
