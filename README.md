# FusedStamen Antenna Database — Verified Edition

This database is a ground-up rebuild of the original antenna testing list, designed to be **actionable**: every entry includes a direct purchase link so you can buy exactly what was tested, not just find out what's good and then try to track it down yourself.

## Why a new database?

The original list was built from antennas accumulated over time from various sources including Amazon multipacks, manufacturer samples, bundled hardware, and surplus gear. Over time this created real problems:

- **Identical-looking antennas from different manufacturers** ended up in the same batch with no way to distinguish them
- **Knockoffs and OEM variants** of the same model tested differently but couldn't be cleanly separated in the data
- **No purchase links** meant results were useful for evaluation but not for replication - someone reading "BF8 is Good" had no reliable way to buy the same antenna
- **Data integrity concerns** accumulated as the batch grew, making it hard to confidently recommend specific units

Rather than patch the old list, this database starts fresh with verified provenance: every antenna entered has a confirmed purchase source, a manufacturer SKU or vendor product number as the batch identifier, and a direct link to where it was purchased. Some antennas from the old database will make their way here as I retest and add purchasing information

The old database is preserved as a historical reference in `/archive/` and remains useful for general antenna quality assessment, but this list is the one to use for buying decisions.

---

## Testing Methodology

All antennas are measured using a **LiteVNA 64** with a calibrated short-open-load (SOL) calibration performed before each session.

### Sweep ranges

| Band | Sweep Range | Spot Frequencies |
|------|-------------|-----------------|
| 2.4GHz | 2300–2600 MHz | 2400, 2450, 2484 MHz |
| 5GHz | 4800–6000 MHz | 5150, 5500, 5850 MHz |
| Other bands | ±20% around band | Low, mid, high edge |

### SWR verdict thresholds

Thresholds are set for **passive receive wardriving use**, where reflected power loss matters less than in a transmit context. Verdicts are based on worst-in-band SWR across the full sweep range, not just spot frequencies.

| Verdict | Worst-in-Band SWR | Approx. Power Loss | Practical Meaning |
|---------|-------------------|-------------------|-------------------|
| **Good** | < 3.0 | < 1.25 dB | Recommended for primary rig use |
| **Marginal** | 3.0 – 5.0 | 1.25 – 2.55 dB | Usable as secondary/backup; costs fringe APs at range |
| **Do_Not_Use** | > 5.0 | > 2.55 dB | Measurable consistent signal loss; not recommended |

> **Note on the old database:** The original list used stricter thresholds (Good < 2.0, Marginal 2.0–3.0, Do_Not_Use > 3.0) chosen for antenna quality comparison rather than practical wardriving impact. Many antennas listed as Marginal in the old database would be Good under these thresholds. The new thresholds reflect what actually matters for passive receive capture.

---

## Schema

Each row represents one unit of one antenna tested on one band. Dual-band antennas produce two rows per physical unit.

| Field | Description |
|-------|-------------|
| `batch_id` | `Brand-ModelOrSKU` — manufacturer name + model number or vendor SKU |
| `unit_id` | `batch_id-NN` — zero-padded unit number within the batch; multi-element antennas append element identifier (e.g. `-PanelA-L1`) |
| `description` | Product page title as listed by the vendor |
| `link` | Direct URL to the exact product page where this batch was purchased |
| `connector` | Physical connector type and gender on the antenna side |
| `primary_band` | Band tested in this row (2.4GHz, 5GHz, 1090MHz, etc.) |
| `scan_range_mhz` | VNA sweep range used for this measurement |
| `res_freq_mhz` | Resonant frequency — where SWR was lowest within the band |
| `swr_at_res` | SWR at the resonant frequency |
| `spot1_freq_mhz` | Low-edge spot frequency |
| `spot1_swr` | SWR at low-edge spot |
| `spot2_freq_mhz` | Mid-band spot frequency |
| `spot2_swr` | SWR at mid-band spot |
| `spot3_freq_mhz` | High-edge spot frequency |
| `spot3_swr` | SWR at high-edge spot |
| `overall_status` | Good / Marginal / Do_Not_Use — based on worst-in-band SWR |
| `notes` | Per-unit observations, batch consistency notes, or flags |

---

## Provenance Standard

Every batch in this database meets the following criteria before entry:

1. **Known purchase source** — bought directly from a manufacturer storefront, authorized distributor, or well-documented vendor (not a generic Amazon listing without clear manufacturer identity)
2. **Verified SKU or model number** — batch_id is derived from the manufacturer's own part number or the vendor's SKU, not an assigned internal code
3. **Direct link** — the `link` field points to the exact product page; if a product is delisted, the link is preserved as a historical record and flagged in notes
4. **Per-unit testing** — every individual unit in a batch is tested separately; batch verdicts are not inferred from sampling

---

## Contributing

If you've tested antennas with a VNA and have S1P files with known purchase provenance, contributions are welcome. Open a PR with your S1P files and purchase link and the data will be processed into the standard schema before merging.

---

## Antenna Testing Requests

Have an antenna you'd like to see tested and added to the database? Reach out on Discord in the [#antennas](https://discord.gg/bw8k4fr26) channel.
