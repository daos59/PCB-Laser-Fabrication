# PCB Laser Fabrication

Development of a laser-assisted process for prototyping single- and double-sided printed circuit boards (PCBs), combining Gerber processing, laser mask ablation, chemical etching, solder mask processing, manual drilling and component assembly.

## Overview

This project documents the development of an in-house PCB prototyping workflow using accessible digital fabrication tools. The process starts with a PCB design exported as Gerber files and uses a diode laser to selectively remove temporary masks and solder mask while maintaining physical registration between operations.

I mainly use **EasyEDA** for PCB design, **FlatCAM** for Gerber processing and **LightBurn** for laser operations. The workflow is not tied to EasyEDA; any EDA software capable of exporting the required Gerber files can be used.

A key part of the process is a reusable sacrificial-board jig that establishes a physical reference for the copper-clad board. This makes it possible to remove, flip and return the PCB to the laser while maintaining alignment, including during double-sided fabrication.

## Process Flow

```text
PCB Design → Gerber Export → FlatCAM → SVG
        ↓
Physical board measurement
        ↓
Registration jig
        ↓
Copper preparation + paint mask
        ↓
Laser ablation — Side 1
        ↓
Controlled flip + mirrored Side 2
        ↓
Laser ablation — Side 2
        ↓
Chemical copper etching
        ↓
Cleaning and drying
        ↓
Laser outline marking + mechanical cutting
        ↓
Solder mask + UV curing
        ↓
Selective pad ablation
        ↓
Manual drilling
        ↓
SMD assembly → THT assembly
```

## 1. PCB Design and Gerber Export

The PCB is designed in an EDA program capable of exporting Gerber files. I mainly use **EasyEDA**, since it is the platform I have worked with the most.

The fabrication workflow uses the required copper-layer information together with the drill data.

## 2. Gerber Processing

The Gerber and drill files are processed in **FlatCAM** and converted into geometry that can be imported into laser-control software.

For this workflow I prefer **SVG** because it preserves vector geometry and LightBurn can interpret the curves directly.

## 3. Physical Board Measurement

Before preparing the laser job, the actual copper-clad laminate is measured rather than relying only on its nominal dimensions.

In practice, I have encountered dimensional variations of approximately 1 mm between edges. The measured geometry is therefore used to prepare the physical registration reference.

## 4. Registration Jig

A sacrificial sheet, normally MDF or a similar material, is firmly fixed to the laser bed.

An opening matching the measured copper-clad board is cut into this material. A small semicircular access feature is added along one edge so the board can be removed without disturbing the jig.

```text
Sacrificial board
┌─────────────────────────────┐
│    ┌───────────────────┐    │
│    │                   │    │
│    │    COPPER-CLAD    │ )  │
│    │       BOARD       │    │
│    └───────────────────┘    │
└─────────────────────────────┘
```

The jig remains fixed throughout the fabrication process and becomes the physical reference for subsequent operations. Whenever possible, one corner is used as a consistent reference.

## 5. Copper Surface Preparation

The copper is cleaned thoroughly with isopropyl alcohol.

A light surface sanding can also be performed using approximately **300–350 grit** abrasive paper. I prefer sanding lightly in one direction to keep the surface finish uniform.

The purpose is to improve adhesion of the temporary paint mask without unnecessarily damaging the copper.

## 6. Temporary Etch Mask

The copper surface is covered with a thin paint layer that acts as an etch resist. Spray paint, acrylic paint or another coating can be used provided that it adheres adequately to the prepared copper.

For a double-sided PCB, both copper surfaces can be prepared before laser processing.

## 7. Laser Mask Ablation

The PCB geometry is imported into **LightBurn** and aligned to the reference established by the jig.

The geometry is prepared so that the laser removes paint from the regions where copper must later be chemically etched while preserving the mask over the traces and pads that must remain.

The objective is only to remove the thin paint layer, not to machine the copper or laminate. Laser parameters therefore depend on the machine, coating and motion characteristics and must be calibrated for the specific setup.

After ablation, loose paint residue is removed carefully with a soft-bristle brush.

## 8. Double-Sided Registration

Double-sided alignment is one of the central elements of the process.

After the first side is processed, the PCB is removed while the sacrificial jig remains completely fixed. A flip direction is defined in advance—X or Y—and the board is returned to the same physical opening.

The opposite copper layer is imported and mirrored as required. It is then aligned using the corresponding opposite corner according to the selected flip axis.

