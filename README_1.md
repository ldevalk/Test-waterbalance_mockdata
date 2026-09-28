# Waterbalans Kalmthoutse Heide: dashboard mock-up

| File | What it is |
|---|---|
| `waterbalans_kalmthoutse_heide_dashboard.json` | Grafana dashboard (import via *Dashboards → New → Import*) |
| `waterbalans_kalmthoutse_heide_mock.json` | Mock data: daily values, 120 days measured + 10 days forecast (t0 = 28-09-2026) |
| `genereer_mockdata.py` | Mock data generator (simple bucket model per sub-area, so the balance closes) |
| `src/*.jsonata` | Query logic per panel |
| `src/bouw_dashboard.py` | Builds the dashboard JSON (colours, series order, variables) |
| `preview_kalmthoutse_heide.png` | Preview (matplotlib, not a Grafana screenshot) |
| `archief_ARK-NZK/` | The first mock-up (ARK-NZK example) |

## Sub-areas (indicative, to be replaced by the real drainage areas)

| Id | Name | Country | ha |
|---|---|---|---|
| BE-Stappersven | Stappersven / Nol (KH-Noord) | BE | 600 |
| BE-CentraleHeide | Central heath and fens | BE | 1900 |
| BE-KleineAa | Upper reaches of the Kleine Aa | BE | 1200 |
| NL-GrooteMeer | De Groote Meer | NL | 400 |
| NL-Ossendrecht | Ossendrecht / Brabantse Wal | NL | 900 |
| NL-Huijbergen | Huijbergen / De Zoom | NL | 1000 |

In the dashboard you can also pick **Hele Grenspark** (the whole park), **België** or **Nederland**. These group options are expanded in the query.

## Balance terms (posts)

**Vertical and groundwater**
- Neerslag (precipitation)
- Verdamping (actual evapotranspiration; heath and open water/fens)
- Wegzijging (infiltration to deep groundwater). The sandy plateau is mainly an infiltration area.
- Uittreding voet Brabantse Wal: groundwater leaving towards the Schelde polders (seepage appears at the foot of the slope).

**Surface water leaving the park**
- Afvoer Kleine Aa (towards Essen, then Mark/Weerijs)
- Drainage landbouw (agricultural ditches/drains in the edge zone)
- Afvoer Nol / Vaart Nol-Roosendaal (via the constriction weirs)
- Afvoer richting Zoom

**Structures and abstractions**
- Watertransportleiding Nol → Groote Meer: surplus water from Stappersven/Nol, via the purification plant, to De Groote Meer. It only runs when there is a water surplus.
- Drinkwaterwinning Evides: Ossendrecht and Huijbergen. Values are indicative, and these wells draw from the deep aquifer.

**Exchange between sub-areas** (drops out when both are selected)
- Centrale heide ⇄ Stappersven
- Centrale heide ⇄ Kleine Aa
- Grondwater BE → NL (Ossendrecht / Huijbergen)
- Huijbergen ⇄ Groote Meer

**Netto (bergingsverandering)**: the sum of everything, which equals the change in storage (groundwater plus surface water).

## Units
The **Eenheid** (unit) switch: `m³/dag` (absolute) or `mm/dag` (divided by the area of the selection, useful for comparing BE with NL or sub-areas of different sizes). The *Totaal per post* panel shows the sum in m³ or mm respectively.

## Setup / updating
1. Upload `waterbalans_kalmthoutse_heide_mock.json` to the GitHub repo (*Add file → Upload files*).
2. Import `waterbalans_kalmthoutse_heide_dashboard.json`. It has its own uid, so it appears next to the ARK-NZK mock-up.
3. Check *Databron* (your Infinity source). The Data-URL already points to the new file.

## Fixed series order
Infinity (Go) returns grouped JSONata results in random order. Each post therefore has a `volgorde` (order) field in `stromen`, and the query sorts on it. This keeps the stacking and legend the same on every refresh.
