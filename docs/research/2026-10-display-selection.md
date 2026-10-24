# OpenDAP T1 touch faceplate: display selection

- **Date:** 2026-10-22. All stock and price figures were captured on 2026-10-22 unless a row says otherwise.
- **Scope:** the display and touch panel of the **touch faceplate** only. The aspect-ratio rule below does not apply to the cassette or classic faceplates.
- **Inputs:**
  - [`board/docs/research/2026-10-sourcing-alternatives.md`](https://github.com/opendap-project/board/blob/main/docs/research/2026-10-sourcing-alternatives.md) §2;
  - [`board/docs/research/2026-10-component-selection.md`](https://github.com/opendap-project/board/blob/main/docs/research/2026-10-component-selection.md) §1;
  - [`t1/docs/requirements/README.md`](https://github.com/opendap-project/t1/blob/main/docs/requirements/README.md) (T1-009, T1-010, T1-013 to T1-016, T1-027 to T1-030);
  - [`faceplates/touch/docs/requirements/README.md`](../requirements/README.md) (FPT-002, FPT-013, FPT-014, FPT-018, FPT-019).
- **Status:** desk research. Nothing here has been built or measured. Every pick is **landed as a recommendation, not validated**: it still needs samples on an RK3566 board, a measured power curve and an outdoor readability check.

## 1. Rules applied

### 1.1 Owner rules

1. **The display fits the product; the product is never resized for a display** (T1-030). The body may only grow inside the maintainers' maximum envelope, keeping its proportions (§1.2).
2. **Ranking order:**
   1. real commercial availability, validated against live distributor or maker stock;
   2. fit to the body and to the requirements;
   3. driver path, in this order of preference:
      - mainline and enabled in Debian's arm64 config;
      - mainline, built as DKMS;
      - vendor-published, validated GPL driver built as DKMS;
      - a new panel compatible that OpenDAP upstreams;
   4. newest generation.
3. **Display aspect ratio (touch faceplate only):**
   - **2:3 portrait is the preferred ratio** (320×480, 480×720, 640×960).
   - Other ratios with height:width ≈ 1.5–1.6 (for example 16:10, 480×768 or 800×1280) are fallbacks.
   - 4:3, 9:16 and square panels are **non-compliant** and are listed only for completeness.
4. **Touch:** use the best touch technology available. Compare in-cell, on-cell and out-cell (GFF/OGS), the controller generation, the mainline driver and the tunability for an extra cover lens (§6).
5. **Interface:** MIPI DSI is preferred. RGB/DPI or SPI only if it is clearly better.
6. **Battery life is a top priority** (T1-009). Outdoor readability matters.

### 1.2 Body envelope and the fit budget

All body figures are **outer** dimensions of the main body, which already include the case walls, the display frame or holder, the cover glass and every layer. Side buttons may protrude beyond the body. The width budget is therefore body-based.

| Envelope | Outer body (W × H × T) | Height:width | Module budget, width | Module budget, height |
|---|---|---|---|---|
| **Ideal** | 54 × 85.6 × ≤20 mm | 1.585 | **≤ 48–50 mm** | ≤ ~79.6–81.6 mm, before top and bottom features |
| **Maximum** | 62.5 × 95 × ≤25 mm | 1.52 | **≤ 56.5–58.5 mm** | ≤ ~89–91 mm, before top and bottom features |

**How the module budget was computed** (used for every candidate):

- Per side, subtract a case wall of **1.5–2 mm** and a display frame or holder margin of **0.5–1 mm**, so **2–3 mm per side** (4–6 mm in total).
- Module budget = outer dimension − 4 to 6 mm.
- When a module forces the body to grow, the resulting body is:
  - width = module width + 4 to 6 mm (the mid value, +5 mm, is used below);
  - height = the larger of (module height + 5 mm) and (width × 1.52), capped at width × 1.59 and at 95 mm. This keeps the body inside the maintainers' 1.52–1.59 ratio band.
- **Assumption (UNVERIFIED until the mechanical layout exists):** the bottom-edge ports (3.5 mm jack, 4.4 mm jack, USB-C and speaker) sit behind the display stack in z. If they need their own band below the display, add their band height to the body height.
  - Rough z check: display 5.5 mm + lens 0.7 mm + 4.4 mm jack about 8 mm + walls 3 mm ≈ 17 mm, which is under the ideal 20 mm.

**Aspect-ratio consequences:**

- A 2:3 panel (1.5) fills a 1.52–1.59 body naturally, with 1–4 mm left for top and bottom features.
- A 9:16 panel (1.78) is too tall for the body ratio.
- A 4:3 panel (1.33) leaves a band free for physical controls, but the touch faceplate does not need one.

## 2. Summary

### 2.1 Findings

1. **No catalog-stocked touch display that meets the aspect rule fits the ideal 54 mm body.**
   - Every stocked 2:3 touch module is a 3.5" 320×480 part whose outline is **54.4–54.5 mm wide** (active area 48.96 mm).
   - The ideal budget is 48–50 mm.
   - The only stocked panels that fit the ideal width are all non-touch and either non-compliant (2.4" 3:4, 3.0" 9:16) or too small and out of stock in MIPI (2.35" 2:3).
2. **The best compliant option needs the body to grow to about 59.5 × 91.5 mm (+5.5 mm wide, +5.9 mm tall, ratio 1.54).** That is well inside the 62.5 × 95 mm maximum.
   - Note (2026-10-24): [t1 ADR 0003](https://github.com/opendap-project/t1/blob/main/docs/adr/0003-tentative-body-size.md) (Accepted) later set the tentative body at 59.5 × 94.3 × 24 mm (credit-card proportions at this width), taller than the 91.5 mm the display alone needs; that ADR is the current body size.
3. **Two stocked 2:3 touch modules fit that grown body:**
   - **Focus LCDs E35GH-I-MW800-CB:** MIPI DSI. It has an **exact mainline compatible**, so no new kernel code is needed. Stock is thin.
   - **Displaytech DT035CTFT-IPS-SHB-PTS:** RGB plus SPI. It has **23 times more stock**. It has no mainline panel driver yet, but a new compatible in a Debian-enabled driver is a credible path.
4. **Excluded because they exceed the maximum:**
   - Newhaven NHD-3.5-640480EF-MSXP-CTP: verified outline 76.90 × 63.90 mm, so 63.9 mm wide in portrait.
   - Winstar WF35UTYAIMNG0: 68.7 × 95.6 mm.
   - Focus E35KB-I-MW850-C1: 62.90 × 88.14 mm.
5. **No AMOLED or OLED** candidate is stocked and compliant. The stocked small AMOLEDs show 0 stock, are 4:3 or round, and have no mainline driver.
6. **No in-cell or on-cell small panel** is catalog-available with a mainline path. Every stocked candidate uses an **out-cell projected-capacitive** touch panel bonded to the LCD (§6).

### 2.2 Recommendation

> **Recommendation (primary): Focus LCDs E35GH-I-MW800-CB.**
>
> - 3.5", 320×480 (2:3), IPS, 800 nit (680 nit minimum), 1-lane MIPI DSI.
> - ILI9488 controller with the exact mainline compatible `focuslcds,e35gh-i-mw800cb`.
> - GT911 touch, which Debian enables.
> - Outline 54.46 × 82.94 × 5.50 mm, so the body grows to about **59.5 × 91.5 mm**.
> - Stock: Digi-Key 9 @ $56.55 and Focus LCDs 9 @ $45.24, "Recommended for New Designs". Its non-touch twin, E35GB-I-MW850-N, has 18 more units.
> - **Condition:** get a production quote, lead time and longevity statement from Focus before the faceplate layout is frozen.
>
> **Recommendation (second source): Displaytech DT035CTFT-IPS-SHB-PTS.**
>
> - 3.5", 320×480 (2:3), IPS, 850 nit, 16-bit RGB plus 4-wire SPI.
> - ST7796UI display controller; ST1633I touch, which Debian enables through `st1232`.
> - Outline 54.40 × 86.24 × 5.50 mm, so the body grows to about **59.5 × 92.5 mm**.
> - Stock: Digi-Key 256 and Mouser 159. Price $52.71 at 1, $38.29 at 100.
> - It needs a new ST7796 DPI panel compatible (§5.2) and about 24 faceplate pins instead of 10–12 for DSI.
> - **Use it for the first prototypes and the DIY kit if Focus cannot quote production volume.**

**On rule 1:**

- By raw quantity on the shelf, the Displaytech part ranks first.
- Both parts meet the maintainers' availability bar: an established maker with live stock, a datasheet, published init code and MOQ by quote.
- Rules 2 and 3 then favour Focus:
  - it is 3.3 mm shorter;
  - it uses DSI, which the requirements prefer;
  - it already has a mainline driver.
- **If the maintainers treats shelf quantity as decisive, swap the two.**

## 3. Candidate tables

Columns are the same in every table. "Fit" uses the budgets in §1.2. "Body" is the resulting outer body (W × H, ratio) computed with the rule in §1.2.

### 3.1 Tier A: fits the ideal 54 × 85.6 mm body (module ≤ 48–50 mm wide)

No stocked touch module qualifies. These are the closest parts, all **without touch**, so each would need a custom CTP or cover lens from the maker.

| Part (maker) | Size / resolution / aspect | Outline W × H × T (mm) / active | Fit (ideal) | Tech / brightness | Interface / controller | Touch | Temp | Stock / price (2026-10-22) | Lifecycle | Driver path |
|---|---|---|---|---|---|---|---|---|---|---|
| **AFR320480A0-2.35INTH-I** (Orient Display) | 2.35" 320×480, **2:3** | Outline **UNVERIFIED** ([datasheet](https://www.orientdisplay.com/wp-content/uploads/2020/07/AFR320480A0-2.35INTH-I.pdf) not parsed) / 34.32 × 50.88 | Yes (active 34 mm wide). Body stays 54 × 85.6, but the screen is very small | IPS / **UNVERIFIED** | MIPI DSI / ST7701S | **None** | UNVERIFIED | [Digi-Key **0**](https://www.digikey.com/en/products/detail/orient-display/AFR320480A0-2-35INTH-I/22531849) (1 expected 2026-12-17), $23.03 / $14.45 @ 500. RGB/SPI twin `AFR320480A0-2.35INTH`: Digi-Key 47 @ $24.16 | Active | ST7701 is mainline (`panel-sitronix-st7701.c`), Debian **off**: new compatible plus DKMS |
| **E30RA-I-MS550-N** (Focus LCDs) | 3.0" 480×854, 9:16 (**non-compliant**) | **42.04 × 73.67** / 36.72 × 65.28 | Yes | IPS / 550 nit | 2-lane MIPI DSI / ST7701S | **None** | −30 to +85 °C | [Digi-Key 23 @ $35.05](https://www.digikey.com/en/products/result?keywords=E30RA-I-MS550-N); Focus 23 @ $28.04 | Recommended for New Designs | ST7701 mainline, Debian off: new compatible plus DKMS |
| **E24RF-I-MS800-N** (Focus LCDs) | 2.4" 480×640, 3:4 (**non-compliant**) | **42.92 × 60.26** / 37.72 × 49.86 | Yes | IPS / 800 nit | 2-lane MIPI DSI / ST7701SI | **None** | −30 to +80 °C | [Digi-Key 25 @ $37.82](https://www.digikey.com/en/products/detail/focus-lcds/E24RF-I-MS800-N/16601997); Focus 25 @ $32.51 | Recommended for New Designs | ST7701 mainline, Debian off: new compatible plus DKMS |

**Touch versions do not fit.** Focus's own touch version of the 3.0" panel, **E30RA-I-MW350-C**, has an oversized bonded lens:

- outline **52.72 × 83.28 × 4.22 mm** (spec sheet), so it fails the ideal width;
- 350 nit, FT5436 touch, Digi-Key 9 @ $62.78;
- 9:16 anyway.

The RGB variant, E30RA-I-RW500-C, has the same outline and 500 nit.

### 3.2 Tier B: needs the body to grow toward the maximum (module ≤ 56.5–58.5 mm wide)

| Rank | Part (maker) | Size / resolution / aspect | Outline W × H × T (mm) / active | Fit and resulting body | Tech / brightness / contrast | Interface / controller | Touch | Power (datasheet) | Temp | Stock / price (2026-10-22) | Lifecycle | Driver path (evidence) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| **1** | **E35GH-I-MW800-CB** (Focus LCDs) | 3.5" **320×480, 2:3**, 165 ppi | **54.46 × 82.94 × 5.50** / 48.96 × 73.44; CTP view 49.56 × 74.04 | Fails the ideal (54.46 > 50). Fits the maximum. **Body ≈ 59.5 × 91.5 mm, 1.54 (+5.5 W, +5.9 H)** | IPS, normally black, 800 nit typ. (680 min), 700:1, 80° all round, 50 % gamut | **1-lane MIPI DSI** / ILI9488 | GT911, 5 points, optically bonded CTP (stack type UNVERIFIED) | Backlight 10 LEDs, 15 V × 60 mA ≈ 0.9 W at full; VCI 2.8 V | −20 to +70 °C | [Digi-Key 9 @ $56.55](https://www.digikey.com/en/products/detail/focus-lcds/E35GH-I-MW800-CB/28144107); [Focus 9 @ $45.24](https://focuslcds.com/product/e35gh-i-mw800-cb/); production MOQ by quote | Recommended for New Designs | **Mainline exact compatible** `focuslcds,e35gh-i-mw800cb` in `drivers/gpu/drm/panel/panel-ilitek-ili9488.c` (commit `99e9aca83bfb`, 2026-06-17). `DRM_PANEL_ILITEK_ILI9488` is absent in Debian 7.2 and "not set" in 7.3 → **DKMS** plus a Debian config request. GT911 → `goodix`, Debian `=m` |
| 1b | E35GB-I-MW850-N (Focus LCDs), the same panel without touch | 3.5" 320×480, 2:3 | 54.46 × 82.94 × UNVERIFIED | Same as rank 1 | IPS / 850 nit | 1-lane MIPI DSI / ILI9488 | **None** | UNVERIFIED | −20 to +70 °C | [Digi-Key 18 @ $30.95](https://www.digikey.com/en/products/detail/focus-lcds/E35GB-I-MW850-N/28716490); Focus 18 @ $24.77 | Recommended for New Designs | Same panel and init, so the same driver. Useful as a stock buffer if Focus bonds a CTP or OpenDAP adds one |
| **2** | **DT035CTFT-IPS-SHB-PTS** (Displaytech) | 3.5" **320×480, 2:3** | **54.40 × 86.24 × 5.50** (non-touch twin 3.50 mm thick) | Fails the ideal. Fits the maximum. **Body ≈ 59.5 × 92.5 mm, 1.55 (+5.5 W, +6.9 H)** | IPS, 850 nit (CTP version; 1,000 without CTP), 700:1, 80° all round, 6H surface | **16-bit RGB plus 4-wire SPI** / ST7796UI-G5 | **ST1633I**, projected capacitive, I²C | Backlight 6 LEDs (2 × 3), 60 mA, VF 11.4–13.4 V → ≈ 0.75 W at full; IDD "TBD" | −20 to +70 °C | [Digi-Key 256 @ $52.71 (10: $43.56, 100: $38.29)](https://www.digikey.com/en/products/detail/displaytech/DT035CTFT-IPS-SHB-PTS/26772446); [Mouser 159 @ $52.71](https://www.findchips.com/search/DT035CTFT-IPS-SHB-PTS); 9-week factory lead time | Active | **No mainline ST7796 DPI driver** (§5.2). Best path: a new compatible in `panel-sitronix-st7789v.c` (SPI init plus DPI video, Debian `=m`), otherwise DKMS. Touch: `sitronix,st1633` in `drivers/input/touchscreen/st1232.c`, `TOUCHSCREEN_ST1232=m` in Debian 7.2 and 7.3 |
| 3 | E30RA-I-MW350-C (Focus LCDs) | 3.0" 480×854, 9:16 (**non-compliant**) | 52.72 × 83.28 × 4.22 / 36.72 × 65.28 | Fits the maximum. Body ≈ 57.7 × 88.3 mm, 1.53. The body is fine, but the display ratio of 1.78 breaks the rule | IPS, 350 nit, 1000–1500:1 | 2-lane MIPI DSI / ST7701S | FT5436, optical bonding | Backlight 19.2 V × 20 mA ≈ 0.38 W; IDD 38 mA | −20 to +70 °C | [Digi-Key 9 @ $62.78](https://www.digikey.com/en/products/detail/focus-lcds/E30RA-I-MW350-C/13683598); Focus 9 @ $53.94 | Recommended for New Designs | ST7701 mainline, Debian off: new compatible plus DKMS. FT5436 → `edt-ft5x06`, Debian `=m` |
| — | NHD-2.9-376960AF-ASXP-CTP (Newhaven) | 2.9" 376×960, 1:2.55 (**non-compliant**, a bar display) | UNVERIFIED / 27.51 × 68.68 | Fits the ideal width, but the ratio is far outside the band | IPS / UNVERIFIED | RGB plus SPI / ST7701SN | FT5426G | UNVERIFIED | UNVERIFIED | [Digi-Key 88 @ $36.60](https://www.digikey.com/en/products/detail/newhaven-display-intl/NHD-2-9-376960AF-ASXP-CTP/28007646); TME 2 | Active | ST7701 mainline (DSI only); RGB mode needs a new driver. Listed only because it is stocked |

### 3.3 Excluded: exceed the maximum envelope or fail availability

| Part (maker) | Size / resolution / aspect | Outline W × H × T (mm) | Why excluded | Stock (2026-10-22) |
|---|---|---|---|---|
| NHD-3.5-640480EF-MSXP-CTP (Newhaven) | 3.5" 640×480, 4:3 (non-compliant) | **76.90 × 63.90 × 4.95** (drawing rev. 09/05/2025, read from the spec's mechanical drawing) | 63.9 mm wide in portrait needs a ≥ 68 mm body, above the 62.5 mm maximum. It is also 4:3 | Digi-Key 189 @ $44.85; Mouser 69; TME 9 |
| WF35UTYAIMNG0 (Winstar) | 3.5" 320×480, 2:3 | **68.7 × 95.6 × 4.61** ([datasheet](https://www.winstar.com.tw/d/514/WF35UTYAIMNG0.pdf)) | The oversized cover lens exceeds the maximum in both width and height. 500 nit typical | Digi-Key 11 @ $38.99 |
| E35KB-I-MW850-C1 (Focus LCDs) | 3.5" 320×480, 2:3 | 62.90 × 88.14 | Exceeds the width budget, and is "Not Recommended for New Designs" | Digi-Key 5 @ $79.30 |
| E35RC-B-MW360-C (Focus LCDs) | 3.5" 480×640, 3:4 | UNVERIFIED (active 54.28 × 72.04) | Active area alone is 54.3 mm wide, so the outline exceeds the budget. Non-compliant ratio. $131 | Digi-Key 9 |
| AB-J240-A800R (American Bright), AMOLED | 2.4" 600×450, 4:3 | UNVERIFIED | 0 stock; RM690B0 has no mainline driver; 4:3 | Digi-Key 0 |
| Waveshare 2.8inch DSI LCD | 2.8" 480×640, 3:4 | UNVERIFIED | Non-compliant ratio. Controller not published. Runs only through the Raspberry Pi downstream `panel-waveshare-dsi.c` (`waveshare,2.8inch-panel`, MCU at I²C 0x45), which is not in mainline. Mainline `bridge/waveshare-dsi.c` covers only the `dsi2dpi` and `dsi2lvds` boards | [Waveshare $43.99](https://www.waveshare.com/2.8inch-dsi-lcd.htm), no stock figure |

## 4. Fit and aspect ranking

The maintainers' preference order is applied first, then fit, then availability.

| Order | Candidate | Display ratio | Ideal body? | Body needed (W × H, ratio) | Growth vs ideal |
|---|---|---|---|---|---|
| 1 | Focus E35GH-I-MW800-CB | **2:3 (1.50), preferred** | No | 59.5 × 91.5, 1.54 | +5.5 W, +5.9 H |
| 2 | Displaytech DT035CTFT-IPS-SHB-PTS | **2:3 (1.50), preferred** | No | 59.5 × 92.5, 1.55 | +5.5 W, +6.9 H |
| 3 | Orient AFR320480A0-2.35INTH-I + custom CTP | **2:3 (1.50), preferred** | Yes | 54 × 85.6, 1.585 | none, but a 2.35" screen and 0 MIPI stock |
| — | No 16:10 or other 1.5–1.6 fallback was found in stock at 2.4–3.5" | — | — | — | — |
| n/c | Focus E30RA-I-MW350-C | 9:16 (1.78), non-compliant | No | 57.7 × 88.3, 1.53 | +3.7 W, +2.7 H |
| n/c | Focus E30RA-I-MS550-N / E24RF-I-MS800-N (no touch) | 9:16 / 3:4, non-compliant | Yes | 54 × 85.6 | none |

**Reading:**

- At 3.5", a 2:3 panel needs the grown body.
- A 2:3 panel of about 3.0–3.2" would fit the ideal body: active about 41–43 mm wide, so a module about 45–47 mm wide.
- **No such part was found in stock** at Digi-Key, Mouser, Focus LCDs, Newhaven, Winstar, Orient or Displaytech.
- It remains the target for a **semi-custom module** from a Shenzhen house (§7). Its MOQ and NRE are UNVERIFIED.

## 5. Driver paths in detail

### 5.1 Debian arm64 status (read from `linux-config-7.2_7.2.9-1~bpo13+1` and `linux-config-7.3_7.3~rc7-1~exp1`)

| Symbol | 7.2 bpo | 7.3 exp | Relevance |
|---|---|---|---|
| `DRM_PANEL_ILITEK_ILI9488` | absent (driver not in 7.2) | not set | Focus E35GH/E35GB → DKMS until enabled |
| `DRM_PANEL_SITRONIX_ST7789V` | m | m | Host for a new ST7796 SPI-plus-DPI compatible (Displaytech) |
| `DRM_PANEL_SITRONIX_ST7701` | not set | not set | Orient 2.35", Focus E30RA/E24RF |
| `DRM_PANEL_SITRONIX_ST7703` | m | m | No stocked 2:3 ST7703 part was found |
| `DRM_PANEL_SIMPLE` | m | m | Only for DPI panels that need no init sequence |
| `DRM_PANEL_MIPI_DBI` | not set | not set | SPI-only full-frame transfer; too slow for 320×480 at 60 Hz |
| `DRM_WAVESHARE_BRIDGE` | not set | not set | Not relevant (DSI2DPI boards only) |
| `TOUCHSCREEN_GOODIX` | m | m | GT911 (Focus) |
| `TOUCHSCREEN_EDT_FT5X06` | m | m | FT5436 / FT5426G |
| `TOUCHSCREEN_ST1232` | m | m | ST1633I (Displaytech) |
| `TOUCHSCREEN_HYNITRON_CSTXXX`, `TOUCHSCREEN_ILITEK` | not set | not set | Avoid these touch ICs |
| `ROCKCHIP_DW_MIPI_DSI` / `PHY_ROCKCHIP_INNO_DSIDPHY` | y / m | y / m | RK3566 DSI host and PHY |

### 5.2 Per candidate

- **Focus E35GH (ILI9488, DSI): mainline via DKMS.**
  - The exact compatible already exists, so the device tree only needs `compatible = "focuslcds,e35gh-i-mw800cb"` and one data lane. The driver hard-codes `lanes = 1`, 320×480 and 48 × 73 mm.
  - Ship `panel-ilitek-ili9488.c` as a DKMS package for Debian 7.2 and 7.3.
  - Ask Debian to set `CONFIG_DRM_PANEL_ILITEK_ILI9488=m`.
  - No new kernel code is needed.
  - **Caveat:** a 1-lane, 320×480 at 60 Hz, 24 bpp link needs about 221 Mbit/s, well inside one D-PHY lane. RK3566 DSI with a single lane is supported by `dw-mipi-dsi`; on-hardware behaviour is UNVERIFIED.
- **Displaytech DT035CTFT (ST7796, RGB plus SPI): a new compatible we upstream.**
  - `panel-sitronix-st7789v.c` already implements "SPI command init, then DPI pixel stream" and is enabled in Debian.
  - ST7796 shares the MIPI DCS core with ST7789V but has a different extended command set (gamma, power control).
  - Adding it means a per-panel init hook plus a 320×480 mode. How much rework the driver's fixed ST7789V init needs is **UNVERIFIED**, so budget a real upstream patch.
  - Until it lands, ship it as DKMS.
  - Cost to the faceplate: 16 data lines plus PCLK, HSYNC, VSYNC, DE and SPI (CS, SCL, SDA) ≈ **23–24 signals** across the faceplate flat cable, against 10 for 2-lane DSI or 6 for 1-lane DSI (plus grounds).
- **Orient 2.35" / Focus E30RA / E24RF (ST7701S, DSI):**
  - Add a new compatible to `panel-sitronix-st7701.c`, using its parametrised descriptor (gamma tables, timings, lanes).
  - Focus publishes "2 Lane MIPI Code" for E30RA and E24RF.
  - Debian has the driver off, so ship it as DKMS plus a config request.

## 6. Touch technology

| Technology | What it is | Optics and thickness | Availability for this product | Mainline |
|---|---|---|---|---|
| **In-cell (TDDI)** | Touch electrodes inside the LCD cell; one IC drives display and touch | Thinnest, best transmittance, no extra air or adhesive layer | Phone-class panels only. **None found** in distributor stock at 2.4–3.5"; no 2:3 TDDI part with init code | `DRM_PANEL_SYNAPTICS_TDDI` exists but is off in Debian, and no small panel has an entry ([component-selection §1](https://github.com/opendap-project/board/blob/main/docs/research/2026-10-component-selection.md)) |
| **On-cell** | Sensor patterned on the colour-filter glass | Thin, good optics | Small AMOLEDs (CO5300, RM690B0 families) and some LCDs. **None stocked** that are compliant | Touch IC drivers vary; the DDICs lack mainline drivers |
| **Out-cell OGS / G+G** | Sensor on the cover glass (OGS) or on a second glass (G+G) | One or two glasses; with OCA or OCR bonding, optics are very good | Offered by Winstar (codes `G`, `G1`, `G2`) and others as options. Stocked parts are oversized (Winstar) | GT911, FT5x06 and ST1633 are all mainline |
| **Out-cell GFF (G+F+F)** | Film sensors under a cover lens | Cheapest; slightly lower transmittance than glass sensors | **What the stocked candidates use.** Winstar `C`/`C1`/`C2` are GFF; Focus "CB"/"-C" and Displaytech "PTS" do not state their stack (**UNVERIFIED**, likely GFF or G+G, optically bonded) | Same |

**Controllers on the candidates:**

| Controller | Candidate | Generation | Points | Report rate | Palm / glove / wet | Mainline (Debian) | Tuning for an extra lens |
|---|---|---|---|---|---|---|---|
| Goodix **GT911** | Focus E35GH | Mature (2012 family) | 5 on this module (the IC supports up to 5 per Goodix) | ≈100 Hz (vendor claim, UNVERIFIED) | Water and glove modes exist in Goodix config (vendor claim, UNVERIFIED) | `goodix.c`, `=m` | **Yes:** the driver uploads a 186-byte config blob from `goodix_<id>_cfg.bin`, or from `goodix/<name>` through the DT property `goodix,config-name`. The vendor must supply the blob tuned for the final lens |
| FocalTech **FT5426G / FT5436** | Newhaven 2.9" / Focus E30RA | Mature | 10 (Newhaven states 10-point) / multipoint | UNVERIFIED | UNVERIFIED | `edt-ft5x06.c`, `=m` | Partly: threshold, gain and offset are writable through the driver's sysfs attributes; deep retuning is firmware (vendor) |
| Sitronix **ST1633I** | Displaytech | Mature | UNVERIFIED | UNVERIFIED | UNVERIFIED | `st1232.c` (`sitronix,st1633`), `=m` | No driver-side tunables. Retuning is vendor firmware only |

**Touch verdict:**

- In-cell and on-cell are **not obtainable** today in a compliant, stocked, driver-backed part.
- The best realistic choice is an **optically bonded out-cell projected-capacitive panel on GT911 or FT5x06**, both of which Debian enables.
- GT911 has the edge because its full configuration can be shipped from Linux as a firmware file, so the lens retune is reproducible in OpenDAP's tree.
- For a semi-custom production module, ask for **G+G or OGS with full OCA lamination** and a **GT911 or newer Goodix/FocalTech part with a mainline driver**. Avoid Hynitron and Ilitek, which Debian has off.

## 7. Cover glass, bonding and stack

**Two ways to build the front:**

1. **Use the module's own CTP lens as the front glass.** This needs a custom lens outline, black border print and thickness from the maker. It gives the thinnest stack and no retune, but brings MOQ and NRE.
2. **Add a separate lens over the stock module.** This adds the lens and its bond to the stack and moves the finger away from the sensor, so the touch controller needs retuning.

**Stack budget for option 2 on the Focus E35GH:**

| Layer | Thickness (mm) | Note |
|---|---|---|
| Added cover lens, chemically strengthened aluminosilicate (Gorilla- or Dragontrail-class) | 0.55–0.7 | 0.7 mm is the usual handheld choice; 1.1 mm for IP65 drop margin |
| OCA full lamination | 0.15–0.2 | Optical bonding: no internal reflections, better sunlight contrast, stiffer |
| Module (CTP plus LCD plus backlight) | 5.50 | From the spec |
| **Total** | **≈ 6.2–6.4** | An air gap instead of OCA saves nothing in z (gasket ≥ 0.3 mm) and adds two reflecting surfaces (≈ 8 % reflectance), which hurts outdoor readability. **Bond it** |

**Retune:**

- The sensor-to-finger distance grows from the module's lens (UNVERIFIED, typically 0.5–1.1 mm) to about 1.2–2.0 mm.
- GT911 handles this with a new config blob loaded by the mainline driver.
- FT5x06 handles it partly, through threshold and gain.
- For ST1633, ask Displaytech for retuned firmware.

**Makers that offer custom lenses with bonding:**

- **Focus LCDs:** custom CTP; optical bonding is stated on the E30RA spec.
- **Winstar:** catalog codes for GFF+OCA, GFF+OCR, G+G+OCA and G+G+OCR, verified in the WF35 datasheet's part-number key.
- **Newhaven, Displaytech, Orient Display:** custom display work is advertised; terms are UNVERIFIED.
- **Shenzhen module houses** (Wisecoco, Kingtech, East Rising): MOQ 1–5k and NRE $1–5k are **UNVERIFIED** industry figures.
- Ask for the lens to be **bonded by the module maker**, so the touch tuning ships tuned for the final stack.

## 8. Battery and outdoor readability

- **Backlight power** dominates a transmissive LCD:

  | Module | Backlight at full |
  |---|---|
  | E35GH | 15 V × 60 mA ≈ **0.9 W** |
  | Displaytech | ≈ **0.75 W** |
  | E30RA-I-MW350-C | ≈ 0.38 W, at 350 nit |

  - Run the backlight from a PWM- or analog-dimmed boost LED driver, controlled by the ambient light sensor (FPT-014). Use about 150–250 nit indoors and full brightness only in sunlight.
  - Assuming roughly linear luminance against current, 200 nit on the E35GH is ≈ 0.2 W (UNVERIFIED until measured).
- **Logic power** is small:
  - E30RA IDD 38 mA at 3.3 V;
  - Winstar ILI9488 13.6 mA;
  - Displaytech "TBD".
  - The ILI9488 has idle and sleep commands, but whether the mainline driver uses them beyond `disable`/`unprepare` is UNVERIFIED.
- **Outdoor:** 800–850 nit with optical bonding is the best available here. Contrast is modest (700:1), but IPS gives full viewing angles. No transflective 2:3 touch part was found.
- **AMOLED** would save power on dark UIs, but no stocked, compliant, driver-backed part exists (§3.3).

## 9. Not covered, and why

| Item | Status | How it was covered instead |
|---|---|---|
| Web search | The session's web-search budget was exhausted, and Bing, DuckDuckGo and Mojeek blocked scripted queries | Parametric search directly on Digi-Key's category (LCD, OLED, Graphic, category 107); FindChips for Mouser, TME and Newark; maker product pages and spec PDFs fetched directly |
| BuyDisplay / East Rising | HTTP 403 | Not covered; semi-custom 2:3 options from them are **UNVERIFIED** |
| Crystalfontz, Riverdi, Raystar, Kingtech, Seeed, Adafruit, DFRobot | Only Digi-Key-listed parts were seen (Adafruit and DFRobot entries are SPI or ESP32 boards) | No compliant DSI touch part surfaced; maker-direct catalogs not searched |
| LCSC, Arrow, Farnell stock | FindChips showed no rows for the Focus parts | Treated as 0 / not listed |
| Handheld-console spare panels | Not searchable (marketplaces) | See the [sourcing report](https://github.com/opendap-project/board/blob/main/docs/research/2026-10-sourcing-alternatives.md) §2.3: no touch and no lifecycle |
| Orient 2.35" outline, Displaytech touch stack, Focus CTP stack, GT911 report rate | Datasheets not fully parsed or the figures not published | Marked **UNVERIFIED** |
| Hardware validation | No samples | Next steps below |

## 10. Next steps

1. Order **2 × E35GH-I-MW800-CB** and **2 × DT035CTFT-IPS-SHB-PTS** now, because stock is thin and the data is dated 2026-10-22.
2. Ask Focus LCDs for:
   - a production quote at 1k and 5k;
   - lead time and a longevity statement;
   - the CTP stack type;
   - a custom 0.7 mm lens with bonding and its GT911 config;
   - whether a **3.0–3.2" 2:3** variant of the ILI9488 family exists, which would fit the ideal body.
3. Start the **DKMS package** for `panel-ilitek-ili9488` and file the Debian request for `CONFIG_DRM_PANEL_ILITEK_ILI9488=m`.
4. Prototype the ST7796 compatible in `panel-sitronix-st7789v.c` only if Displaytech becomes the production part.
5. Bring both panels up on an RK3566 board. Measure:
   - backlight and logic current at 0, 200 and 800 nit;
   - touch accuracy through the final lens;
   - sunlight readability.

## 11. Sources

- Digi-Key parametric search, category LCD, OLED, Graphic (107), keywords "320 x 480 capacitive", "320 x 480 MIPI", "480 x 854 MIPI", "480 x 640 MIPI touch", "ST7701", "ILI9806E", "ILI9881C", "ST7796 MIPI", "MIPI AMOLED", 2026-10-22.
- Focus LCDs product pages and spec sheets:
  - [E35GH-I-MW800-CB](https://focuslcds.com/product/e35gh-i-mw800-cb/) ([spec](https://focuslcds.com/wp-content/uploads/2025/09/E35GH-I-MW800-CB_Spec.pdf));
  - [E35GB-I-MW850-N](https://focuslcds.com/product/e35gb-i-mw850-n/);
  - [E35KB-I-MW850-C1](https://focuslcds.com/product/e35kb-i-mw850-c1/);
  - [E30RA-I-MW350-C](https://focuslcds.com/product/e30ra-i-mw350-c/) ([spec](https://focuslcds.com/wp-content/uploads/Specs/E30RA-I-MW350-C_Spec.pdf));
  - [E30RA-I-RW500-C](https://focuslcds.com/product/e30ra-i-rw500-c/);
  - [E30RA-I-MS550-N](https://focuslcds.com/product/e30ra-i-ms550-n/);
  - [E24RF-I-MS800-N](https://focuslcds.com/product/e24rf-i-ms800-n/);
  - [E27RA-I-MS800-C](https://focuslcds.com/product/e27ra-i-ms800-c/).
- [Displaytech DT035CTFT-IPS-SHB spec V1.1](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/7157/DT%20Spec%20-%20DT035CTFT-IPS-SHB%20V1.1.pdf).
- [Winstar WF35UTYAIMNG0 datasheet](https://www.winstar.com.tw/d/514/WF35UTYAIMNG0.pdf).
- [Newhaven NHD-3.5-640480EF-MSXP-CTP spec](https://newhavendisplay.com/content/specs/NHD-3.5-640480EF-MSXP-CTP.pdf) (mechanical drawing, 09/05/2025).
- [FindChips](https://www.findchips.com/) for Mouser, TME and Newark stock.
- Kernel (`torvalds/linux` master, 2026-10-22):
  - `drivers/gpu/drm/panel/panel-ilitek-ili9488.c`, `panel-sitronix-st7701.c`, `panel-sitronix-st7703.c`, `panel-sitronix-st7789v.c`, `panel-ilitek-ili9806e-dsi.c`;
  - `drivers/gpu/drm/bridge/waveshare-dsi.c`;
  - `drivers/input/touchscreen/goodix.c`, `edt-ft5x06.c`, `st1232.c` (from git.kernel.org).
- Raspberry Pi downstream `rpi-6.12.y` `drivers/gpu/drm/panel/panel-waveshare-dsi.c` and `vc4-kms-dsi-waveshare-panel-overlay.dts`.
- Debian `linux-config-7.2_7.2.9-1~bpo13+1_arm64.deb` and `linux-config-7.3_7.3~rc7-1~exp1_arm64.deb` (`config.arm64_none_arm64`).
