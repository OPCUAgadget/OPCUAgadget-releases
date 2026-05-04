# OPCUAgadget

> **Free full license — valid until 2026-12-31, no tag limit.**
> Get the license key at **[opcuaexport.com](https://www.opcuaexport.com)**
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
| **Free full license** | **Free** | **Until 2026-12-31** |
| Paid license | Contact us | 1 year |

Get your free license key at **[opcuaexport.com](https://www.opcuaexport.com)**

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
