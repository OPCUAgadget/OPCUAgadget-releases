# OPCUAgadget-releases
OPC UA tag export tool for Windows — browse any OPC UA server and export variable tags to CSV and XML

> **Free demo available — export limited to 150 tags per run.**
> A full version with no tag limit is planned for release.
>
> **Interested? ★ Star this repo or open a GitHub Issue — that is the best way to get in touch.**

---

OPCUAgadget is a lightweight Windows tool for browsing an OPC UA server and
exporting variable tags to **CSV** and/or **XML**.

Written in Python (Tkinter GUI), packaged as a standalone Windows EXE.
Primary use case: industrial automation and SCADA systems.

---

## Demo vs Full version

| Feature | Demo | Full version |
|---|---|---|
| Tag export limit | **150 tags** | Unlimited |
| CSV export | Yes | Yes |
| XML export | Yes | Yes |
| Filter rules | Yes | Yes |
| All name modes | Yes | Yes |
| Price | Free | To be announced |

The demo is fully functional — the only restriction is the 150-tag export cap.
If your server has more tags, the export will stop at 150 and tell you in the log.

To get in touch about the full version, **star this repo or open a GitHub Issue**.

---

## Features

- Connect to any OPC UA endpoint using anonymous login
- Automatic namespace detection — no manual namespace configuration needed
- Browse from Objects root or a user-defined start folder
- Export to CSV, XML, or both simultaneously
- Optional rule-based tag filtering with suffix or leaf matching
- Automatic datatype detection from the OPC UA DataType node
- Fallback metadata injection via filter rules (description, unit, ranges, alarm/trend options)
- Clean tag naming with fixed `_` separator
- Fast export mode for large server trees
- Open output folder button — jump straight to your exported files
- Background thread — GUI stays responsive during export

---

## OPC UA Authentication

Currently supports **anonymous login** only.

Username/password and certificate-based authentication are planned for a future version.

---

## Export format

Each exported tag contains:

| Field | Description |
|---|---|
| `name` | Sanitized tag name (path-derived) |
| `address` | `NSx\|DataType\|/Objects/...` |
| `datatype` | OPC UA DataType (e.g. `Boolean`, `Double`) |
| `rawmin` / `rawmax` | Instrument range |
| `engmin` / `engmax` | Engineering range |
| `unit` | Engineering unit |
| `description` | OPC UA Description attribute |
| `alarmoptions` | From filter rules |
| `trendoptions` | From filter rules |

XML export additionally includes `NodeId`, `BrowsePath`, export statistics, and a settings snapshot.

---

## Tag naming modes

| Mode | Description |
|---|---|
| `full` | Full browse path, excluding Objects |
| `relative` | Path relative to start folder |
| `last3` | Last 3 path elements (shorter names) |

Tag separator is always `_`. Invalid characters are replaced with `_`.
Multiple consecutive underscores are collapsed to one.

---

## Filter rules

Filter files are semicolon-separated CSV files.

Supports:
- Wildcards (`*` and `?`)
- Regex patterns (e.g. `AL[1-8]`)
- Leaf matching (full BrowseName, e.g. `LB01_CMD3`)
- Suffix matching (token after last `_`, e.g. `CMD3`)
- Fallback metadata: description, unit, scaling, alarm/trend options

A sample rules file can be generated directly from the application
(section 3 → Create sample file).

Rules are matched in file order — first match wins.

---

## Performance tips

- Enable **Fast export** to skip Description, Unit, and Range reads per node
- Enable **Skip system nodes** when start folder is empty (skips Server / Types / Views)
- Use a **Start folder** to limit the scan scope

---

## System requirements

- Windows 10 or Windows 11
- Network access to an OPC UA server
- No Python installation required — standalone EXE

---

## Output files

Files are saved to the configured output folder with a timestamp:
```
OPCUAgadget_export_YYYYMMDD_HHMMSS.csv
OPCUAgadget_export_YYYYMMDD_HHMMSS.xml
```

Default output folder: `Documents\OPCUAgadget`

---

## Disclaimer

You are responsible for verifying exported data before using it in production systems.
This software is provided as-is with no warranty.
