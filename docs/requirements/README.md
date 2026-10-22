# Touch faceplate requirements

Requirements for the touch faceplate, the default faceplate of the OpenDAP T1.

| ID | Requirement | Priority | Status |
|---|---|---|---|
| FPT-001 | The touch faceplate shall be the default faceplate. | must | Confirmed |
| FPT-002 | The touch faceplate shall have a touch display. | must | Confirmed |
| FPT-003 | The touch faceplate shall include a status LED. | must | Confirmed |
| FPT-004 | The touch faceplate shall include a microphone. | must | Confirmed |
| FPT-005 | The touch faceplate shall meet the [faceplate interface](https://github.com/opendap-project/t1/blob/main/docs/specs/faceplate-interface.md) defined in the t1 repository. | must | Confirmed |
| FPT-006 | The touch faceplate shall keep its own mechanics in this repository. | must | Confirmed |
| FPT-007 | Display: 3.5" IPS, 480 × 800, MIPI DSI (ST7701S controller), with mainline Linux support. | should | Withdrawn |
| FPT-008 | Touch controller: Goodix GT911 (mainline driver). | should | Draft |
| FPT-009 | Stack: cover glass, touch and LCD laminated on an aluminum tray with a graphite spreader that also takes heat from the SoC. | should | Withdrawn |
| FPT-010 | Front sensors: ambient light sensor and microphone, next to the status LED. | should | Draft |
| FPT-011 | Faceplate outline 53 × 90.5 × 3.6 mm. | should | Withdrawn |
| FPT-012 | The display panel shall already be supported by a mainline Linux panel driver (no kernel patch). | must | Withdrawn |
| FPT-013 | The display shall be a touch panel chosen for the best fit with the project requirements and commercial availability; a 3.5" IPS panel is the reference, not a fixed size or technology. | must | Confirmed |
| FPT-014 | The touch faceplate shall have a display and an ambient light sensor for dimming it (every faceplate has a display). | must | Confirmed |
| FPT-015 | The display driver shall be on the touch faceplate. | must | Confirmed |
| FPT-016 | The touch faceplate's status LED shall be controllable by developers, like the user LED on development boards. | must | Confirmed |
| FPT-017 | The main microphone shall be on the touch faceplate. | must | Confirmed |
| FPT-018 | The display panel should have a mainline Linux panel driver. If no suitable panel has one, OpenDAP may add the driver. | must | Confirmed |
| FPT-019 | For the display, availability outranks existing Debian support: choose the panel that is actually available, and provide its driver through DKMS or an upstreamed panel compatible if needed. | must | Confirmed |
| FPT-020 | The display shall keep an aspect ratio in about the same range as the body (height-to-width ≈1.5 to 1.6 in portrait), with **2:3 preferred**. | must | Confirmed |
| FPT-021 | The touch sensor shall use the best available technology (for example in-cell or on-cell projected capacitive, with multi-touch, good palm rejection and glove/wet operation where available). | must | Confirmed |
| FPT-022 | The display may need an additional cover glass (lens) so it fits and seals in the case; the stack shall account for it. Prefer the touch module's own cover lens or an off-the-shelf cover glass; a custom cover glass is the only custom part the project may accept (T1-038). | must | Confirmed |
| FPT-023 | Front layout, seen from the front: above the display, the ambient light sensor centered and the status LED to the right, not all the way to the corner. | must | Confirmed |
| FPT-024 | Front layout, seen from the front: the main microphone centered below the display. | must | Confirmed |
| FPT-025 | The status LED should be RGB. If no compliant RGB part fits, a single colour is acceptable. | should | Confirmed |

## Context

- FPT-007 to FPT-011: Adopted from the earlier experiments on 2026-10-21 as **Draft**: consistent with the decisions already taken and with the product philosophy, pending the maintainers' confirmation. Part availability still has to be checked against the sourcing rule (T1-010).
- Withdrawn on 2026-10-21 after the [experiment review](https://github.com/opendap-project/t1/blob/main/docs/research/2026-10-experiment-review.md): FPT-007, FPT-009, FPT-011.
- Mainline-supported panels already used on RK3566 handhelds (for example the Anbernic RG353P panel, `panel-newvision-nv3051d`) show which panels qualify.
- FPT-012 was replaced by the requirement above (2026-10-21): a mainline driver is preferred, not mandatory. Debian's kernel may also need the driver enabled; asking Debian is the standard path.
- The concept art (`docs/concept-art/touch-faceplate-three-quarter.png`) shows the status LED above the display on the right and the microphone below it at the center. Use it as a visual reference only.
- FPT-023 and FPT-024 are the preferred layout. If the microphone or the status LED do not fit around the display panel, they may move to other positions (maintainers, 2026-10-22).
