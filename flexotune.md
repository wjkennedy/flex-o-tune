# Project Brief: Flex-PCB Instrument Tuner

## Objective

Develop an initial KiCad project for a rechargeable electronic instrument tuner built around a flexible PCB.

The immediate goal is not to optimize the tuner electronics. The immediate goal is to produce a physically manufacturable PCB prototype that allows us to explore:

* Flex-PCB shape and stackup
* LED visibility across curved instrument surfaces
* Orientation relative to the player
* Mechanical attachment concepts
* Flex routing between the sensing electronics and the visible LED indicator
* Component placement zones
* Bend behavior
* Strain relief
* Gerber and fabrication output generation

The first prototype should help us answer physical and visual questions quickly.

## Product Concept

The tuner is intended to become visually and physically integrated with the instrument rather than appearing as a conventional plastic clip-on tuner.

The system consists of:

1. A compact electronics section mounted against a relatively concealed instrument surface
2. A flexible PCB tail that wraps around an edge or curve
3. A visible LED array positioned where the musician can see it naturally
4. A piezoelectric contact sensor that detects instrument vibration
5. A microcontroller that estimates pitch and maps tuning error to an LED position
6. A small rechargeable LiPo battery
7. USB charging
8. One or more buttons for power, mode selection, calibration, or reference pitch

The visual behavior should resemble a restrained Knight Rider-style scanning indicator.

When the note is flat, the illuminated position should appear on one side of the array.

When the note is sharp, the illuminated position should appear on the opposite side.

When the note is in tune, the display should settle at the center.

A moving point, short illuminated segment, or fading trail may later be used, but the first board only needs individually addressable or independently controllable LED positions.

## Initial Instrument Applications

The first PCB should support physical experimentation across several instrument geometries.

### Guitar and Bass

The main electronics section may sit on the back of the headstock.

The flex section may:

* Wrap around the outer edge of the headstock
* Run along the tip of the headstock
* Lie flat on a Stratocaster-style headstock
* Follow an angled Ibanez-style headstock
* Present LEDs along a curved edge
* Present LEDs on a small angled viewing surface
* Avoid interference with tuning-machine posts and hardware

### Violin and Viola

Potential mounting areas include:

* Under or adjacent to the chin rest
* Along a chin-rest bracket
* Near the tailpiece
* Along the edge of the instrument where the LED display remains visible
* Near the pegbox or scroll for later variants

The piezo sensor should contact a mechanically active part of the instrument while avoiding permanent alteration.

Do not assume adhesive attachment as the only mounting method.

The PCB geometry should remain compatible with later mechanical concepts such as:

* Thin straps
* Existing chin-rest hardware
* Captured flex channels
* Replaceable brackets
* Removable low-profile mounts
* Instrument-specific fixtures
* Friction-fit or mechanically retained carriers

## Prototype Priorities

Prioritize the following, in order:

1. Manufacturable flex geometry
2. Useful physical dimensions
3. LED orientation and visibility
4. Bend zones and rigid zones
5. Fast fabrication output
6. Simple electrical continuity
7. Expandability into a functional tuner

Do not spend excessive effort optimizing:

* Audio performance
* Pitch detection accuracy
* Battery life
* RF emissions
* Production enclosure design
* Final microcontroller selection
* Final LED current
* Final USB implementation
* Component cost

Those will be addressed after the physical concept is validated.

## Proposed Architecture

Use a two-zone flex-PCB architecture.

### Zone A: Electronics Island

This is the relatively rigid section containing most components.

Include provisional footprints for:

* Microcontroller
* Piezo input connector or solder pads
* Piezo protection and analog front-end
* LiPo battery connector
* Battery charger
* USB connector
* Voltage regulation
* Programming and debugging interface
* Power button
* Mode or calibration button
* Status LED
* Test points

The electronics island may initially be fabricated as flex with a stiffener, rigid-flex, or a separate rigid PCB connected to a flex tail.

For the fastest first iteration, prefer a single flex PCB with localized stiffeners unless the MCP tooling or fabrication constraints strongly favor another approach.

### Zone B: LED Flex Tail

