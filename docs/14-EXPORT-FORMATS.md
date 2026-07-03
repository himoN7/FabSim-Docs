# Complete Export Formats Guide

**Version:** 0.4.0  
**Last Updated:** July 2026  
**Audience:** Users, CAD Engineers, Production Teams, Developers

---

## Table of Contents

1. [Overview](#overview)
2. [3D Mesh Formats](#3d-mesh-formats)
3. [FEA/Analysis Formats](#feaanalysis-formats)
4. [Loom/Machine Formats](#loopmachine-formats)
5. [Vector Graphics Formats](#vector-graphics-formats)
6. [Raster Image Formats](#raster-image-formats)
7. [Data Export Formats](#data-export-formats)
8. [Import Formats](#import-formats)
9. [Format Selection Guide](#format-selection-guide)
10. [Batch Export](#batch-export)
11. [Troubleshooting Export](#troubleshooting-export)

---

## Overview

### Export Architecture

```
FabSim Data
    ↓
[Format-Specific Exporter Service]
    ↓
[Geometry Builder / Data Serializer]
    ↓
[File Writer]
    ↓
Output File (.obj, .stl, .wif, etc.)
```

### Available Export Formats (14+)

| Format | Type | File Extension | Best For | Size |
|--------|------|---|---|---|
| **OBJ** | 3D Mesh | `.obj` + `.mtl` | 3D visualization, CAD | Medium |
| **STL** | 3D Mesh | `.stl` | 3D printing prep | Large |
| **3MF** | 3D Mesh | `.3mf` | 3D printing submission | Medium |
| **Abaqus INP** | FEA | `.inp` | Finite element analysis | Medium |
| **COMSOL** | FEA | `.txt` | COMSOL simulation | Small |
| **SVG** | Vector | `.svg` | Web use, design tools | Small |
| **DXF** | Vector | `.dxf` | AutoCAD, CAM | Small |
| **WIF** | Loom | `.wif` | Weaving machine | Tiny |
| **CSV Liftplan** | Loom | `.csv` | Loom control software | Tiny |
| **FABJC** | Jacquard | `.jc5`, `.jc6`, `.jc7` | Jacquard machines | Tiny |
| **PNG** | Raster | `.png` | Screenshots, documentation | Variable |
| **Analysis CSV** | Data | `.csv` | Spreadsheet analysis | Small |

---

## 3D Mesh Formats

### OBJ Format

#### **What is OBJ?**
```
- Wavefront OBJ format (widely supported)
- Plain text (human-readable)
- Paired with .mtl material file
- Standard for 3D visualization
- No proprietary licensing
```

#### **OBJ File Structure**
```
obj_file.obj (geometry)
├── Vertices (v x y z)
├── Texture coordinates (vt u v)
├── Normals (vn x y z)
└── Faces (f references to v/vt/vn)

obj_file.mtl (materials)
├── Material name
├── Color (Kd r g b)
├── Specularity (Ks r g b)
├── Shininess (Ns value)
└── Texture maps (map_Kd filename)
```

#### **Export Settings**
```
1. File → Export → OBJ
2. Configuration:
   - Output Scale: mm / cm / m / inches
   - Include Colors: Yes/No
   - Include Materials: Yes/No
   - Optimize Mesh: Yes/No (reduce vertex count)
   - Material Library: Separate .mtl file

3. Output:
   - model.obj (vertex/face data)
   - model.mtl (material definitions)
   - model_textures/ (folder with PBR maps if applicable)
```

#### **OBJ Quality Settings**
```
Normal Detail: 
- Low: 50k polygons, fast loading
- Medium: 200k polygons, balanced
- High: 500k+ polygons, very detailed

Texture Resolution:
- 512×512: Web suitable
- 1024×1024: High detail
- 2048×2048: Professional quality

Optimization:
- Weld nearby vertices: Reduce duplicate verts
- Remove unused materials: Clean .mtl file
- Flip normals: Correct orientation if needed
```

#### **Use Cases**
```
✓ 3D visualization in Blender, Maya, 3ds Max
✓ Web 3D viewers (Three.js, Babylon.js)
✓ Product visualization
✓ Marketing materials
✓ Design review with clients
✓ Integration with game engines
✓ VR/AR applications
```

#### **Example Workflow: OBJ to Blender**
```
1. Export from FabSim as OBJ
2. Open Blender
3. File → Import → Wavefront (.obj)
4. Select model.obj
5. Materials import as referenced .mtl
6. Textures load from texture folder
7. Ready for rendering, animation, compositing
```

### STL Format

#### **What is STL?**
```
- Stereolithography format
- Binary or ASCII (binary recommended)
- 3D printing standard
- Mesh-only (no materials)
- Widely supported by slicing software
```

#### **STL Characteristics**
```
Binary STL:
- Smaller file size
- Faster to read/write
- Cannot view as text
- Preferred for large models

ASCII STL:
- Human-readable
- Larger file size
- Slower processing
- Useful for debugging
```

#### **Export Settings**
```
1. File → Export → STL
2. Configuration:
   - Format: Binary (recommended)
   - Output Scale: mm / cm / m / inches
   - Mesh Quality: Low/Medium/High
   - Merge Separate Objects: Yes/No
   - Auto-orient normals: Yes/No

3. Output:
   - model.stl (single binary file)
```

#### **Preparation for 3D Printing**
```
1. Export at actual target size (mm recommended)
2. Use High mesh quality
3. Open in slicing software:
   - Prusaslicer
   - Cura
   - Simplify3D
4. Check for:
   - Correct scale
   - Proper orientation
   - No holes or gaps
5. Generate supports if needed
6. Slice and print
```

#### **Use Cases**
```
✓ 3D printing (FDM, SLA, SLS)
✓ Pre-production prototypes
✓ Physical samples for testing
✓ Tactile design verification
✓ Manufacturing process validation
```

#### **Example: Print Preparation**
```
1. Export STL at 100 mm scale
2. Open in Prusaslicer
3. Load printer profile (Prusa i3 MK3S+)
4. Orient for optimal print (flat surface down)
5. Supports added automatically
6. Check preview
7. Export G-code
8. Send to printer
```

### 3MF Format

#### **What is 3MF?**
```
- 3D Manufacturing Format
- ZIP-based container
- Includes geometry + materials + textures
- Microsoft-backed standard
- Supported by major slicing software
- Superior to STL (preserves colors, materials)
```

#### **3MF File Structure**
```
model.3mf (ZIP archive)
├── 3D/
│   └── model.xml (geometry + materials)
├── Textures/
│   ├── texture1.png
│   └── texture2.png
└── [Content_Types].xml (manifest)
```

#### **Export Settings**
```
1. File → Export → 3MF
2. Configuration:
   - Output Scale: mm / cm / m / inches
   - Include Colors: Yes/No
   - Include Materials: Yes/No
   - Preserve UV Coordinates: Yes/No
   - Compress: Yes/No

3. Output:
   - model.3mf (single ZIP file)
   - Includes all textures and metadata
```

#### **Color & Material Export**
```
3MF supports:
- Per-vertex colors (detailed coloring)
- Material assignments
- PBR properties (roughness, metallic)
- Texture maps
- Transparency (alpha channel)

Result:
- Slicing software sees colors
- 3D printing preserves warp/weft coloring
- Photorealistic digital preview
```

#### **Use Cases**
```
✓ Professional 3D printing (color support)
✓ Modern slicing software (Cura, PrusaSlicer)
✓ Material Jetting printers (Stratasys, HP)
✓ CAD software integration
✓ Design review with full colors
✓ Fabric pattern visualization for production
```

#### **Example: Color 3D Print**
```
1. Design fabric with multicolor weave
2. Export 3D simulation as 3MF (include colors)
3. Open in slicing software
4. Colors automatically detected
5. Map to available material colors
6. Print on multi-material printer
7. Result: Physical fabric model in actual colors
```

---

## FEA/Analysis Formats

### Abaqus INP Format

#### **What is Abaqus INP?**
```
- Abaqus finite element input file
- Industry-standard FEA software
- Text-based, structured format
- Widely used in automotive, aerospace
- Supports complex material models
- Can be run directly in Abaqus/CAE or batch
```

#### **INP File Structure**
```
** Header
*Heading
Model Name

** Geometry
*Node
1, x1, y1, z1
2, x2, y2, z2
...

*Element, type=S4R
1, 1, 2, 6, 5
2, 2, 3, 7, 6
...

** Materials
*Material, name=Cotton
*Elastic
E, nu
*Density
rho

** Boundary Conditions
*Boundary
fixed_node, 1, 3

** Load Steps
*Step, name=Static
*Static
*Boundary
loaded_node, 2, 2, displacement

*Output, field, variable=PRESELECT
*Output, history, variable=PRESELECT

*End Step
```

#### **Export Settings**
```
1. File → Export → Abaqus INP
2. Configuration:
   - Solver Type: Standard/Explicit
   - Element Type: S4R (shell) or C3D8 (solid)
   - Material Model: From project settings
   - Boundary Conditions: Load applied direction
   - Element Size: Mesh density (mm)
   - Output Variables: Stress, Strain, Displacement

3. Output:
   - model.inp (complete FEA input file)
   - Ready to run directly
```

#### **Material Definition**
```
Cotton Fabric:
*Material, name=CottonFabric
*Elastic
12000.0, 0.3
*Density
0.00055

Parameters (typical):
- E (Young's modulus): 5000-15000 MPa
- nu (Poisson's ratio): 0.25-0.35
- rho (density): 0.55-0.95 g/cm³
- Custom elasticity for orthotropic material
```

#### **Use Cases**
```
✓ Academic research (fabric mechanics)
✓ Manufacturing validation (stress prediction)
✓ Material property optimization
✓ Failure analysis
✓ Comparative simulations between designs
✓ Integration with production pipeline
✓ Advanced nonlinear analysis
```

#### **Example Workflow: Abaqus Analysis**
```
1. Export from FabSim as Abaqus INP
2. Open Abaqus/CAE
3. File → Import Job... → model.inp
4. Model loaded with materials and boundary conditions
5. Configure analysis parameters (if needed)
6. Job → Submit
7. Analysis runs (seconds to hours)
8. Visualize results:
   - Stress distribution
   - Strain pattern
   - Failure zones
9. Export results for report
```

### COMSOL Format

#### **What is COMSOL?**
```
- COMSOL Multiphysics format
- Multi-physics solver (not just FEA)
- Can include thermal, fluid, electromagnetic
- Text-based mesh format
- Research-oriented
```

#### **Export Settings**
```
1. File → Export → COMSOL TXT
2. Configuration:
   - Mesh Format: Basic/Refined
   - Include Material Props: Yes/No
   - Coordinate System: Cartesian/Cylindrical
   - Output Scale: mm / cm / m

3. Output:
   - model.txt (COMSOL-formatted mesh)
```

#### **Use Cases**
```
✓ Multi-physics simulations
✓ Thermal analysis (temp-dependent properties)
✓ Coupled analysis (mechanical + thermal)
✓ Academic research
✓ Advanced material modeling
```

---

## Loom/Machine Formats

### WIF Format (Weaving Interchange Format)

#### **What is WIF?**
```
- Industry standard for loom control
- Version 1.1 (current)
- Text-based, INI-like structure
- All loom machines recognize WIF
- Preserves weave pattern, threading, treadling
- No proprietary information
```

#### **WIF File Structure**
```
[FORMAT]
Version=1.1

[CONTENT]
Title=Pattern Name
Creator=FabSim
Date=2026-07-03

[THREADING]
1=1
2=2
3=1
... (repeat for each end/warp thread)

[TREADLING]
1=1,3,5,7
2=2,4,6,8
... (repeat for each pick/weft)

[TIEUP]
1=1
2=1
...

[PALETTE]
1=0x000000
2=0xFF0000
...

[WEAVING]
UnitOfLength=cm
```

#### **Export Settings**
```
1. File → Export → WIF
2. Configuration:
   - Pattern Name: User-defined
   - Include Metadata: Yes/No
   - Color Palette: Included automatically
   - Thread Count: Auto-calculated
   - Unit of Length: cm / inches / mm

3. Output:
   - pattern.wif (single text file)
```

#### **Use Cases**
```
✓ Transfer pattern to physical loom
✓ Control Jacquard machines (with adapters)
✓ Share patterns with other weavers
✓ Archive patterns (universal format)
✓ Integration with weaving software
✓ Production pipeline input
```

#### **Example Workflow: WIF to Loom**
```
1. Design pattern in FabSim
2. Export as WIF
3. Transfer to loom control computer
4. Loom software reads WIF
5. Automatic threading calculation
6. Loom ready to weave
7. Enter yarn, start production
```

### CSV Liftplan Format

#### **What is Liftplan CSV?**
```
- Comma-separated values
- Liftplan format for loom machine control
- Simple matrix: Rows = picks, Columns = harnesses
- 1 = raise harness, 0 = lower harness
- Direct machine input for many looms
- Used in dobby looms and Jacquard machines
```

#### **Liftplan CSV Structure**
```
# Liftplan for 8-harness loom
Pick,Harness1,Harness2,Harness3,Harness4,Harness5,Harness6,Harness7,Harness8
1,1,0,1,0,0,0,0,0
2,0,1,0,1,0,0,0,0
3,1,0,1,0,0,0,0,0
4,0,1,0,1,0,0,0,0
...
```

#### **Export Settings**
```
1. File → Export → CSV Liftplan
2. Configuration:
   - Number of Harnesses: 4/8/12/16 or custom
   - Include Header Row: Yes/No
   - Number Format: 0/1 or Binary
   - Repeat Pattern: 1, 2, 4, or 8 times

3. Output:
   - pattern.csv (spreadsheet format)
   - Ready for loom software import
```

#### **Use Cases**
```
✓ Dobby loom programming
✓ Jacquard machine input
✓ Loom control software integration
✓ Pattern documentation
✓ Team sharing and versioning
```

### FABJC Binary Format (Jacquard)

#### **What is FABJC?**
```
- FabSim proprietary binary format
- Optimized for Jacquard machine compatibility
- Supports JC5, JC6, JC7 variants
- Compressed format
- Faster machine loading
- Machine-specific encodings
```

#### **Export Settings**
```
1. File → Export → FABJC
2. Configuration:
   - Machine Type: JC5 / JC6 / JC7
   - Compression: Yes/No (recommend Yes)
   - Include Color Codes: Yes/No
   - Optimization Level: Fast / Balanced / Maximum

3. Output:
   - pattern.jc5 (or .jc6 / .jc7)
   - Directly loadable into Jacquard machine
```

#### **Use Cases**
```
✓ Jacquard machine control
✓ High-speed production runs
✓ Multi-color patterns
✓ Complex designs
✓ Professional production facilities
```

---

## Vector Graphics Formats

### SVG Format (Scalable Vector Graphics)

#### **What is SVG?**
```
- XML-based vector format
- Resolution-independent (scalable)
- Web-standard (browser-native)
- Human-readable (editable in text editors)
- Supports colors, gradients, effects
- Small file size
- Animatable
```

#### **Export Options**
```
SVG Weave Draft:
- Weave pattern as vector grid
- Clickable cells (interactive)
- Color-coded threads
- Scale to any size without loss

SVG Fabric Faces:
- 3D fabric rendered as 2D SVG
- Orthographic projection
- Illustrative style
- Good for documentation
```

#### **Export Settings**
```
1. File → Export → SVG
2. Configuration:
   - Type: Weave Draft / Fabric Faces
   - Grid Size: Small (50×50) / Medium / Large (200×200)
   - Include Grid Lines: Yes/No
   - Include Labels: Yes/No
   - Inline Styles: Yes/No
   - Viewbox: Auto / Fixed dimensions

3. Output:
   - pattern.svg (single XML file)
```

#### **Use Cases**
```
✓ Web publication (GitHub README, wikis)
✓ Design documentation
✓ Interactive pattern viewers
✓ CAD software import (convert to paths)
✓ Printing (scale to any size)
✓ Animation (SMIL or CSS animations)
✓ Accessibility (text-based format)
```

#### **Example: SVG in Web**
```
1. Export pattern as SVG
2. Add to website HTML:
   <img src="pattern.svg" alt="Weave Pattern">
   or
   <object data="pattern.svg" type="image/svg+xml"></object>
3. Renders scalably at any size
4. Can add interactivity with JavaScript
5. Fully responsive design
```

### DXF Format (AutoCAD)

#### **What is DXF?**
```
- Drawing Exchange Format (AutoCAD)
- Vector format (lines, arcs, polylines)
- Industry standard for CAD
- Text-based (ASCII DXF) or binary (DXB)
- Compatible with all CAD software
- Used in manufacturing, construction, design
```

#### **Export Settings**
```
1. File → Export → DXF
2. Configuration:
   - Format: AutoCAD 2000+ (recommended)
   - Unit: mm / cm / inches / meters
   - Precision: Integer / Decimal places
   - Include Layers: Yes/No
   - Include Blocks: Yes/No

3. Output:
   - pattern.dxf (single file)
   - Ready for CAD import
```

#### **Use Cases**
```
✓ CAD design workflow integration
✓ Manufacturing CAM software
✓ CNC machine tool paths
✓ Fabric marking patterns
✓ Production templates
✓ Design review with architects
✓ Integration with 2D design tools
```

#### **Example Workflow: DXF to CAM**
```
1. Export fabric face as DXF
2. Import into CAM software (e.g., Fusion 360)
3. Create tool paths from geometry
4. Export G-code for CNC cutter
5. Cut fabric patterns physically
```

---

## Raster Image Formats

### PNG Format

#### **What is PNG?**
```
- Portable Network Graphics
- Raster (pixel-based) format
- Lossless compression
- Supports transparency (alpha channel)
- Web-standard
- Good quality for screenshots
```

#### **Export Options**
```
Current Viewport:
- Capture what you see
- At current zoom/rotation
- Resolution: 1920×1080 (adjustable)
- Format: PNG (recommended)

Six-Orthographic Views:
- Front, Back, Left, Right, Top, Bottom
- Six separate PNG files
- All at same resolution
- Isometric-like coverage
- Useful for documentation
```

#### **Export Settings**
```
1. File → Export → PNG (or Tools → Screenshot)
2. Configuration:
   - Resolution: 1920×1080 / 2560×1440 / 4K / Custom
   - Transparency: Yes/No
   - Include Grid: Yes/No
   - Include Measurement: Yes/No
   - Compression Level: 0-9 (higher = smaller, slower)

3. Output:
   - viewport.png (current view)
   - Or: front.png, back.png, left.png, etc. (6 views)
```

#### **Use Cases**
```
✓ Documentation and reports
✓ Social media and marketing
✓ Email and presentations
✓ Web publishing
✓ Before/after comparisons
✓ Portfolio and portfolio
✓ Quality control documentation
```

---

## Data Export Formats

### Analysis CSV Format

#### **What is Analysis CSV?**
```
- Comma-separated values with analysis data
- Spreadsheet-compatible (Excel, Google Sheets, Python)
- One row per metric
- Detailed numerical data
- Complete audit trail
```

#### **CSV Structure**
```
Metric,Value,Unit,Notes
ThreadDensity_Ends,15,ends/cm,Warp thread count
ThreadDensity_Picks,15,picks/cm,Weft thread count
CoveragePercentage,92.5,%,Fabric coverage
WeightEstimate,145,g/m²,Based on yarn properties
WarpThreadCount,144,threads,Total warp
WeftThreadCount,144,threads,Total weft
PatternRepeatX,12,cells,Horizontal repeat
PatternRepeatY,12,cells,Vertical repeat
MaxStress,25.3,MPa,Peak stress in 3D sim
AvgStress,8.7,MPa,Average stress
StrainAtBreak,0.15,%,Maximum strain
```

#### **Export Settings**
```
1. Tools → Export Analysis → CSV
2. Automatic export after analysis complete
3. Configuration:
   - Decimal Places: 2-6 (precision)
   - Include Metadata: Yes/No
   - Include Formulas: Yes/No
   - Timestamp: Yes/No

3. Output:
   - analysis.csv (single file)
```

#### **Use Cases**
```
✓ Data analysis in Excel/Sheets
✓ Python/R statistical analysis
✓ Documentation and reporting
✓ Comparative studies
✓ Performance tracking
✓ Quality assurance metrics
✓ Integration with external tools
```

#### **Example: Python Analysis**
```python
import pandas as pd

# Load FabSim analysis
df = pd.read_csv('analysis.csv')

# Calculate metrics
avg_density = df[df['Metric'].str.contains('Density')]['Value'].mean()

# Generate report
print(f"Average Density: {avg_density} threads/cm")

# Plot stress distribution
df[df['Metric'].str.contains('Stress')].plot()
```

---

## Import Formats

### Supported Import Formats

#### **Pattern Import**
```
WIF Format:
- File → Import → Pattern (WIF)
- Reads weave pattern, threading, colors
- Converts to FabSim internal format
- Preserves pattern data

CSV Liftplan:
- File → Import → Pattern (CSV)
- Rows = picks, Columns = harnesses
- Converts liftplan to visual weave
- May require manual color assignment

JC5/6/7 Binary:
- File → Import → Pattern (Jacquard)
- Extracts pattern data
- Reads machine-specific encoding
- Limited to available information
```

#### **Project Import**
```
FabSim Project:
- File → Open Recent (if .weave file exists)
- Or File → Import → Project
- Restores all settings, patterns, colors
- Ready to edit immediately
```

---

## Format Selection Guide

### Decision Tree

```
What do you want to do?

├─ Visualize in 3D software?
│  ├─ Professional CAD/rendering → OBJ (most compatible)
│  ├─ Blender/Cycle rendering → OBJ or 3MF
│  └─ Web viewer → OBJ or glTF (future)
│
├─ 3D Print?
│  ├─ Basic printing → STL
│  ├─ Color printing → 3MF
│  └─ Professional facility → 3MF (recommended)
│
├─ FEA Analysis?
│  ├─ Abaqus software → Abaqus INP
│  ├─ COMSOL software → COMSOL TXT
│  └─ External FEA tool → STL + load setup manual
│
├─ Weave on Loom?
│  ├─ Any loom (universal) → WIF
│  ├─ Dobby/Simple loom → CSV Liftplan
│  └─ Jacquard machine → FABJC or WIF
│
├─ CAD Integration?
│  ├─ AutoCAD → DXF
│  ├─ Fusion 360 → SVG or DXF
│  └─ Generic CAD → OBJ or DXF
│
├─ Documentation?
│  ├─ Print/PDF → PNG (high resolution)
│  ├─ Web/Online → SVG or PNG
│  └─ Reports → PNG + CSV analysis
│
└─ Data Analysis?
   ├─ Excel/Sheets → CSV Analysis
   ├─ Python/R → CSV Analysis
   └─ Academic paper → CSV + analysis plot
```

### Format Comparison Table

| Format | File Size | Quality | Editability | Industry Use | Time to Export |
|--------|-----------|---------|-------------|---|---|
| OBJ | Medium | High | Easy (text) | Universal | Fast |
| STL | Large | High | None | 3D Print | Fast |
| 3MF | Medium | Very High | Medium | Modern 3D Print | Medium |
| Abaqus | Small | High | Medium | FEA | Medium |
| COMSOL | Small | High | Medium | FEA | Medium |
| SVG | Tiny | Vector | Easy (text) | Web/Print | Fast |
| DXF | Small | Vector | Easy (CAD) | CAD/Mfg | Fast |
| WIF | Tiny | Pattern | Easy (text) | Looms | Fast |
| CSV Liftplan | Tiny | Pattern | Easy (spreadsheet) | Looms | Fast |
| FABJC | Tiny | Pattern | None (binary) | Jacquard | Fast |
| PNG | Variable | Good | None (raster) | Web/Doc | Fast |

---

## Batch Export

### Export Multiple Formats

#### **Batch Export Workflow**
```
1. File → Batch Export
2. Select desired formats (checkboxes):
   ☑ OBJ
   ☑ STL
   ☑ SVG
   ☑ DXF
   ☑ PNG
   ☑ WIF
   ☑ CSV Analysis

3. Configure each format (or use defaults)
4. Choose output folder
5. Click "Export All"
6. Progress bar shows status
7. All files generated simultaneously
```

#### **Batch Export Options**
```
Format Selection:
- Preset "3D Suite" (OBJ, STL, 3MF, PNG)
- Preset "Loom Kit" (WIF, CSV Liftplan, PNG)
- Preset "CAD Kit" (DXF, SVG, OBJ)
- Preset "Analysis Kit" (Abaqus INP, CSV, PNG)
- Custom selection

Output:
- Single folder with all formats
- Timestamped subfolders (optional)
- Manifest file listing contents
```

#### **Use Cases**
```
✓ Prepare deliverables for clients
✓ Archival (multiple formats for future)
✓ Documentation kits
✓ Production handoff (all needed formats)
✓ Collaborative projects (formats for different teams)
```

---

## Troubleshooting Export

### Common Export Issues

#### **"Export Failed" Error**
```
Possible Causes:
1. Invalid file path (special characters, spaces)
2. Insufficient disk space
3. File already open in another program
4. Permission denied on folder
5. Unsupported scale conversion

Solutions:
- Save to simpler path (C:\Exports\)
- Check available disk space (>1 GB recommended)
- Close file in other programs
- Run as Administrator (if needed)
- Try different scale (mm instead of inches)
```

#### **Missing Materials in OBJ**
```
Cause:
- Materials not embedded in model
- Texture files not found

Solution:
1. Verify .mtl file exists in same folder
2. Verify texture files in subfolder
3. Copy all files together to single folder
4. Re-import in 3D software
```

#### **Scale Issues**
```
Problem: Geometry too large or too small
Solution:
1. Identify current scale (mm, cm, meters)
2. Export with correct target scale
3. Example: 1 meter fabric → export in cm (scale ÷ 100)
4. Verify scale in target software (Tools → Preferences)

Common Scales:
- FabSim internal: millimeters
- 3D printing: millimeters
- CAD: millimeters or meters (varies)
- Loom patterns: centimeters
```

#### **Color Loss in Export**
```
Problem: Colors not showing in exported file
Causes & Solutions:
- OBJ: Verify .mtl file included and adjacent
- STL: STL format doesn't support color (use 3MF instead)
- 3MF: Include Colors checkbox must be enabled
- SVG: Use "Include Colors" option
- PNG: Screenshot naturally includes colors

Best Practice: Always export colors in OBJ (.mtl file)
```

#### **File Corruption**
```
Prevention:
- Never interrupt export (let it complete)
- Ensure sufficient disk space (3× file size)
- Disable antivirus scanning during export
- Export to local drive (not network)

If Corrupted:
- Delete incomplete file
- Try again with simpler settings
- Export smaller region if possible
- Check system logs for errors
```

---

## Format-Specific Tips

### OBJ Pro Tips
```
✓ Always include .mtl file
✓ Keep textures in same folder
✓ Use "Optimize Mesh" for web use
✓ Normal vectors necessary for lighting
✓ Scale model to actual size before export
```

### STL Pro Tips
```
✓ Use Binary format (faster, smaller)
✓ Export at actual print size
✓ Check normals orientation (View → Show Normals)
✓ Test slice in slicer before printing
✓ Keep model scale consistent
```

### 3MF Pro Tips
```
✓ Modern slicing software prefers 3MF
✓ Colors greatly improve visualization
✓ Textures embedded in single file
✓ More reliable than STL for color prints
✓ Compress option saves space
```

### WIF Pro Tips
```
✓ Universal format (use for sharing)
✓ Always include pattern name
✓ Verify threadings before loom loading
✓ Test with loom software first
✓ Color information survives export
```

### SVG Pro Tips
```
✓ Perfect for web publishing
✓ Scales perfectly (no pixelation)
✓ Use "Weave Draft" for pattern documentation
✓ Use "Fabric Faces" for visual appeal
✓ Edit SVG in Inkscape for post-processing
```

---

## Getting Help

For export-specific questions:
- See [Troubleshooting Guide](08-TROUBLESHOOTING.md)
- Check [API Reference](06-API-REFERENCE.md) for technical details
- Community forum: [forums.fabsim.example.com](https://forums.fabsim.example.com)
- Email: support@fabsim.example.com
