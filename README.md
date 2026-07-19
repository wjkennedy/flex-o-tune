# FlexOTune prototype (Revision A)

This KiCad project begins the physical/visual flex-PCB experiment described in
[`flexotune.md`](flexotune.md). It favors a hand-assemblable, manufacturable
geometry prototype over final audio or power performance.

## Baseline configuration

- Variant A: continuous, straight LED flex strip for wrapping an instrument edge.
- Electronics island: 30 × 40 mm, intended for a local stiffener.
- Flex neck: 10 × 45 mm; no components or vias; initial bend target: R ≥ 5 mm.
- LED tail: 11 × 64 mm, 13 positions at 5 mm pitch, with a distinct centre.
- Provisional controller: RP2040 in a Raspberry Pi Pico module form factor.
- LEDs: WS2812B-2020 one-wire chain; power, ground, and data run through the tail.

## Flex fabrication intent

Request a two-layer polyimide flex construction with coverlay and rolled-annealed
copper where available. The island receives a local stiffener; the neck does not.
Avoid copper pours, vias, and component pads in the neck. Confirm final stackup,
coverlay openings, stiffener material/thickness, and minimum bend radius with the
selected fabricator before ordering.

## Schematic status

`FlexOTune.kicad_sch` contains USB-C sink resistors, MCP73831 LiPo charging,
an RP2040 Pico-module placeholder, two buttons, and a 13-device WS2812B-2020
data chain. The piezo input now includes a 1 MΩ series protector, rail clamps,
AC coupling, a 1.65 V bias, an MCP6001 unity-gain buffer, and an ADC isolation/
filter network.

The USB-C VBUS sink is explicitly flagged as a power source for ERC. ERC now
has zero errors. Its remaining warnings are schematic-grid artifacts from the
MCP integration and one intentionally unused end-of-chain LED output.

## Open decisions

- Final LED package/current limit and whether the tail itself is stiffened.
- Exact instrument-specific mechanical attachment and trim tabs.
- Final RP2040 integration footprint and programming method.
- Final LiPo protection, charging current, and USB data requirements.
- Piezo front-end clamp/current values after instrument testing.
# flex-o-tune
