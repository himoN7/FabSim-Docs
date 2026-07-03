# Data Models & Formats

Complete reference for project file structures, data serialization, and model specifications.

## Table of Contents

1. [Project File Format](#project-file-format)
2. [Fabric Simulation Spec](#fabric-simulation-spec)
3. [Weave Pattern Data](#weave-pattern-data)
4. [Yarn Specifications](#yarn-specifications)
5. [Simulation Results](#simulation-results)
6. [Import/Export Formats](#importexport-formats)
7. [Model Builder Packages](#model-builder-packages)

## Project File Format

### WinUI FabSim Project (.weave)

**File Type:** JSON-based with `.weave` extension

**Storage Location:**
```
%AppData%\WinUi FabSim\Projects\
├── proj-123.weave
├── proj-456.weave
└── ...

%AppData%\WinUi FabSim\Recovery\    (Auto-saved backups)
├── proj-123_recovery.weave
└── ...
```

### Project File Structure

```json
{
  "projectId": "proj-2024-001",
  "projectType": "FabricSimulation",
  "version": "0.4.0",
  "metadata": {
    "name": "My Weave Design",
    "description": "Custom plain weave with cotton yarns",
    "created": "2024-01-15T10:30:00Z",
    "lastModified": "2024-01-15T14:45:00Z",
    "author": "Your Name",
    "tags": ["cotton", "plain-weave", "prototype"],
    "thumbnail": "base64-encoded-png-image"
  },
  "fabricSimulationSpec": {
    "weavePattern": { /* ... */ },
    "geometryParams": { /* ... */ },
    "yarns": [ /* ... */ ],
    "materials": [ /* ... */ ],
    "simulationOptions": { /* ... */ }
  },
  "colorPalette": {
    "warpColors": ["#FF0000", "#00FF00", "#0000FF"],
    "weftColors": ["#FFFF00", "#FF00FF", "#00FFFF"]
  },
  "uiState": {
    "currentViewMode": "2D",
    "cameraPosition": { "x": 0, "y": 0, "z": 10 },
    "zoomLevel": 1.0
  }
}
```

---

## Fabric Simulation Spec

Complete specification of fabric for simulation.

### Schema

```json
{
  "fabricSimulationSpec": {
    "weavePattern": {
      "type": "object",
      "description": "2D weave interlacing pattern"
    },
    "geometryParams": {
      "type": "object",
      "description": "3D geometry configuration"
    },
    "yarns": {
      "type": "array",
      "description": "Array of yarn specifications",
      "items": { "type": "object" }
    },
    "materials": {
      "type": "array",
      "description": "Fiber material profiles",
      "items": { "type": "object" }
    },
    "simulationOptions": {
      "type": "object",
      "description": "Solver and algorithm options"
    }
  }
}
```

### Full Example

```json
{
  "weavePattern": {
    "name": "Plain Weave",
    "width": 8,
    "height": 8,
    "patternRepeatUnit": 2,
    "gridData": [
      [1, 0, 1, 0, 1, 0, 1, 0],
      [0, 1, 0, 1, 0, 1, 0, 1],
      [1, 0, 1, 0, 1, 0, 1, 0],
      [0, 1, 0, 1, 0, 1, 0, 1],
      [1, 0, 1, 0, 1, 0, 1, 0],
      [0, 1, 0, 1, 0, 1, 0, 1],
      [1, 0, 1, 0, 1, 0, 1, 0],
      [0, 1, 0, 1, 0, 1, 0, 1]
    ]
  },
  "geometryParams": {
    "endsPerCm": 20.0,
    "picksPerCm": 18.0,
    "yarnPathModel": "Peirce",
    "crimpFactor": 0.15,
    "warpYarnThickness": 0.5,
    "weftYarnThickness": 0.48,
    "fabricThickness": 0.8
  },
  "yarns": [
    {
      "id": "warp",
      "name": "Warp Yarn",
      "denier": 150,
      "linearDensity": 0.85,
      "crossSection": "Round",
      "fiberRoughness": 0.15,
      "color": { "r": 255, "g": 0, "b": 0, "a": 255 }
    },
    {
      "id": "weft",
      "name": "Weft Yarn",
      "denier": 145,
      "linearDensity": 0.82,
      "crossSection": "Round",
      "fiberRoughness": 0.12,
      "color": { "r": 0, "g": 0, "b": 255, "a": 255 }
    }
  ],
  "materials": [
    {
      "id": "cotton",
      "name": "100% Cotton",
      "fiberType": "Cellulose",
      "density": 1.54,
      "elasticity": 0.05,
      "dampingFactor": 0.1
    }
  ],
  "simulationOptions": {
    "solverType": "ParticleSystem",
    "timeStep": 0.001,
    "convergenceTolerance": 1e-6,
    "maxIterations": 1000,
    "useGpuAcceleration": true,
    "threadCount": 4
  }
}
```

---

## Weave Pattern Data

### Format

```csharp
public class WeavePatternData
{
    public string? Name { get; set; }
    public int Width { get; set; }      // Number of warp ends
    public int Height { get; set; }     // Number of weft picks
    public int PatternRepeatUnit { get; set; }
    public int[][]? GridData { get; set; }  // 1 = warp over, 0 = weft over
    public string? Category { get; set; }
    public Dictionary<string, string>? Metadata { get; set; }
}
```

### Grid Data Explanation

Grid is 2D array where:
- `1` = Warp yarn passes over weft yarn
- `0` = Weft yarn passes over warp yarn

**Plain Weave Example (2x2):**
```
[
  [1, 0],
  [0, 1]
]

Visual (top-down):
  Warp
  ↓
  W w    (W = over, w = under)
  w W
  ↑
  Weft
```

### Built-in Pattern Categories

Available patterns:
- `"PlainWeave"`
- `"TwillWeave"`
- `"SatinWeave"`
- `"RibWeave"`
- `"HoneycombWeave"`
- `"WaffleWeave"`
- `"HuckabackWeave"`
- `"VelvetWeave"`
- `"DamaskWeave"`

---

## Yarn Specifications

### Complete Yarn Model

```json
{
  "yarnSpecification": {
    "id": "yarn-001",
    "name": "Primary Yarn",
    "yarnSystem": "Warp",
    "denier": 150,
    "linearDensity": 0.85,
    "crossSection": "Oval",
    "crossSectionDimensions": {
      "majorAxis": 0.8,
      "minorAxis": 0.6
    },
    "fiberRoughness": 0.15,
    "color": {
      "r": 255,
      "g": 0,
      "b": 0,
      "a": 255
    },
    "material": {
      "fiberType": "Polyester",
      "density": 1.38,
      "elasticity": 0.08,
      "dampingFactor": 0.12
    },
    "properties": {
      "tensileStrength": 450.0,
      "elongationAtBreak": 25.0,
      "youngModulus": 1200.0
    }
  }
}
```

### Cross-Section Types

| Type | Use Case | Properties |
|------|----------|-----------|
| **Round** | Natural fibers | Most common, isotropic |
| **Oval** | Twisted yarns | Realistic for most yarns |
| **Ribbon** | Flat wires, ribbons | Non-isotropic, flat appearance |
| **Rectangular** | Industrial yarns | Sharp edges, geometric |

### Denier Reference

Common deniers:
- `50` — Very fine (silk, lingerie)
- `100` — Fine (dress fabrics)
- `150` — Medium (common apparel)
- `300` — Coarse (upholstery)
- `600` — Very coarse (industrial)

### Material Properties

| Property | Range | Notes |
|----------|-------|-------|
| **Density** | 0.9-1.54 | g/cm³ |
| **Elasticity** | 0.0-0.5 | Dimensional change |
| **Dampening** | 0.05-0.5 | Energy absorption |
| **Roughness** | 0.0-1.0 | Surface texture |

---

## Simulation Results

### Result Structure

```json
{
  "simulationResult": {
    "metadata": {
      "simulationId": "sim-2024-001",
      "timestamp": "2024-01-15T14:45:00Z",
      "computationTime": 12.5,
      "isConverged": true,
      "convergenceError": 9.8e-7
    },
    "geometry": {
      "vertexCount": 10240,
      "triangleCount": 3410,
      "vertices": [
        { "x": 0.0, "y": 0.0, "z": 0.0 },
        { "x": 0.1, "y": 0.0, "z": 0.05 }
        // ... more vertices
      ],
      "triangles": [
        [0, 1, 2],
        [1, 2, 3]
        // ... more triangles
      ]
    },
    "analysis": {
      "fabricProperties": {
        "thickness": 0.8,
        "porosity": 0.45,
        "coverageFactor": 0.78,
        "airPermeability": 125.5,
        "tensileStrength": 180.0
      },
      "stressData": [
        12.5, 14.3, 11.8
        // ... per vertex
      ],
      "strainData": [
        0.05, 0.04, 0.06
        // ... per vertex
      ]
    }
  }
}
```

### Export Formats

**OBJ (3D Model):**
```obj
# WinUI FabSim Export
# Vertices
v 0.0 0.0 0.0
v 0.1 0.0 0.05
v 0.2 0.0 0.0

# Faces
f 1 2 3
f 2 3 4
```

**CSV (Analysis Data):**
```csv
Vertex,X,Y,Z,Stress,Strain
1,0.0,0.0,0.0,12.5,0.05
2,0.1,0.0,0.05,14.3,0.04
3,0.2,0.0,0.0,11.8,0.06
```

---

## Import/Export Formats

### Supported Import Formats

| Format | Extension | Purpose |
|--------|-----------|---------|
| WinUI FabSim | `.weave` | Native format |
| Jacquard ASCII | `.asc` | Jacquard machine format |
| Jacquard DUP | `.dup` | Jacquard designer format |
| PNG/JPG | `.png/.jpg` | For color palette extraction |
| SVG | `.svg` | Vector designs |

### Supported Export Formats

| Format | Extension | Use Case |
|--------|-----------|----------|
| WinUI FabSim | `.weave` | Project archival |
| JSON | `.json` | Data interchange |
| CSV | `.csv` | Spreadsheet analysis |
| SVG | `.svg` | 2D visualization |
| PNG/JPG | `.png/.jpg` | Image sharing |
| OBJ | `.obj` | 3D CAD import |
| FBX | `.fbx` | Game engine import |
| PDF | `.pdf` | Documentation |

### Color Palette Format

```json
{
  "colorPalette": {
    "name": "Cotton Blend",
    "colors": [
      {
        "name": "Red",
        "hexValue": "#FF0000",
        "rgba": { "r": 255, "g": 0, "b": 0, "a": 255 },
        "usage": "warp"
      },
      {
        "name": "Blue",
        "hexValue": "#0000FF",
        "rgba": { "r": 0, "g": 0, "b": 255, "a": 255 },
        "usage": "weft"
      }
    ]
  }
}
```

---

## Model Builder Packages

### Package Structure

```json
{
  "modelBuilderPackage": {
    "metadata": {
      "packageId": "pkg-2024-001",
      "name": "Advanced Crimp Calculator",
      "version": "1.0.0",
      "author": "Your Name",
      "description": "Calculates crimp factor based on yarn properties",
      "created": "2024-01-15T10:30:00Z",
      "lastModified": "2024-01-15T14:45:00Z"
    },
    "parameters": [
      {
        "id": "yarn_denier",
        "name": "Yarn Denier",
        "type": "number",
        "description": "Yarn denier (weight)",
        "min": 50,
        "max": 1000,
        "default": 150,
        "unit": "denier"
      },
      {
        "id": "thread_density",
        "name": "Thread Density",
        "type": "number",
        "description": "Ends per centimeter",
        "min": 5,
        "max": 100,
        "default": 20,
        "unit": "ends/cm"
      }
    ],
    "governingEquations": [
      {
        "id": "eq-crimp",
        "name": "Crimp Factor",
        "expression": "0.2 * log(yarn_denier) * (thread_density / 20)",
        "description": "Calculate crimp based on yarn properties"
      }
    ],
    "outputs": [
      {
        "id": "out_crimp",
        "name": "Crimp Factor",
        "equation": "eq-crimp",
        "unit": "ratio",
        "min": 0.0,
        "max": 1.0
      }
    ],
    "parameterPresets": [
      {
        "name": "Fine Cotton",
        "values": {
          "yarn_denier": 100,
          "thread_density": 30
        }
      },
      {
        "name": "Coarse Polyester",
        "values": {
          "yarn_denier": 300,
          "thread_density": 15
        }
      }
    ]
  }
}
```

### Package Manifest

```json
{
  "manifest": {
    "manifestVersion": "1.0",
    "packageId": "pkg-2024-001",
    "name": "My Custom Model",
    "version": "1.0.0",
    "author": "Your Name",
    "license": "MIT",
    "dependencies": [],
    "capabilities": [
      "EquationResolution",
      "ParameterBinding",
      "ViewerControls"
    ]
  }
}
```

### Validation Schema

Packages are validated against:
1. Manifest correctness
2. Equation syntax and validity
3. Parameter ranges and types
4. Output consistency

---

## Data Type Reference

### Numeric Types

```csharp
double        // 64-bit floating point (default for measurements)
float         // 32-bit floating point
decimal       // High precision (not used in simulation)
int           // Integer count values
```

### Common Units

| Property | Unit | Range |
|----------|------|-------|
| **Denier** | denier | 50-1000 |
| **Density** | ends/cm or picks/cm | 5-100 |
| **Linear Density** | g/cm | 0.01-2.0 |
| **Thickness** | mm | 0.1-5.0 |
| **Crimp** | ratio | 0.0-1.0 |
| **Elasticity** | ratio | 0.0-1.0 |
| **Stress** | MPa | 0-2000 |
| **Strain** | ratio | 0.0-1.0 |

### Color Representation

```json
{
  "colorRgba": {
    "r": 255,      // 0-255
    "g": 128,      // 0-255
    "b": 64,       // 0-255
    "a": 255       // 0-255 (0=transparent, 255=opaque)
  },
  "colorHex": "#FF8040"
}
```

---

**See Also:** [API Reference](06-API-REFERENCE.md), [Architecture](04-ARCHITECTURE.md)
