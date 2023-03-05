# LabVIEW-Config
Configuration file library written in LabVIEW.

- Drop-in replacement for NI's Configuration Library.
- Uses no references, passes data in a public cluster (i.e. anyone can access/modify).
  - NI's library uses a single element queue type cast as strict datalog refnum & private VIs.
- Supports ';' and '#' comments.
- Supports '\t' (Tab), '\r' (Carriage Return) & '\n' (Line Feed) escape characters.

```ini
[section]
key = value
; comment

[My Section]
My Key=3.14; Add a comment here
```

## Gettings Started

Refer to the `/Examples/` VIs for usage.

![LabVIEW-Config Demo](/Demo.png)

## Testing

Run the `/Tests/Test All.vi` to verify the configuration library functionality.

If successful, the **All Passed** boolean should be true.