Create an elongated flexible section containing the visible LED array.

The LED tail should support wrapping around a curved instrument surface.

Initial target geometry:

* LED count: 13
* One center LED
* Six LEDs on each side
* Symmetrical placement
* LED pitch: approximately 5 mm
* Illuminated array length: approximately 60 mm
* Flex tail width: approximately 8 to 12 mm
* Additional unpopulated flex length between electronics and LEDs for routing and bend experimentation
* Rounded outside corners
* No sharp internal corners
* No vias in primary bend zones
* No components in dynamic bend zones

Use top-emitting LEDs for the first revision.

Select a common small SMD package suitable for flex assembly, such as:

* 0603
* 0805
* 1206
* PLCC-2

Favor visibility and hand assembly over maximum miniaturization.

The first revision should be comfortable to inspect and populate manually.

## LED Control

The first PCB may use either of these approaches:

### Preferred Exploration Option

Use individually addressable LEDs if they can be powered and controlled simply at low voltage.

Advantages:

* Minimal routing
* Easy animation
* Simple mapping of tuning position
* Flexible prototype behavior

Risks:

* Higher current
* Larger packages
* More complex power behavior
* Potential difficulty at low LiPo voltage

### Conservative Option

Use conventional LEDs controlled by:

* GPIO
* A constant-current LED driver
* A shift register
* A charlieplexed arrangement

For the physical prototype, conventional LEDs with a driver footprint may be preferable if they produce a thinner and more predictable flex layout.

Do not commit irreversibly to either option before comparing footprint size, routing density, current, and flex bend behavior.

## Piezo Input

Include a provisional contact-piezo input circuit.

The piezoelectric element may produce high-voltage transients and has a high source impedance.

Provide space for:

* Input current-limiting resistor
* Bias network
* Clamp diodes
* Optional TVS protection
* AC coupling
* High-impedance buffer
* Gain stage
* Low-pass filtering
* ADC input protection

A reasonable first-pass topology is:

1. Piezo input
2. Series resistor
3. Input clamps
4. Mid-supply bias
5. High-input-impedance op-amp buffer
6. Optional gain and filtering
7. Microcontroller ADC input

The circuit only needs to be plausible and testable in revision one.

Clearly mark analog input and ground test points.

Keep the piezo input and analog front-end away from USB, switching regulators, and high-current LED traces where practical.

## Microcontroller

Use a provisional low-power microcontroller footprint with:

* ADC input
* Sufficient GPIO or serial peripheral support for the LED array
* USB support or a simple external programming interface
* Small package
* Broad software support
* Easy procurement
* Suitable KiCad symbol and footprint availability

Candidates may include:

* RP2040
* RP2350
* STM32
* SAMD21
* AVR DA-series
* ESP32-C3, only if wireless capability is considered useful later

For the first board, favor toolchain simplicity and physical layout flexibility.

A module footprint may be used for the first prototype if it accelerates fabrication, although the final product will likely require a smaller integrated design.

## Power System

Include:

* Single-cell rechargeable LiPo input
* Small JST battery connector or solder pads
* USB-C or USB Micro-B charging input
* LiPo charge-controller footprint
* Power switch or soft-power provision
* Regulated logic supply if required
* LED power rail
* Battery-voltage measurement divider
* Charging-status indication
* Reverse-polarity consideration
* Test points for battery, regulated voltage, USB input, and ground

USB-C should be implemented as a basic USB 2.0 power sink.

Include the correct CC resistors for 5 V power negotiation.

Data lines are optional unless the selected microcontroller uses USB for programming.

The first prototype may use a larger connector than the final product.

## Buttons and Controls

Provide at least two user inputs:

* Power or wake button
* Mode or calibration button

Potential future behaviors include:

* Short press to change string or instrument mode
* Long press to change A reference frequency
* Hold to power off
* Double press to change display behavior
* Calibration or sensitivity adjustment

For revision one, the buttons only need accessible footprints, debouncing provisions, and microcontroller connections.

## Flex-PCB Stackup

Create a practical initial flex stackup proposal suitable for low-volume prototype fabrication.

Preferred baseline:

