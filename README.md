
# Make Cell (MCELL): Synthesis of On-Chip Passive Components 
## Motivation
Silicon foundries generally offer a limited set of models for passive components in their PDKs. Transformers, despite their importance in single-ended to differential signal conversion and matching network design, are often excluded, with no models provided in the PDK. While models for inductors are typically available, they are usually constrained to a narrow frequency range, up to around 20 GHz insufficient for designers working in the millimeter-wave (MW) frequency range.

To address this issue, EM simulators are often used to model the missing passive components. However, running design iterations and optimizations with an EM simulator in the loop is a slow and laborious process, that may often lead to tape-out delays and increased development costs.

**Make Cell (MCELL) is a novel tool that provides the automatic synthesis of on-chip passive components, generating models that are customized to the required geometry and frequency range for any process node. MCELL enables synthesising components with optimal area and performance for a given application, while significantlly accelerating the desing process.**

## Supported components
MCELL currently supports synthesis of the following components:
- Spiral inductor (inductor-spiral): common mode inductor with arbitrary number of windings
- Symmetric inductor (inductor-symmetric): differential inductor with arbitrary number of windings
- 1:1 transformer (transformer1o1): the secondary is placed in the metal layer directly below the primary to maximize the coupling coefficient
- 1:2 transformer (transformer1o2): the secondary is placed in the metal layer directly below the primary to maximize the coupling coefficient
- 2:1 transformer (transformer2o1): the secondary is placed in the metal layer directly below the primary to maximize the coupling coefficient
- 2:2 transformer (transformer2o2): the secondary is placed in the metal layer directly below the primary to maximize the coupling coefficient
- Spiral transformer (transformer-spiral): the primary and secondary are placed on the same metal layer, supporting an arbitrary number of windings on both sides

These components can be designed with either rectangular or octagonal geometries and can include an optional patterned ground shield. For symmetric components (all except the spiral inductor), a DC feed can be integrated at the center of symmetry.

![](/mcell/img/all2-1024x609.png)

## How it works 
MCELL can performe the following tasks:
- Generate DRC clean set of GDS files for a given component

  Example:
  ```
  mcell -d 150:200:10 -w 6:10:2 -s 6:10:2 -n 2:5:1 -t inductor-symmetric --pin-lenght=20 --top-metal=TM2 --rect-geometry
  ```
  Output is folder `gdsFile` populated with DRC clear GDS files.
- Generate DRC clean set of GDS files for a given component, and prepare EMX simulation

  Example:
  ```
  mcell -d 150:200:10 -w 6:10:2 -s 6:10:2 -n 2:5:1 -t inductor-symmetric --pin-lenght=20 --top-metal=TM2 --rect-geometry --generate-em
  ```
  The output consists of the following:
  - A `gdsFile` folder containing DRC-clean GDS files
  - A `yFile` folder where simulation results (Y-parameters) will be saved
  - A script, `runEmx.sh`, which needs to be executed to start the simulation

  To start the simulation for all GDS files in the `gdsFile` folder, execute the script by running `source runEmx.sh`. All simulation results  (Y-parameters), will be saved in the `yFile` folder.
    
- Generate FastHenry input files for a given component

  In addition to EMX, MCELL supports FastHenry, which can extract inductance and resistance up to two orders of magnitude faster than a full-wave EM solver.  
  Example:
  ```
  mcell -d 150:200:10 -w 6:10:2 -s 6:10:2 -n 2:5:1 -t inductor-symmetric --pin-lenght=20 --top-metal=TM2 --rect-geometry --fast-henry
  ```
  The output consists of the following:
  - A `fastHenryFile` folder containing FastHenry input files
  - A `zFile` folder where simulation results (Z-parameters) will be saved
  - A script, `runFastHenry.sh`, which needs to be executed to start the simulation
 
  To start the simulation for all FastHenry input files in the `fastHenryFile` folder, execute the script by running `source runFastHenry.sh`. All simulation results  (Z-parameters), will be saved in the `zFile` folder.


Apply for 30 days free evaluation license.

### Contact: info@passivelib.com