```text
Side 1
  ↓
Remove PCB — keep jig fixed
  ↓
Flip around known X or Y axis
  ↓
Mirror opposite copper layer
  ↓
Reference corresponding opposite corner
  ↓
Side 2
```

This combination of a fixed jig, known flip direction and corner references provides a repeatable physical registration method for double-sided prototyping.

## 9. Chemical Etching

After laser processing, the exposed copper is removed using a **ferric chloride etchant**.

The process is carried out in a chemically compatible container and monitored continuously. Gentle movement helps prevent residues from remaining concentrated on the board surface.

Care is taken not to scratch the remaining paint mask because any newly exposed copper can also be attacked. Excessive etching time can also produce lateral undercutting beneath the mask and reduce trace quality.

> **Safety:** Ferric chloride requires appropriate personal protective equipment, compatible containers and adequate ventilation. Preparation, concentration and temperature should follow the instructions for the specific etchant being used.

## 10. Cleaning and Drying

Once the unwanted copper has been removed, chemical residue is removed from the board and the PCB is rinsed and cleaned.

The board is then dried completely. Avoiding prolonged moisture on exposed copper helps reduce oxidation before subsequent processing.

## 11. Board Outline and Mechanical Cutting

The PCB can be returned to the same laser jig using the established reference.

I normally use the laser to **mark the final board outline rather than completely cut through the laminate**. The substrate tends to carbonize during aggressive laser cutting, and traces positioned close to an edge can be affected.

The final separation of the board is therefore performed mechanically with a saw or another appropriate tool.

## 12. Solder Mask Application and UV Curing

A UV-curable solder mask is spread across the PCB as evenly and as thinly as practical.

Keeping the layer thin simplifies later pad exposure and reduces the laser energy required.

The mask is cured using ultraviolet light. Depending on the PCB dimensions, a dedicated UV source or a UV nail-curing lamp can be used. For double-sided boards, I prefer processing one side at a time.

## 13. Selective Pad Ablation

After curing, the pad geometry is aligned again in LightBurn and the laser selectively removes the solder mask from the areas that will be soldered.

Energy control is particularly important during this operation. Excessive heating can contribute to copper-trace lifting, so the laser parameters must be adjusted to the specific solder mask.

Mask color also affects laser interaction; lighter colors can require different settings than darker masks.

## 14. Pad Preparation

The exposed pads are cleaned with isopropyl alcohol. Cotton swabs can be used for localized cleaning and, when necessary, the pad surface can be polished very lightly with fine abrasive paper.

The objective is to leave a clean copper surface before assembly.

## 15. Drilling

Drilling is currently performed manually.

The drill data remains part of the digital workflow and provides the required hole locations, but automated drilling is not yet integrated into the process.

A future development objective is to create a sufficiently repeatable registration method to transfer the existing laser reference to an automated drilling operation.

## 16. Component Assembly

After drilling and final cleaning, the components can be assembled.

I normally solder **SMD components first** and **THT components afterward**, which keeps the board easier to access during the initial assembly stages.

## Repeatability

When several identical PCBs are required, the sacrificial jig can remain installed on the laser bed.

This preserves the physical reference and allows additional copper-clad boards of the same dimensions to reuse the established setup instead of rebuilding the alignment system for every board.

## Process Development and Lessons Learned

This workflow has been refined through repeated fabrication trials. Some of the most important practical observations have been:

- Physical copper-clad dimensions should be measured rather than assumed.
- A fixed registration jig greatly improves repeatability.
- Surface preparation strongly affects paint adhesion.
- The temporary mask should remain uniform and free from scratches.
- Residue after laser ablation can interfere with chemical etching.
- Etching must be monitored to reduce lateral undercutting.
- A thin solder-mask layer makes selective pad exposure easier.
- Solder-mask color influences laser behavior.
- Excessive laser heating can contribute to trace lifting.
- Maintaining the same physical reference simplifies multiple fabrication stages.

## Current Development

Current work is focused on improving the repeatability and automation of the workflow, particularly the drilling stage and the transfer of the established physical reference between different fabrication operations.

## Scope

This repository documents an experimental, low-volume PCB prototyping process. It is not intended to replace professional or industrial PCB manufacturing.

The project focuses on integrating **PCB design, CAM processing, laser fabrication, mechanical registration, chemical etching, solder-mask processing and manual electronics assembly** into a practical in-house rapid-prototyping workflow.

## Author

**Daniel Andrés Oliva Salvatierra**  
Mechatronics Engineering · Digital Manufacturing · Rapid Prototyping

GitHub: [@daos59](https://github.com/daos59)