* Two copper layers
* Polyimide core
* Polyimide coverlay
* Rolled-annealed copper where available
* Approximately 0.1 mm to 0.2 mm total thickness in unsupported flex regions
* Local stiffeners beneath:

  * USB connector
  * Battery connector
  * Microcontroller or module
  * Charger
  * Buttons
  * Programming connector
  * Potentially the LED array, depending on viewing geometry

The LED region may need either:

* No stiffener, allowing it to follow a curve
* A thin segmented stiffener
* A continuous stiffener that creates a defined viewing plane
* Multiple short rigidized LED islands connected by flex bridges

Create notes documenting these alternatives.

## Bend Rules

Apply conservative initial flex rules:

* Avoid 90-degree trace corners
* Use arcs or 45-degree transitions
* Route traces perpendicular to the primary bend axis where practical
* Stagger traces between layers
* Avoid stacked traces on opposite layers in high-strain bend zones
* Avoid vias in bend zones
* Avoid copper pours in dynamic bend zones unless hatched or otherwise justified
* Use teardrops where supported
* Use wide traces for power
* Maintain generous clearance from board edges
* Add fillets where narrow flex sections join wider rigidized sections
* Use curved transitions rather than abrupt neck-down geometry
* Define bend zones in documentation and fabrication drawings

Assume the first prototype may be bent repeatedly during experimentation even if the final product becomes a static-flex design.

## Mechanical Exploration Features

Include physical features that make the PCB useful as a geometry prototype:

* Printed centerline
* Printed bend-zone boundaries
* Ruler or millimeter reference markings
* LED position numbers
* Clearly marked center LED
* Orientation arrows
* Player-view side indication
* Instrument-facing side indication
* Reference curves or bend-radius marks
* At least two alternative trim outlines if practical
* Nonfunctional mechanical tabs that may be removed
* Optional holes or slots for temporary straps, thread, brackets, or fixtures
* Keep-out markings for tuners, chin-rest hardware, or mounting brackets

Add silkscreen or coverlay markings that make it easy to photograph and compare placement options on instruments.

## Suggested Initial Dimensions

Use these as starting values, not immutable constraints:

### Electronics Island

* Width: 25 to 35 mm
* Length: 35 to 50 mm
* Rounded corners
* Local stiffener over the complete electronics area

### Flex Neck

* Width: 8 to 12 mm
* Length: 30 to 60 mm
* No components
* No vias
* Clearly marked bend zone

### LED Array

* 13 LEDs
* Approximately 5 mm pitch
* Approximately 60 mm active length
* Approximately 8 to 12 mm width
* One center LED with distinctive silkscreen marking
* Optional extra LEDs or dummy footprints at the ends for experimentation

Create the board so these dimensions can be changed parametrically or with clearly documented constraints.

## Initial Schematic Blocks

Create separate schematic sheets or clearly separated blocks for:

1. USB and charging
2. Battery and power regulation
3. Microcontroller
4. Piezo analog front-end
5. LED driver or LED chain
6. Buttons and user interface
7. Programming and test points

Use net labels consistently.

Use explicit power flags where required.

Run electrical rules checking and resolve real errors rather than suppressing them broadly.

## PCB Layout Requirements

The first board layout should:

* Place the electronics on a rigidized island
* Place the LED array in a straight initial configuration
* Define flexible transition zones
* Use smooth board-edge curves
* Include fabrication notes
* Include stiffener notes
* Include coverlay opening notes
* Include bend-zone dimensions
* Avoid components under likely mounting hardware
* Keep the analog front-end compact
* Separate analog sensing from LED return currents where practical
* Include a ground plane only where it does not compromise flex behavior
* Avoid unnecessary copper in bend regions

The LED array may be laid out straight even though it will be bent in use.

Do not attempt to pre-curve the board outline merely to imitate an installed shape unless doing so provides a manufacturing or mounting advantage.

## Prototype Modes

Create fabrication variants or documented configurations for three experiments.

### Variant A: Continuous Curved Strip

A continuous LED flex strip that bends around an instrument edge.

### Variant B: Flat Viewing Strip

A straight LED region intended to lie flat on the top or side of a headstock.

