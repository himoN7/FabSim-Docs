# Getting Started with WinUI FabSim

Quick start guide to get you up and running in minutes.

## 5-Minute Quickstart

### 1. Install the Application

**Option A: MSIX Bundle (Recommended)**
1. Download `WinUi FabSim_0.4.0_x64_ARM64.msixbundle`
2. Double-click the file
3. Follow the installer prompts
4. Launch from Start Menu

**Option B: Portable Executable**
1. Download portable `.exe`
2. Run directly (no installation needed)
3. All user data stored locally

### 2. Launch for the First Time

1. Open WinUI FabSim from your applications
2. Wait for initial setup (first launch takes ~10 seconds)
3. You'll see the Home Page

### 3. Create Your First Weave

**Simple 2-minute weave:**

1. Click **Weave Design** in the sidebar
2. Select **Built-in Patterns** → **Plain Weave**
3. You now have a basic weave pattern!
4. Click **Fabric Physics** to see it in 2D

### 4. Run a Quick Simulation

1. Go to **Fabric Physics** → **2D Simulation**
2. Click **Run Simulation** (blue button)
3. Watch the yarn overlay preview
4. Adjust thread density slider to see changes in real-time

### 5. Export Your Design

1. Click **File** → **Export**
2. Choose format (SVG for images, JSON for data)
3. Select save location
4. Click **Export**

**That's it!** You've completed a full design-to-export workflow.

---

## Learning Path

### Beginner (30 minutes)

**Goal:** Understand basic concepts and navigate the UI

1. **Watch/Read:** [Home Page Overview](01-USER-GUIDE.md#1-home-page)
2. **Do:** Create 3 different built-in patterns
3. **Explore:** Switch between 2D and 3D views
4. **Learn:** [Weave Design Basics](01-USER-GUIDE.md#2-weave-design)

**Checkpoint:** You can create and view basic weaves ✓

---

### Intermediate (1-2 hours)

**Goal:** Design custom weaves and simulate physics

1. **Create:** A custom weave pattern from scratch
   - Start with blank canvas
   - Design a simple twill pattern
   - Adjust thread count and density
   
2. **Customize:** Color and appearance
   - Set warp and weft colors
   - Create color repeats
   - Save color palette

3. **Simulate:** Physics calculations
   - Run 2D simulation
   - Experiment with yarn properties
   - Compare different densities

4. **Export:** Your first design
   - Export as SVG image
   - Export as JSON data file

**Checkpoint:** You can design custom weaves and export results ✓

---

### Advanced (2-4 hours)

**Goal:** Use 3D simulation, Model Builder, and analysis tools

1. **3D Simulation:**
   - Run 3D simulation with different yarn path models
   - Experiment with Peirce vs. Bézier models
   - Adjust fabric deformation parameters
   - Rotate and zoom 3D viewport

2. **Model Builder:**
   - Start with a template model
   - Modify equations and parameters
   - Use AI assistance for suggestions
   - Publish as custom package

3. **Analysis:**
   - Calculate fabric properties
   - Compare different designs
   - Generate analysis reports

4. **Advanced Export:**
   - Export 3D geometry as OBJ/FBX
   - Generate analysis PDFs
   - Batch export multiple formats

**Checkpoint:** You're now an advanced user ✓

---

## Common Workflows

### Workflow 1: Design a Weave from Scratch

```
1. Weave Design → Blank Canvas
2. Set grid size (e.g., 8x8)
3. Click cells to create pattern
4. Adjust thread count (ends/cm, picks/cm)
5. Set yarn properties (denier, density)
6. Save project (Ctrl+S)
```

**Time:** 15 minutes

### Workflow 2: Compare Two Designs

```
1. Open Design A
2. Simulate and note properties
3. File → Open Recent → Design B
4. Simulate and compare
5. Export both for side-by-side comparison
```

**Time:** 10 minutes

### Workflow 3: 3D Geometry Export

```
1. Create/open weave design
2. Fabric Physics → 3D Simulation
3. Run simulation with desired yarn model
4. File → Export → Select OBJ format
5. Use in CAD or 3D software
```

**Time:** 20 minutes

### Workflow 4: Create Custom Model

```
1. Model Builder → Create Blank
2. Add input parameters
3. Write governing equations
4. Test with sample values
5. Publish as package
6. Use in future projects
```

**Time:** 30-60 minutes

---

## Tips & Best Practices

### Designing Weaves
- **Start simple:** Use templates before creating from scratch
- **Thread count matters:** Higher counts = finer fabric
- **Test variations:** Save multiple versions to compare
- **Use presets:** Save color palettes for consistency

### Running Simulations
- **Start basic:** Try 2D before 3D
- **Adjust gradually:** Make small parameter changes to see effects
- **Monitor resources:** Larger simulations take more time
- **Save progress:** Export results before experimenting further

### Exporting Data
- **Multiple formats:** Export both visual (SVG) and data (JSON)
- **Metadata included:** All exports contain project information
- **Batch export:** Use export dialog to save multiple formats
- **Archive projects:** Keep original .weave files for future editing

### Performance
- **Large grids:** May slow down simulation (start with ≤128x128)
- **3D rendering:** GPU-accelerated; ensure drivers are updated
- **Real-time preview:** Toggle off for large designs to improve speed

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| **Ctrl+N** | New project |
| **Ctrl+O** | Open project |
| **Ctrl+S** | Save project |
| **Ctrl+E** | Export |
| **Ctrl+Z** | Undo |
| **Ctrl+Y** | Redo |
| **F5** | Run simulation |
| **Space** | Pan canvas (drag) |
| **+** | Zoom in |
| **-** | Zoom out |
| **Ctrl+0** | Reset zoom |

---

## Common Issues & Quick Fixes

| Issue | Solution |
|-------|----------|
| App won't launch | Restart computer, check Windows 10+ requirement |
| Simulation hangs | Reduce grid size or close other apps |
| Can't save file | Check disk space and folder permissions |
| 3D view is slow | Update GPU drivers, reduce viewport resolution |
| Import fails | Verify file format is supported (.weave, .asc, .dup) |

**For more issues:** See [Troubleshooting Guide](08-TROUBLESHOOTING.md)

---

## Next Steps

- **Create a sample project:** 15 minutes
- **Explore all simulation modes:** 30 minutes  
- **Read full User Guide:** 1 hour
- **Learn Model Builder:** 2 hours
- **Master advanced features:** Ongoing

---

## Need Help?

1. **In-app:** Click "Help" → "Guides"
2. **Documentation:** See [User Guide](01-USER-GUIDE.md)
3. **Troubleshooting:** [Common Issues](08-TROUBLESHOOTING.md)
4. **GitHub:** [Report issues](https://github.com)
5. **Email:** Contact via app's About page

Good luck, and happy designing! 🧵
