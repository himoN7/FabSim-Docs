# Complete Features Reference Guide

**Version:** 0.4.0
**Last Updated:** July 2026
**Audience:** Users, Support, Trainers

---

## Table of Contents

1. [2D Weave Design](#2d-weave-design)
2. [Weave Patterns Library](#weave-patterns-library)
3. [2D Simulation & Visualization](#2d-simulation--visualization)
4. [3D Simulation & Physics](#3d-simulation--physics)
5. [Model Builder (AI-Assisted)](#model-builder-ai-assisted)
6. [Project Management](#project-management)
7. [Import & Export](#import--export)
8. [Image Palette Extraction](#image-palette-extraction)
9. [Analysis & Metrics](#analysis--metrics)
10. [Advanced Settings](#advanced-settings)

---

## 2D Weave Design

### Canvas Controls

#### **Navigation**


| Action            | Method                  | Purpose                         |
| ------------------- | ------------------------- | --------------------------------- |
| **Pan/Move**      | Middle mouse + drag     | Move around the infinite canvas |
| **Zoom In**       | Scroll wheel up         | Get closer view                 |
| **Zoom Out**      | Scroll wheel down       | Get wider view                  |
| **Reset View**    | Double-click background | Return to default zoom/position |
| **Fit to Window** | Ctrl+0                  | Auto-zoom to fit entire pattern |

#### **Editing**


| Action            | Method              | Purpose                             |
| ------------------- | --------------------- | ------------------------------------- |
| **Toggle Cell**   | Left-click cell     | Switch warp/weft intersection state |
| **Select Cell**   | Shift+Click         | Select single cell for properties   |
| **Select Region** | Click+Drag          | Select multiple cells               |
| **Fill Region**   | Right-click → Fill | Fill selected area with color       |
| **Clear Region**  | Delete or Backspace | Clear selected cells                |

### Canvas Properties Configuration

#### **Grid Setup**

```
Ends: Number of warp threads
- Range: 1-1000+
- Typical: 8-200 (affects grid complexity)

Picks: Number of weft threads
- Range: 1-1000+
- Typical: 8-200 (affects grid complexity)

Pattern Repeat: Base unit for pattern tiling
- Usually matches Ends×Picks or subset
- Controls tile visualization
```

#### **Thread Properties**

```
Thread Count (Ends/cm): Density of warp threads
- Range: 1-100+ per cm
- Typical: 10-40 per cm (coarse to fine)
- Affects simulated fiber thickness

Thread Count (Picks/cm): Density of weft threads
- Range: 1-100+ per cm
- Typical: 10-40 per cm
- Affects simulated fiber thickness

Yarn Denier: Thread thickness
- Range: 10D - 10000D (Denier units)
- Typical: 75D-400D (fine to thick)
- Affects simulation density calculation

Fiber Roughness: Surface texture
- Range: 0.0 (smooth) - 1.0 (rough)
- Default: 0.5
- Affects friction in physics simulation
```

### Color Management

#### **Color Properties**

```
Warp Color: Primary warp thread color
- Adjustable via color picker
- Affects all warp threads equally

Weft Color: Primary weft thread color
- Adjustable via color picker
- Affects all weft threads equally

Color Repeat X/Y: Control pattern color tiling
- Allows multi-color repeating patterns
- Create complex color arrangements
```

#### **Color Palette Operations**

```
Save Palette: Store current colors as preset
- Saved in app settings
- Quick access via palette library
- Share with team

Load Palette: Apply saved color scheme
- Select from library
- Instantly update canvas

Extract from Image: Automatic palette generation
- Import image (JPG, PNG, BMP)
- Auto-analyze colors
- Create palette with 2-16 dominant colors
- See "Image Palette Extraction" section
```

### Design Operations

#### **Pattern Save/Load**

```
Save as Custom Pattern:
1. Design weave on canvas
2. File → Save Pattern As...
3. Name and categorize
4. Stored in: %AppData%\FabSim\Patterns\Custom\

Load Pattern:
1. File → Load Pattern
2. Select from built-in or custom
3. Pattern replaces current canvas
4. Preserves color settings if applicable

Export Pattern:
1. File → Export Pattern
2. Choose format (JSON recommended)
3. Share via email or repository
```

#### **Pattern Operations**

```
Clear Canvas: Ctrl+N
- Delete all weave data
- Start fresh design

Copy Pattern: Ctrl+C
- Copy pattern to clipboard
- Paste into new project

Mirror Horizontal: Transform → Flip H
- Mirror pattern left-right
- Useful for symmetric designs

Mirror Vertical: Transform → Flip V
- Mirror pattern top-bottom
- Creates symmetry

Rotate 90°: Transform → Rotate 90
- Rotate pattern clockwise
- Can convert warp-heavy to weft-heavy

Invert Pattern: Transform → Invert
- Swap all cells (on↔off)
- Creates inverse weave structure
```

---

## Weave Patterns Library

### Built-in Patterns (27 Total)

#### **Plain Weaves (3)**

1. **Plain 1/1** — Basic over-under pattern

   - Simplest weave
   - Maximum interlacing
   - Used as baseline for blends
2. **Basket 2/2** — Two over, two under

   - More open than 1/1
   - Flexible fabric
   - Lighter appearance
3. **Panama 3/3** — Three over, three under

   - Very open structure
   - Straw-like texture
   - Breathable fabric

#### **Twill Weaves (9)**

1. **Twill 2/2** — Standard twill

   - Diagonal ribs
   - Durable and stable
   - Classic fabric texture
2. **Twill 3/1** — 3 warp over, 1 weft over

   - Steeper diagonal
   - Warp-faced appearance
   - Strong warp emphasis
3. **Point Twill 2/2** — Symmetric twill

   - Diamond-like repeating pattern
   - Balanced appearance
   - Reversible structure
4. **Curved Twill 2/2** — Wavy diagonal

   - Serpentine pattern
   - Flowing visual effect
   - Decorative applications
5. **Broken Twill 2/2** — Interrupted diagonal

   - Step-pattern appearance
   - Visual interest
   - Casual texture
6. **Herringbone 2/2** — V-shaped pattern

   - Mirror diagonal shifts
   - Distinctive zigzag
   - Highly decorative
7. **Diamond 2/2** — Diamond repetition

   - Lozenge-shaped tiles
   - Formal appearance
   - Geometric design
8. **Sequence Twill** — Programmable twill

   - Custom progression
   - Variable diagonal angle
   - Experimental structures

#### **Satin Weaves (4)**

1. **Satin 5-end** — 5-thread satin

   - Smooth shiny surface
   - Minimal interlacing
   - Luxurious appearance
2. **Satin 8-end** — 8-thread satin

   - Extended float
   - Maximum smoothness
   - Premium look
3. **Sateen 5-end** — Weft-faced satin

   - Lustrous weft surface
   - Luxurious feel
   - Smooth weave variant
4. **Sateen 8-end** — Extended weft satin

   - Very smooth weft surface
   - Premium appearance
   - Fine fabric texture

#### **Rib Weaves (2)**

1. **Warp Rib 2×2** — Vertical ribs

   - Warp-emphasized texture
   - Pronounced ridges
   - Structural appearance
2. **Weft Rib 2×2** — Horizontal ribs

   - Weft-emphasized texture
   - Horizontal ridges
   - Linear appearance

#### **Specialty Weaves (8)**

1. **Honeycomb** — Dimensional texture

   - Three-dimensional effect
   - Cellular pattern
   - Technical application
2. **Waffle 8×8** — Box-like pattern

   - 8×8 repeat unit
   - Quilted appearance
   - Textured surface
3. **Huckaback 18×18** — Large pile weave

   - Towel-like structure
   - Absorbent properties
   - Practical application
4. **Granité Crepe** — Grainy texture

   - Random appearance
   - Matte surface
   - Casual look
5. **M's and O's** — Letter-pattern weave

   - M and O shapes
   - Decorative design
   - Artistic pattern
6. **Mock Leno** — Open weave effect

   - Appears as leno without actual leno
   - Lightweight appearance
   - Cost-effective alternative

### Pattern Categories

```
By Complexity:
- Simple: Plain, Basket, Twill basic
- Intermediate: Most twills, ribs
- Advanced: Satin, specialty weaves

By Characteristics:
- Tight: Plain weaves (high interlacing)
- Medium: Most twills and ribs
- Open: Satin, Panama, Mock leno

By Visual Effect:
- Smooth/Shiny: Satin, Sateen
- Textured: Honeycomb, Waffle, Granité
- Geometric: Diamond, Herringbone
- Linear: Ribs, basic twill

By Application:
- General Purpose: Plain 1/1, Twill 2/2
- Luxury: Satin 8-end, Sateen 8-end
- Technical: Honeycomb, Huckaback
- Decorative: Diamond, Herringbone, M's and O's
```

---

## 2D Simulation & Visualization

### Real-Time Yarn Visualization

#### **Yarn Oval Display**

```
What You See:
- Interlaced warp threads shown as ovals
- Weft threads shown as crossing ovals
- Ovals deform at intersection points
- Color indicates warp vs weft

Parameters Affecting Display:
- Yarn Denier: Larger denier = thicker ovals
- Fiber Roughness: Higher roughness = coarser edges
- Thread Count: Higher count = denser ovals
- Color settings: Warp/weft colors applied
```

#### **Live Parameter Adjustment**

```
Change Any Setting:
1. Modify thread count, denier, or roughness
2. Canvas updates in real-time
3. Yarn ovals recalculate instantly
4. See results immediately without export

Performance:
- Smooth updates up to 100×100 grid
- Slightly slower for 200×200+ grids
- Configurable refresh rate (Settings)
```

### Density Calculations

#### **Metrics Displayed**

```
Ends/cm: Actual warp thread density
- Calculated from grid dimensions
- Shows weave efficiency
- Range: 1-100+ per cm

Picks/cm: Actual weft thread density
- Calculated from grid dimensions
- Shows weave efficiency
- Range: 1-100+ per cm

Coverage: Estimated fabric coverage %
- 100% = completely filled
- <100% = open weave
- Shows mesh/transparency

Weight Estimate: Projected fabric weight
- Based on yarn properties
- Helps predict material usage
- In grams per square meter
```

#### **Density Analysis**

```
View Detailed Analysis:
1. Tools → Fabric Analysis
2. See density heatmap
3. Identify weak points
4. Adjust parameters for optimization

Export Analysis:
1. Tools → Export Analysis → CSV
2. Get detailed metrics in spreadsheet
3. Share with production team
```

---

## 3D Simulation & Physics

### Physics Simulation Types

#### **Yarn XPBD (Default)**

```
Method: Extensible Position-Based Dynamics
Pros:
- Fast simulation (real-time at moderate scale)
- Stable convergence
- Handles large deformations well
- Suitable for design visualization

Cons:
- Less physically accurate than FEA
- May not capture all stress concentrations
- Simplified constraint handling

Best For:
- Quick design validation
- Visual feedback during design
- General deformation preview
```

#### **Shell Drape (Advanced - FEA)**

```
Method: Finite Element Analysis with shell elements
Pros:
- More physically accurate
- Captures stress distribution
- Suitable for technical analysis
- Produces research-quality results

Cons:
- Slower simulation (~10-100× slower)
- Requires calibration
- More computationally intensive

Best For:
- Detailed stress analysis
- Academic research
- Manufacturing validation
- Performance prediction

Note: FEA module available in FabSim.Physics.Fea
```

### Simulation Parameters

#### **Global Settings**

```
Gravity: Fabric weight effect
- 0: No gravity (floating)
- 9.8 m/s²: Earth gravity (typical)
- Adjustable for scenarios

Air Damping: Air resistance
- 0: No air resistance
- 0.1-0.5: Typical (dampens oscillation)
- 1.0: Maximum (static fabric)

Wind Force: Applied wind load
- 0: No wind
- 0.1-10: Wind direction and magnitude
- Useful for draping simulation

Friction Coefficient: Fabric-to-fabric friction
- 0: Frictionless (slides freely)
- 0.3-0.8: Typical textile friction
- 1.0+: Sticky interaction
```

#### **Convergence Settings**

```
Iterations per Frame: Physics solver iterations
- Range: 1-20
- Higher = more accurate, slower
- Typical: 5-10

Time Step: Simulation time advancement
- Smaller = more stable, slower
- Larger = faster, less stable
- Typical: 0.016 seconds (60 FPS)

Tolerance: Constraint convergence threshold
- Tighter = more accurate, slower
- Looser = faster, less precise
- Default: 0.001 (1 mm at 1 meter scale)
```

### 3D Viewport Controls

#### **Navigation**

```
Rotate: Right-click + Drag
- Rotate view around fabric
- Horizontal drag: Rotate around Z
- Vertical drag: Rotate around X/Y

Pan: Middle-click + Drag (or Ctrl+Right-click)
- Move view without rotation
- Useful for positioning

Zoom: Scroll wheel
- Zoom in/out on cursor
- Precise control

Reset View: Double-click
- Return to default camera
- Useful after complex rotations

Orthographic Views: View menu
- Front, Back, Left, Right, Top, Bottom
- Fixed viewpoints for analysis
```

#### **Visualization Options**

```
Wireframe Mode: View → Wireframe
- Show mesh structure
- See all geometry elements
- Useful for debugging

Surface Mode: View → Shaded
- Show actual fabric surface
- Apply materials and colors
- Production-quality view

Stress Heatmap: View → Heat Map
- Color-code by stress level
- Red = high stress, Blue = low
- Helps identify failure zones

Particle Points: View → Particles
- Show particle positions
- See yarn path nodes
- Debug geometry

Constraint Visualization: View → Constraints
- Show distance constraints
- Show bend constraints
- Understand binding
```

### Material System

#### **Built-in Materials**

```
Cotton: Natural fiber appearance
- Matte surface
- Warm color tone
- Realistic texture

Silk: Shiny, luxurious
- High specular reflection
- Smooth surface
- Premium appearance

Polyester: Synthetic glossy
- Mirror-like shine
- Cool tone
- Technical appearance

Wool: Rough, organic
- Matte surface
- Textured detail
- Natural look

Linen: Structured, crisp
- Medium sheen
- Irregular texture
- Professional appearance

Custom: Define your own
- Adjustable specularity
- Custom color/roughness
- Create unique materials
```

#### **Material Properties**

```
Diffuse Color: Base fabric color
- Applies to entire surface

Roughness: Surface texture detail
- 0.0: Smooth, mirror-like
- 0.5: Medium, typical textile
- 1.0: Very rough, matte

Specularity: Shine/reflection intensity
- 0.0: No shine (matte)
- 0.5: Medium shine
- 1.0: Very shiny (glossy)

Metallic: Metallic surface effect
- 0.0: Non-metallic (typical)
- 0.5: Partially metallic
- 1.0: Full metallic appearance

Normal Map: Surface detail texture
- Imported from texture file
- Adds micro-detail without geometry
- Common in PBR materials
```

---

## Model Builder (AI-Assisted)

### Overview

The Model Builder allows you to create custom yarn path geometry models using:

- **Visual Graph Interface** — Equation builder GUI
- **AI Assistance** (Optional) — Get suggested equations from AI
- **Offline Fallback** — Built-in suggestions if no AI available
- **Package Publishing** — Save model for reuse

### Creating a Custom Model

#### **Step 1: Start New Model**

```
1. Go to Model Builder page
2. Click "New Model"
3. Choose template (or blank)
4. Name your model
5. Set target thread count
```

#### **Step 2: Define Yarn Path Equations**

```
Model consists of parametric equations:
- X(u) = f(u) — Horizontal position
- Y(u) = g(u) — Vertical position  
- Z(u) = h(u) — Vertical elevation

Where:
- u = parameter (0 to 1, normalized)
- f, g, h = mathematical functions
```

#### **Step 3: Use Equation Editor**

```
GUI Interface:
- Node-based graph builder
- Drag nodes for operations
- Connect nodes for data flow
- Visual formula construction

Supported Functions:
- Trigonometric: sin(u), cos(u), tan(u)
- Polynomial: u, u², u³, etc.
- Hyperbolic: sinh, cosh, tanh
- Exponential: exp(u), ln(u)
- Custom: Define variables, constants

Variables:
- u: Parameter (0-1)
- phase: Add phase shift
- amplitude: Control magnitude
- frequency: Control oscillation rate
```

#### **Step 4: Request AI Assistance**

```
AI Features (Optional):
1. Describe desired yarn shape: "wavy," "spiral," "zigzag"
2. Click "AI Suggest Equation"
3. System generates equation options
4. Select best fit
5. Refine if needed

AI Providers:
- Offline: Built-in heuristic (always available)
- OpenAI: GPT-4 for advanced suggestions
- Ollama: Local LLM for privacy
- Hugging Face: Open-source models

Configuration: Settings → AI Provider
```

#### **Step 5: Validate & Compile**

```
Validation:
1. Click "Validate"
2. Check for syntax errors
3. Verify mathematical properties
4. Ensure reachable parameters

Compilation:
1. Click "Compile"
2. Convert equations to executable code
3. Optimize for simulation
4. Generate simulation data

Testing:
1. Preview in 3D viewport
2. Adjust parameters if needed
3. Check for anomalies
```

#### **Step 6: Publish Model Package**

```
Publishing:
1. Click "Publish"
2. Name package (e.g., "MyWavePattern")
3. Add description
4. Set version (e.g., 1.0.0)
5. Tag categories

Storage:
- Saved to: %AppData%\FabSim\Models\Published\
- Can be shared as `.zip` file
- Importable by other users

Usage:
- Available in "Geometry Models" dropdown
- Select in 3D simulation
- Generates custom yarn path
```

### Example: Creating a Spiral Yarn Model

```
Step 1: Set Model Name
Name: "Spiral Yarn"
Description: "Spiral interlace pattern"

Step 2: Define Equations
X(u) = amplitude * cos(frequency * u)
Y(u) = amplitude * sin(frequency * u)
Z(u) = u * height

Parameters:
- amplitude: 5 mm (control spiral width)
- frequency: 10 (control spiral tightness)
- height: 10 mm (yarn length)

Step 3: Ask AI
Prompt: "Create a tight spiral pattern"
AI Response: [Generates optimized parameters]

Step 4: Compile and Preview
Result: Visible spiral in 3D viewport

Step 5: Publish
Name: "Spiral_v1.0"
Ready to use in any simulation
```

### AI Assistance Options

#### **Offline Mode (Always Available)**

```
Heuristic Suggestions:
- Based on parameter descriptions
- No internet required
- Instant response
- Good for common patterns

Limitations:
- Less creative than LLM
- Limited to pre-defined templates
- May not handle complex requests
```

#### **Online Mode (OpenAI)**

```
Requirements:
- API key in Settings
- Internet connection
- OpenAI account with credits

Advantages:
- Advanced equation suggestions
- Creative problem-solving
- Context-aware responses
- More accurate models

Setup:
1. Settings → AI Provider → OpenAI
2. Enter API key
3. Click "Test Connection"
```

#### **Local Mode (Ollama)**

```
Requirements:
- Ollama installed locally
- Model downloaded (e.g., "llama2")
- Running on localhost

Advantages:
- Privacy preserved
- No API costs
- Fast local processing
- Offline capable

Setup:
1. Install Ollama from ollama.ai
2. Download model: ollama pull llama2
3. Settings → AI Provider → Ollama
4. Enter localhost:11434
```

---

## Project Management

### Creating Projects

#### **New Project**

```
1. File → New Project (Ctrl+N)
2. Choose starting configuration:
   - Blank: Empty canvas
   - From Template: Pre-configured
   - From Pattern: Load existing pattern
3. Name project
4. Choose default parameters
5. Start design
```

#### **Project Metadata**

```
Auto-Saved Fields:
- Project Name
- Creation Date
- Last Modified Date
- Thumbnail image
- Associated patterns
- Simulation parameters
- Color schemes
- Export history
```

### Saving & Loading

#### **Auto-Save**

```
When Active: On by default
- Saves every 5 minutes (configurable)
- Saves on page navigation
- Saves before export
- No manual action required

Location: %AppData%\FabSim\Projects\AutoSave\

Disable: Settings → Auto-Save (toggle off)
```

#### **Manual Save**

```
Save Current: Ctrl+S
- Overwrite existing file
- Update modification time

Save As: File → Save As
- Create new named file
- Keep original
- Useful for versioning

Quick Save Locations:
- Recent Projects (File menu)
- Recent list (main page)
- %AppData%\FabSim\Projects\
```

### Project Recovery

#### **Crash Recovery**

```
If App Crashes:
1. Restart application
2. Recovery dialog appears
3. Select "Restore Recent Project"
4. Choose from recovered files
5. Project restores to last state

Recovery Options:
- Restore Latest: Most recent auto-save
- Choose Specific: Browse recovery files
- Discard: Start fresh project
```

#### **Backup Management**

```
Enable Backups: Settings → Backup
- Automatic backups on save
- Configurable backup count (3-20)
- Stored in: %AppData%\FabSim\Backups\

Manual Backup:
1. File → Create Backup
2. Backup created with timestamp
3. Manual backups preserved separately

Restore from Backup:
1. File → Restore from Backup
2. Browse backup history
3. Select desired date/time
4. Restore project state
```

### Project Sharing

#### **Export Project**

```
1. File → Export Project
2. Choose location
3. File format: .weave (JSON-based)
4. Include settings and thumbnails
5. Shareable via email or repository
```

#### **Import Project**

```
1. File → Import Project
2. Browse to .weave file
3. Project loads with all data
4. Creates local copy
5. Ready to edit and save
```

---

## Import & Export

See [Complete Export Formats Guide](14-EXPORT-FORMATS.md) for detailed information on all import/export options.

### Quick Reference

#### **Import Formats**

```
Patterns:
- WIF (Weaving Interchange Format) 1.1
- JC5/6/7 (Jacquard machine formats)
- CSV (Liftplan data)
- Custom JSON patterns

Projects:
- .weave (FabSim native)
- Previously saved projects
```

#### **Export Formats**

```
3D Mesh:
- OBJ (with materials)
- STL (binary mesh)
- 3MF (3D Manufacturing Format)

FEA/Analysis:
- Abaqus INP
- COMSOL TXT
- CSV Analysis

Graphics/CAD:
- SVG (vector)
- DXF (AutoCAD)
- PNG (raster image)

Loom/Machine:
- WIF (loom format)
- CSV (liftplan)
- FABJC (Jacquard binary)
```

---

## Image Palette Extraction

### Feature Overview

Automatically analyze images and extract dominant colors to create weave palettes.

### Using Image Palette Extraction

#### **Step 1: Open Image**

```
1. Weave Design page
2. Tools → Extract Palette from Image
3. Select image file (JPG, PNG, BMP)
4. Image loads in preview
```

#### **Step 2: Configure Extraction**

```
Number of Colors: 2-16
- Fewer colors: More contrast
- More colors: More variety
- Typical: 4-8 colors

Color Dominance: 
- High: Only most prominent colors
- Medium: Balanced selection
- Low: Include all variations
```

#### **Step 3: Review & Apply**

```
Preview Shows:
- Extracted colors in swatches
- Color frequency bar charts
- Applied to canvas preview

Accept and Apply:
1. Click "Apply Palette"
2. Canvas updates with new colors
3. Palette saved in project

Refine:
1. Adjust colors manually
2. Remove unwanted colors
3. Reorder color sequence
```

### Example Workflows

#### **Create Palette from Photo**

```
1. Photo of fabric in natural light
2. Extract Palette from Image
3. 8 colors extracted
4. Apply to canvas
5. Weave now matches photo colors

Use Case: Color reproduction, photo reference
```

#### **Create Palette from Painting**

```
1. Artistic painting image
2. Extract Palette from Image
3. 6 colors extracted (avoid extremes)
4. Apply to gradient weave pattern
5. Create artistic fabric simulation

Use Case: Creative design inspiration
```

#### **Palette from Mood Board**

```
1. Composite mood board image
2. Extract Palette from Image
3. 12 colors extracted (high diversity)
4. Pick favorite subset
5. Create multi-color weave

Use Case: Cohesive design from reference
```

---

## Analysis & Metrics

### Available Analysis Tools

#### **Fabric Analysis Report**

```
Access: Tools → Fabric Analysis

Metrics Included:
- Thread density (ends/cm, picks/cm)
- Coverage percentage
- Weight estimate (g/m²)
- Weave efficiency
- Balance ratio (warp vs weft)
- Opening size average
- Interlacing count
```

#### **Simulation Stress Analysis**

```
After 3D Simulation:

Stress Distribution:
- Peak stress location
- Average stress across fabric
- Stress concentration factor
- Failure prediction

Strain Analysis:
- Maximum strain
- Strain distribution
- Elastic vs plastic region
- Material behavior

Deformation Map:
- Displacement heatmap
- Movement magnitude
- Directional deformation
```

#### **Export to Analysis Format**

```
CSV Export:
1. Tools → Export Analysis → CSV
2. Detailed spreadsheet format
3. Includes all metrics
4. Compatible with Excel, Python

Uses:
- Data analysis
- Optimization studies
- Production planning
- Quality control
```

---

## Advanced Settings

See [Settings & Configuration Reference](15-SETTINGS-REFERENCE.md) for complete settings documentation.

### Quick Reference

#### **Simulation Defaults**

```
Settings → Physics Defaults

- Gravity: 9.8 m/s²
- Air damping: 0.1
- Iterations: 8
- Solver type: XPBD (Yarn)
- Friction: 0.5
```

#### **UI Preferences**

```
Settings → UI Preferences

- Theme: Light/Dark/Auto
- Backdrop: Varied visual effects
- Font size: Adjustable
- Window size: Remember or fullscreen
- Viewport quality: Performance vs quality
```

#### **Export Defaults**

```
Settings → Export Defaults

- Default format: OBJ
- Image resolution: 1920×1080
- Mesh quality: High/Medium/Low
- Include materials: On/Off
- Scale: Millimeters (adjustable)
```

#### **AI Provider**

```
Settings → AI Provider

- Provider: Offline/OpenAI/Ollama
- API key (if applicable)
- Temperature: Creativity level (0.5-1.5)
- Model: Specific LLM variant
```

---

## Performance & Optimization

### Performance Targets

```
Cold Start (First Launch): <3 seconds
Warm Start (Subsequent): <1 second
First Paint (UI visible): <500 ms

Interaction Responsiveness:
- Click to response: <100 ms
- Scroll responsiveness: 60 FPS minimum
- Drag smoothness: 60 FPS

Simulation Performance:
- 2D Simulation: Real-time (60+ FPS)
- 3D @ 1,000 particles: 60 FPS
- 3D @ 5,000 particles: 30 FPS
- 3D @ 50,000 particles: 5-10 FPS
```

### Optimization Tips

#### **For Design Phase**

```
- Use smaller grids (50×50 instead of 200×200)
- Reduce simulation iterations (5-6 instead of 10)
- Disable real-time preview during editing
- Use 2D simulation instead of 3D
```

#### **For Analysis Phase**

```
- Export to external solver (Abaqus, COMSOL)
- Use Shell FEA for accuracy
- Reduce particle count with representative scale
- Run on GPU-enabled system
```

#### **For Rendering**

```
- Export to 3D modeling software for final render
- Use built-in PNG export for quick previews
- High-quality orthographic exports for documentation
- Batch export for multi-format outputs
```

---

## Accessibility & Keyboard Shortcuts

See [Keyboard Shortcuts & Accessibility Guide](16-KEYBOARD-SHORTCUTS.md) for complete keyboard reference.

### Essential Shortcuts

```
File Operations:
- Ctrl+N: New project
- Ctrl+O: Open project
- Ctrl+S: Save project
- Ctrl+Shift+S: Save As

Editing:
- Ctrl+Z: Undo
- Ctrl+Y: Redo
- Ctrl+C: Copy pattern
- Ctrl+V: Paste pattern
- Ctrl+A: Select all

Navigation:
- Ctrl+0: Fit to window
- Home: Default view
- End: Focus on center

Simulation:
- Space: Start/Stop simulation
- R: Reset simulation
- P: Pause simulation
- +/-: Increase/decrease speed
```

---

## Tips & Best Practices

### Design Tips

```
✓ Start with built-in patterns as templates
✓ Adjust parameters gradually to see effects
✓ Use color to distinguish warp from weft
✓ Test different thread densities
✓ Save work frequently (Auto-save helps)
✓ Use 2D sim for quick feedback
✓ Export intermediate designs
```

### Simulation Tips

```
✓ Validate with 2D before 3D
✓ Start with smaller grids (scales faster)
✓ Use appropriate yarn model for accuracy
✓ Adjust physics parameters per fabric type
✓ Export stress analysis for validation
✓ Compare multiple models for optimization
```

### Export Tips

```
✓ Use OBJ for 3D visualization
✓ Use STL for 3D printing prep
✓ Use SVG for CAD workflow
✓ Use WIF for loom machine import
✓ Export at appropriate scale (mm, cm, m)
✓ Include materials in mesh format
✓ Batch export for documentation
```

---

## Common Workflows

### Workflow 1: Photo-Inspired Weave Design

```
1. File → New Project
2. Tools → Extract Palette from Image
   - Select fabric photo
   - Extract 6 colors
3. Apply colors to canvas
4. Design weave pattern with extracted colors
5. Tools → Fabric Analysis
   - Verify coverage and balance
6. File → Export → OBJ
   - Visualize in 3D
7. File → Save Project
```

### Workflow 2: Technical FEA Analysis

```
1. File → New Project (from template)
2. Configure fabric parameters precisely
3. Select Shell Drape solver
4. Set realistic boundary conditions
5. Run simulation
6. Tools → Export Analysis → CSV
7. Analyze stress distribution
8. File → Export → Abaqus INP
   - Send to production team
```

### Workflow 3: Custom Yarn Model Creation

```
1. Model Builder page
2. Click "New Model"
3. Define equations (or AI Suggest)
4. Validate and Compile
5. Publish as package
6. Return to Weave Design
7. Use new model in 3D simulation
8. Export final result
9. File → Save Project (includes model reference)
```

---

## Getting Help

**For More Details:**

- [Complete User Guide](01-USER-GUIDE.md) — Feature details
- [Troubleshooting Guide](08-TROUBLESHOOTING.md) — Problem solving
- [Keyboard Shortcuts](16-KEYBOARD-SHORTCUTS.md) — Shortcut reference
- [Export Formats Guide](14-EXPORT-FORMATS.md) — Export details
- [Settings Reference](15-SETTINGS-REFERENCE.md) — Configuration options

**For Support:**

- Email: support@fabsim.example.com
- GitHub Issues: [repository](https://github.com/fabsim)
- Community Forum: [forums.fabsim.example.com](https://forums.fabsim.example.com)