### Variant C: Segmented Display

Several small stiffened LED islands joined by flexible necks so the display can approximate a compound curve.

The initial KiCad project may implement Variant A and include notes or alternate outlines for Variants B and C.

## Firmware Context

Firmware is not the primary deliverable, but the hardware should support this conceptual loop:

1. Wake or power on
2. Read buttons
3. Sample piezo signal
4. Detect signal envelope
5. Collect an audio window
6. Estimate fundamental frequency
7. Determine nearest target note
8. Calculate cents error
9. Map cents error to LED position
10. Illuminate or animate the corresponding LED
11. Settle at center when in tune
12. Dim or sleep after inactivity

Pitch detection may later use:

* Autocorrelation
* YIN
* McLeod Pitch Method
* Harmonic product spectrum
* FFT-assisted estimation

Do not shape the hardware around a computationally expensive FFT requirement if a lower-power time-domain method is likely to work better.

## First Deliverables

Produce the following:

1. KiCad project
2. Schematic
3. PCB layout
4. Flex stackup proposal
5. Bend-zone drawing
6. Stiffener drawing or fabrication notes
7. Coverlay opening notes
8. Bill of materials
9. Pick-and-place file
10. Gerber files
11. NC drill files
12. Board outline file
13. Fabrication drawing
14. Assembly drawing
15. 3D board render
16. PDF plot of schematic
17. PDF plot of PCB layers
18. Design-rule-check report
19. Electrical-rule-check report
20. README explaining assumptions and open decisions

Package manufacturing outputs into a clearly named ZIP archive.

## KiCad MCP Working Instructions

Use the connected KiCad MCP server to create and inspect the project rather than merely describing the design.

Work incrementally.

After each major stage:

* Save the project
* Run the relevant checks
* Inspect the board visually
* Record assumptions
* Avoid silently substituting unavailable footprints
* Prefer standard KiCad library components
* Create custom footprints only where necessary
* Keep custom library assets inside the project
* Use meaningful reference designators
* Use meaningful net names
* Keep all generated fabrication outputs under a dedicated output directory

Suggested project structure:

```text
flex-instrument-tuner/
├── README.md
├── hardware/
│   ├── flex-instrument-tuner.kicad_pro
│   ├── flex-instrument-tuner.kicad_sch
│   ├── flex-instrument-tuner.kicad_pcb
│   ├── flex-instrument-tuner.kicad_dru
│   ├── symbols/
│   ├── footprints/
│   └── 3dmodels/
├── docs/
│   ├── stackup.md
│   ├── mechanical-exploration.md
│   ├── fabrication-notes.md
│   └── open-decisions.md
├── manufacturing/
│   ├── gerbers/
│   ├── drill/
│   ├── assembly/
│   ├── bom/
│   └── flex-instrument-tuner-fab.zip
└── firmware/
    └── README.md
```

## Open Decisions to Document

Do not block the first layout waiting for these decisions.

Document them clearly:

* Final instrument mounting method
* Electronics location for violin and viola
* Exact LED package
* Addressable versus conventional LEDs
* Final microcontroller
* Final piezo element
* Required analog gain
* Static-flex versus dynamic-flex rating
* USB-C versus smaller charging contact system
* LiPo size and location
* Separate rigid board versus single stiffened flex
* LED viewing angle
* Need for optical diffuser
* Need for waterproofing or sweat resistance
* Interaction with instrument finish
* Serviceability and removability
* Player-facing brightness
* Orchestra reference settings such as A440 and A442

## First Execution Step

Begin by creating a mechanical-first KiCad PCB with:

* One electronics island
* One narrow flex neck
* One 13-LED display strip
* Clearly marked bend zones
* Provisional component footprints
* A valid two-layer flex stackup
* Smooth board-edge geometry
* Stiffener and coverlay notes
* Enough routing to establish realistic trace density

Generate an initial 3D render and fabrication plots as soon as the board outline and LED placement exist.

The first milestone is a visually inspectable, manufacturable flex coupon that can be printed at 1:1 scale, placed against guitar, violin, and viola surfaces, and used to evaluate geometry before the final circuit is optimized.

