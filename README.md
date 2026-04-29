# OPCUAgadget

> **Free full license — valid until 2026-12-31, no tag limit.**
> Copy the key below, open OPCUAgadget → About → Activate license, and paste it in.
>
> ```
> eyJjdXN0b21lciI6Ik9QQ1VBRVhQT1JUIiwiZXhwaXJ5IjoiMjAyNi0xMi0zMSJ9LmJmMjIyOGQ0NjIxNGIwMTg1ZTUzMDFjOTI0YmRiNGFjMDY5ZTgxYTMxNjk3ODEzNGE2Y2RkNGE2NTBlNjI1MTk=
> ```
>
> **Using the tool? Open a GitHub Issue and tell us what works and what doesn't — all feedback is welcome.**

---

OPCUAgadget is a lightweight Windows tool that reads an OPC UA server (live connection)
or a NodeSet2 XML file and exports variable tags to **CSV** ready for import into
SCADA/DCS tag databases such as AVEVA System Platform, Citect SCADA, and Webport.

Written in Python (Tkinter GUI), packaged as a standalone Windows EXE.
Primary use case: industrial automation systems with CODESYS-based OPC UA servers
(e.g. Qronox, Wago, Beckhoff, or any IEC 61131-3 runtime).

---

## Download

Download the latest release from the
[Releases page](https://github.com/OPCUAgadget/OPCUAgadget-releases/releases).

Unzip and run `OPCUAgadget.exe` — no installation required.

---

## Demo vs Full version

| Feature | Demo | Full version |
|---|---|---|
| Tag export limit | **150 tags** | Unlimited |
| Live OPC UA export | Yes | Yes |
| NodeSet2 XML import | Yes | Yes |
| CSV export | Yes | Yes |
| Filter rules | Yes | Yes |
| All name modes | Yes | Yes |
| Price | Free | See below |

The demo is fully functional — the only restriction is the 150-tag export cap.
If your server has more tags, the export will stop at 150 and tell you in the log.

---

## Pricing & License

| Type | Price | Validity |
|---|---|---|
| Demo | Free | No expiry |
| **Free full license** | **Free — open a GitHub Issue** | **Until 2026-12-31** |
| Paid license | Contact us | 1 year |

To request a free full license or purchase a paid license, open a GitHub Issue with your
company name and use case.

### Activating your license key

1. Open OPCUAgadget.
2. Click **About** (top-right corner).
3. Click **Activate license**.
4. Paste your license key and click **Activate**.

The license key file is stored in `%APPDATA%\OPCUAgadget\license.key`.

---

## Features

- **Live OPC UA connection** — connect to any endpoint using anonymous login
- **NodeSet2 XML import** — export tags without a server connection; load an exported NodeSet2 file directly
- Automatic namespace detection — no manual configuration needed
- Browse from Objects root or a user-defined start folder
- Export to **CSV** ready for SCADA tag import
- Optional rule-based tag filtering with suffix or leaf matching
- Automatic datatype detection from the OPC UA DataType node
- Fallback metadata injection via filter rules (description, unit, ranges, alarm/trend options)
- Clean tag naming with fixed `_` separator and smart CODESYS struct-member merging
- Fast export mode for large server trees (live mode)
- Open output folder button — jump straight to your exported files
- Background thread — GUI stays responsive during export

---

## OPC UA Authentication

Currently supports **anonymous login** only.

Username/password and certificate-based authentication are planned for a future version.

Make sure your OPC UA server endpoint allows anonymous access.

---

## Export format

Output is a single semicolon-separated **CSV file** per run.

Each exported tag contains:

| Field | Description |
|---|---|
| `name` | Sanitized tag name (path-derived, `_` separated) |
| `address` | `NS1\|String\|/codesys/...` — AVEVA/Webport address format |
| `datatype` | OPC UA DataType (e.g. `Boolean`, `Double`) |
| `rawmin` / `rawmax` | Instrument range |
| `engmin` / `engmax` | Engineering range |
| `unit` | Engineering unit |
| `description` | OPC UA Description attribute |
| `alarmoptions` | From filter rules |
| `trendoptions` | From filter rules |

The address column uses the AVEVA/Webport/UaExpert format:
```
NS1|String|/codesys/volatile/pdp/Application/VS01/GT10_PV
```
This matches the address shown in UaExpert and is ready for direct paste into
AVEVA Webport or Citect tag configuration.

---

## Tag naming modes

| Mode | Description |
|---|---|
| `full` | Full browse path, excluding Objects |
| `relative` | Path relative to start folder |
| `leaf` | Leaf / merged tag name (default) |

Tag separator is always `_`. Invalid characters are replaced with `_`.
Multiple consecutive underscores are collapsed to one.

**CODESYS struct members** are automatically merged with their parent struct name.
For example, a struct `OPT_DAMPAD` with member `_IND_V` becomes `OPT_DAMPAD_IND_V`.

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

These options apply to **live OPC UA server** mode:

- Enable **Fast export** to skip Description, Unit, and Range reads per node
- Enable **Skip system nodes** when start folder is empty (skips Server / Types / Views)
- Use a **Start folder** to limit the scan scope

NodeSet2 XML import is always fast — no server communication involved.

---

## System requirements

- Windows 10 or Windows 11
- Network access to an OPC UA server (for live mode), or a NodeSet2 XML file (for import mode)
- No Python installation required — standalone EXE

---

## Output files

Files are saved to the configured output folder with a timestamp:
```
OPCUAgadget_export_YYYYMMDD_HHMMSS.csv
```

Default output folder: `Documents\OPCUAgadget`

---

## Disclaimer

You are responsible for verifying exported data before using it in production systems.
This software is provided as-is with no warranty.

---

## License

OPCUAgadget is proprietary software. See `LICENSE.txt` for the full terms.

---

## Third-party licenses

Libraries bundled in the `_internal/` folder of this distribution:

| Library | License | Notes |
|---|---|---|
| [asyncua 1.1.5](https://github.com/FreeOpcUa/opcua-asyncio) | LGPL v3+ | Unmodified; replaceable in `_internal/` |
| [aiofiles](https://github.com/Tinche/aiofiles) | Apache 2.0 | asyncua dependency |
| [aiosqlite](https://github.com/omnilib/aiosqlite) | MIT | asyncua dependency |
| [cryptography](https://cryptography.io/) | Apache 2.0 / BSD-3 | asyncua dependency |
| [pyOpenSSL](https://www.pyopenssl.org/) | Apache 2.0 | asyncua dependency |
| [python-dateutil](https://github.com/dateutil/dateutil) | Apache 2.0 / BSD-3 | asyncua dependency |
| [pytz](https://pythonhosted.org/pytz/) | MIT | asyncua dependency |
| [sortedcontainers](http://www.grantjenks.com/docs/sortedcontainers/) | Apache 2.0 | asyncua dependency |
| [typing-extensions](https://github.com/python/typing_extensions) | PSF | asyncua dependency |
| [Python 3.11](https://www.python.org/) | PSF | Runtime |
| [tkinter / Tcl/Tk](https://docs.python.org/3/library/tkinter.html) | PSF / Tcl License | GUI toolkit |

PyInstaller (GPLv2 + Bootloader Exception) is used only at build time and is not shipped.
See `THIRD_PARTY_NOTICES.txt` for full details.
