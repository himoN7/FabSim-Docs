# Settings & Configuration Reference

**Version:** 0.4.0  
**Last Updated:** July 2026  
**Audience:** Users, Administrators, Developers

---

## Quick Reference

### Access Settings
```
Main Menu → Settings
OR
Keyboard: Ctrl+, (comma)
```

### Settings Organization
```
Settings are organized by category:
├─ Appearance & UI
├─ Physics & Simulation
├─ AI & Assistant
├─ Backup & Recovery
├─ Export & File Handling
├─ Performance
├─ Advanced
└─ About
```

---

## Appearance & UI Settings

### Theme

#### **Current Options**
```
Light Theme:
- White/light gray backgrounds
- Dark text
- High contrast
- Best for: Daytime, bright rooms
- Eye strain: Minimal

Dark Theme:
- Dark gray/black backgrounds
- Light text
- Lower brightness
- Best for: Evening, dim rooms
- Eye strain: Reduced in low light

Auto Theme:
- Switches based on system time
- Sunrise to sunset: Light
- Sunset to sunrise: Dark
- Best for: Automatic adaptation
```

#### **Setting**
```
Location: Settings → Appearance → Theme
Default: Auto
Restart Required: No (applies immediately)
```

### Viewport Backdrop

#### **Backdrop Options**
```
Color Variations:
1. Gray: Neutral (default)
2. Dark Gray: High contrast for light meshes
3. Light Gray: High contrast for dark meshes
4. Gradient Blue: Professional
5. Gradient Purple: Modern
6. Checkered: Transparency indication
7. Custom Color: User-defined
```

#### **Setting**
```
Location: Settings → Appearance → Viewport Backdrop
Default: Gray
Affects: 3D viewport only
Performance Impact: Negligible
```

### UI Scaling

#### **Font Size**
```
Sizes Available:
- Small: 90% (compact)
- Normal: 100% (default)
- Large: 110% (easier reading)
- Extra Large: 120% (accessibility)

Location: Settings → Appearance → Font Size
Default: Normal
Restart Required: No
```

#### **Window Behavior**
```
Remember Window Position:
- On: Restore exact position on restart
- Off: Start at center
- Default: On

Remember Window Size:
- On: Restore exact dimensions
- Off: Default 1920×1080
- Default: On

Fullscreen Mode:
- Off: Windowed
- On: Fullscreen
- Default: Off (windowed)
```

---

## Physics & Simulation Settings

### Default Physics Parameters

#### **Global Physics**
```
Gravity (g):
- Range: 0.0 - 20.0 m/s²
- Default: 9.81 m/s² (Earth)
- Affects: Fabric weight in simulation
- Options: 0 (None), 1.62 (Moon), 9.81 (Earth), Custom

Air Damping:
- Range: 0.0 - 1.0
- Default: 0.1
- Low (0.0): Bouncy, oscillating
- Medium (0.1-0.3): Realistic
- High (0.5+): Dampened, static

Wind Force:
- Range: 0.0 - 10.0
- Default: 0.0 (no wind)
- Direction: Global +Y direction
- Use for: Draping simulation

Friction Coefficient:
- Range: 0.0 - 2.0
- Default: 0.5
- 0.0: Frictionless (ice-like)
- 0.5: Typical textile
- 1.0+: Sticky interaction
```

#### **Setting Location**
```
Settings → Physics → Global Parameters
Apply To: All new simulations
Persist: Yes (saved between sessions)
```

### Solver Configuration

#### **Available Solvers**
```
Yarn XPBD (Extensible Position-Based Dynamics):
- Default solver
- Fast (~10-30 ms per frame)
- Stable convergence
- Good for real-time
- Use for: Design, quick feedback

Shell FEA (Finite Element):
- Advanced solver
- Slower (~100-500 ms per frame)
- More accurate
- Research-grade results
- Use for: Detailed analysis

Solver Selection:
- Per simulation (not global)
- Choose when creating new simulation
- Default: Yarn XPBD
```

### Convergence Tuning

#### **Solver Iterations**
```
Iterations per Frame:
- Range: 1-20
- Default: 8
- Lower: Faster, less accurate
- Higher: Slower, more accurate
- Impact: ~10 ms per iteration added

Target: Balance speed vs accuracy
- Real-time: 4-6 iterations
- High quality: 10-15 iterations
- Research: 15-20 iterations

Location: Settings → Physics → Solver Tuning
Persistence: Global default, overridable per simulation
```

#### **Time Step & Tolerance**
```
Time Step:
- Range: 0.001 - 0.05 seconds
- Default: 0.016 seconds (60 FPS)
- Smaller: More stable, slower
- Larger: Faster, less stable
- Rule: DeltaT ≤ 0.02s for stability

Constraint Tolerance:
- Range: 0.0001 - 0.01 (in meters)
- Default: 0.001 m (1 mm)
- Tighter: More accurate, slower
- Looser: Faster, less precise
```

### Material Defaults

#### **Fiber Material Presets**
```
Cotton:
- Young's Modulus: 5 GPa
- Poisson Ratio: 0.3
- Density: 0.00155 g/mm³
- Damping: 0.05

Silk:
- Young's Modulus: 8 GPa
- Poisson Ratio: 0.32
- Density: 0.00133 g/mm³
- Damping: 0.03

Polyester:
- Young's Modulus: 12 GPa
- Poisson Ratio: 0.35
- Density: 0.00138 g/mm³
- Damping: 0.08

Wool:
- Young's Modulus: 6 GPa
- Poisson Ratio: 0.28
- Density: 0.00160 g/mm³
- Damping: 0.10

Custom:
- Define your own
- Saved for reuse
```

#### **Setting Location**
```
Settings → Physics → Material Defaults
Affects: New fabric specifications
Apply: Via FabricSimulationSpec material selection
```

---

## AI & Assistant Settings

### AI Provider Configuration

#### **Available Providers**
```
Offline Mode:
- Always available
- No internet required
- Heuristic-based suggestions
- Fast response
- Limited creativity

OpenAI API:
- Requires API key
- Internet required
- GPT-4 model
- Advanced suggestions
- Costs apply

Ollama Local:
- Requires local Ollama installation
- No internet during inference
- Privacy-preserving
- Configurable model
- No costs
```

#### **Configuration**

##### **Offline Mode (Default)**
```
Location: Settings → AI Assistant → Provider
Provider: Offline
No configuration needed
Always works as fallback
```

##### **OpenAI Configuration**
```
1. Go to Settings → AI Assistant → OpenAI
2. Enter API Key:
   - Get from: https://platform.openai.com/api-keys
   - Format: sk-... (long string)
   - Keep secret (don't share)
3. Select Model:
   - GPT-4 (recommended)
   - GPT-3.5 (faster, less capable)
4. Test Connection: Click "Test"
5. Save settings

Usage Charges:
- Per token basis
- GPT-4: ~$0.03 per request (typical)
- Monitor usage in OpenAI dashboard
```

##### **Ollama Configuration**
```
Prerequisites:
1. Download Ollama from ollama.ai
2. Install on your computer
3. Run: ollama pull llama2
4. Start Ollama service

Configuration:
1. Settings → AI Assistant → Ollama
2. Endpoint: http://localhost:11434
3. Model: llama2 (or other)
4. Click "Test Connection"
5. Should see "Connected"

Testing:
- First request takes longer (model loads)
- Subsequent requests faster
- Works offline after model loaded
```

### AI Suggestion Parameters

#### **Temperature Setting**
```
Range: 0.1 - 2.0
Default: 0.7

Low (0.1-0.3):
- Deterministic, focused
- Same input → same output
- Good for: Consistent, accurate suggestions

Medium (0.7):
- Balanced
- Some creativity
- Default for good reasons

High (1.0-2.0):
- Creative, random
- Same input → different outputs
- Good for: Brainstorming, exploration

Location: Settings → AI Assistant → Temperature
```

#### **Max Tokens**
```
Range: 100 - 4000 tokens
Default: 1024

Lower:
- Shorter responses
- Faster processing
- Lower cost

Higher:
- Longer, detailed responses
- More thorough suggestions
- Higher cost
```

### AI Usage Limits

#### **Rate Limiting**
```
Requests per Minute:
- Range: 1 - 60
- Default: 10
- Prevents: API overuse, quota issues

Daily Limit:
- Range: 10 - 500
- Default: 100
- Prevents: Excessive billing

Track Usage:
- Settings shows current day/month usage
- Links to OpenAI dashboard
- Allows budget monitoring
```

---

## Backup & Recovery Settings

### Auto-Save Configuration

#### **Auto-Save Timing**
```
Enabled: On/Off (default: On)
Interval: 1 - 30 minutes (default: 5)
On Navigation: Yes/No (default: Yes)

Behavior:
- Saves current project to temporary location
- Doesn't overwrite user-saved files
- Automatic in background
- No user interaction needed

Location: Settings → Backup & Recovery → Auto-Save
```

#### **Auto-Save Files**
```
Storage Location:
- Windows: %AppData%\FabSim\Projects\AutoSave\
- Mac: ~/Library/FabSim/Projects/AutoSave/
- Linux: ~/.local/share/FabSim/Projects/AutoSave/

Retention:
- Number of backups: 3-20 (default: 10)
- Age limit: None (space-based)
- Auto-cleanup: Oldest deleted when limit reached
```

### Manual Backup Options

#### **Create Backup**
```
File → Create Backup
Creates timestamped copy:
- Format: project_YYYYMMDD_HHMMSS.weave
- Location: %AppData%\FabSim\Backups\
- Retention: Manual (not auto-deleted)

Frequency Recommendation:
- Before major changes: Yes
- Before exporting: Optional
- Before closing app: Optional
```

#### **Restore from Backup**
```
File → Restore from Backup
Browse available backups:
- Shows all timestamped versions
- Select desired date/time
- Preview before restore
- Restore creates new working copy
```

### Recovery Options

#### **Enable Auto-Recovery**
```
Location: Settings → Backup & Recovery → Recovery
Auto-Recovery: On/Off (default: On)

On App Crash:
1. Restart application
2. Recovery dialog appears
3. Option to restore last project
4. Or start fresh

Recovery Storage:
- Separate from auto-save
- Preserved across crashes
- Manual cleanup available
```

---

## Export & File Handling Settings

### Default Export Format

#### **Primary Export**
```
Location: Settings → Export → Default Format
Current Options:
- OBJ (recommended)
- STL
- 3MF
- SVG
- DXF
- PNG

This format used:
- First time export
- Quick Export button
- Default in export dialog
```

### Export Quality Presets

#### **Mesh Quality**
```
Low:
- Fewer polygons (~50k)
- Faster export
- Smaller file
- Use for: Web, quick preview

Medium:
- Standard polygons (~200k)
- Balanced speed/quality
- Typical file size
- Use for: General use (default)

High:
- More polygons (~500k+)
- Slower export
- Larger files
- Use for: Detailed analysis

Location: Settings → Export → Mesh Quality
Per-format settings available
```

#### **Image Resolution**
```
Options: 640×480, 1024×768, 1920×1080, 2560×1440, 4K (3840×2160), Custom
Default: 1920×1080
Used for: PNG export, viewport screenshots

Higher resolution:
- Better quality prints
- Larger file size
- Slower export
- Better for: Print documentation
```

### File Association

#### **Open With FabSim**
```
.weave files:
- Double-click opens in FabSim
- Auto-detected if installed
- Can be reset in: Settings → File Association

Other formats (auto-open):
- OBJ, STL, PNG: System default
- CSV, WIF: Text editor default
```

---

## Performance Settings

### Performance Optimization

#### **GPU Acceleration**
```
GPU Acceleration:
- On: Use graphics card for physics (fast)
- Off: CPU-only simulation (slower)
- Default: On (if GPU detected)
- Auto-detect: Recommended

Threshold:
- Use GPU when: >4,096 particles
- Use CPU when: <4,096 particles
- Hybrid: Automatic selection

Location: Settings → Performance → GPU Acceleration
Requires: Restart to apply
GPU Requirements: NVIDIA or AMD (most modern cards)
```

#### **Rendering Quality**
```
Viewport Quality:
- Low (faster): 
  - Simpler shaders
  - 30 FPS minimum
  - Low memory
  
- Medium (balanced):
  - Standard shaders
  - 60 FPS typical
  - Moderate memory
  
- High (best):
  - Advanced shaders
  - 60 FPS (if capable)
  - More memory

Location: Settings → Performance → Rendering Quality
Default: Medium
Change doesn't require restart
```

#### **Particle Rendering**
```
Show Particles: On/Off
Affects: 3D viewport display
Performance: Minor impact when On
Use for: Debugging yarn paths
Default: Off
```

### Memory Management

#### **Memory Limits**
```
Max Memory for Simulation:
- Range: 512 MB - 8 GB
- Default: System memory ÷ 2
- Prevents: Excessive RAM usage
- Effect: Limits particle count

When Limit Reached:
- Reduce particle count automatically
- Warn user
- Or prevent simulation

Location: Settings → Performance → Memory Limit
Recommended: 2-4 GB on typical system
```

### Disk Space

#### **Cache Settings**
```
Cache Folder: %AppData%\FabSim\Cache\
Cache Size Limit: 100 MB - 5 GB
Default: 1 GB

Cache Contents:
- Thumbnail images
- Temporary exports
- Model compilation data

Auto-Cleanup: On (default)
- Removes oldest items when limit reached
- Manual clear available
```

---

## Advanced Settings

### Experimental Features

#### **Enable Beta Features**
```
Location: Settings → Advanced → Beta Features
Warning: Unstable features ahead

Available Beta:
- Shell FEA solver (advanced)
- GPU instancing (performance)
- Procedural texture generation
- Custom shader language
- Multi-scene projects (future)

Opt-in basis (disabled by default)
Restart required: Yes
Feedback encouraged: Yes
```

### Debug & Logging

#### **Logging Level**
```
Options:
- Error: Only critical errors
- Warning: Errors + warnings
- Info: General operational info
- Debug: Detailed diagnostic info
- Trace: Very verbose (development only)

Default: Warning
Location: Settings → Advanced → Logging

Log Storage:
- Location: %AppData%\FabSim\Logs\
- Rotation: Daily, 7-day retention
- Size: Single file ~50 MB
```

#### **Enable Debug Mode**
```
Enables:
- Additional validation checks
- Performance profiler overlay
- Memory tracking display
- Shader compilation logs
- Physics constraint visualization

Location: Settings → Advanced → Debug Mode
Default: Off
Performance Impact: 10-20% overhead when on
```

### Input & Hotkeys

#### **Custom Keyboard Shortcuts**
```
Location: Settings → Advanced → Keyboard Shortcuts

Can Customize:
- File operations (New, Open, Save)
- View controls (Zoom, Pan, Rotate)
- Simulation (Play, Pause, Reset)
- Tools (Export, Analysis)
- Navigation (Pages, Dialogs)

Cannot Customize:
- System shortcuts (Alt+F4, etc.)
- OS-reserved keys

Location: Settings → Advanced → Keyboard
Click "Reset Defaults" to restore built-in shortcuts
```

### Accessibility

#### **High Contrast Mode**
```
Enabled: On/Off
Affects: All UI elements
Improves: Readability for low vision
Use: When default colors insufficient

Location: Settings → Accessibility → High Contrast
Restart Required: No
```

#### **Keyboard Navigation**
```
Tab Navigation:
- Through all UI elements
- Follows logical order
- Ctrl+Tab: Jump to next section

Arrow Keys:
- Navigate lists and menus
- Control sliders
- Grid navigation in weave editor

Location: Settings → Accessibility → Keyboard Nav
Default: Enabled
Can disable if preferred
```

#### **Screen Reader Support**
```
Status: Partial support
Supported Areas:
- Main menus
- Dialog boxes
- Settings dialogs
- File operations

Recommended Readers:
- Windows: Narrator, NVDA
- Mac: VoiceOver
- Linux: Orca

Limitations:
- 3D viewport not fully supported
- Complex dialogs may need workarounds
```

---

## About & System Information

### Application Information

```
Version: 0.4.0
Build: YYYYMMDD.HHmm
Release Date: July 2026
Producer: Hamed Rezaei

Framework Version:
- .NET 8.0
- Windows App SDK 1.4+
- WinUI 3

Display:
- Location: Settings → About
```

### System Information

#### **Hardware Details**
```
Processor: Model and speed
RAM: Installed memory
GPU: Graphics card model
GPU Memory: VRAM available
Storage: Available disk space

Purpose: For support and compatibility troubleshooting
```

#### **Library Versions**
```
Community Toolkit: 8.4.2
HelixToolkit: 2.22.0
SharpDX: 4.2.0

Purpose: For technical support
Displayed in: Settings → About → Detailed Info
```

### Update & License

#### **Check for Updates**
```
Manual: Settings → About → Check for Updates
Automatic: Notify only, no auto-install
Release Channel: Stable (default)

Notification:
- Appears on app start if update available
- Manual check available
- Always check before updating
```

#### **License Information**
```
License: [As applicable]
Terms: [Link to license document]
Third-party: [Link to attributions]

Location: Settings → About → License
```

---

## Keyboard Shortcuts

### Settings-Related Shortcuts

```
Open Settings: Ctrl+,
Settings Search: Ctrl+Shift+,
Reset Settings: Ctrl+Shift+R (confirmation required)
```

---

## Platform-Specific Notes

### Windows

```
Settings Storage:
- Registry: HKEY_CURRENT_USER\Software\FabSim
- File: %AppData%\FabSim\settings.json

Backup:
- Auto-backup: %AppData%\FabSim\Backups\

GPU Support:
- NVIDIA CUDA preferred
- AMD HIP supported
- Intel integrated OK (slower)
```

### macOS

```
Settings Storage:
- ~/Library/Preferences/com.fabsim.plist
- ~/Library/FabSim/settings.json

Backup:
- Auto-backup: ~/Library/FabSim/Backups/

GPU Support:
- Metal API (Apple GPU)
- Limited CUDA support
```

### Linux

```
Settings Storage:
- ~/.config/FabSim/settings.json
- ~/.local/share/FabSim/

Backup:
- Auto-backup: ~/.local/share/FabSim/Backups/

GPU Support:
- CUDA (NVIDIA)
- HIP (AMD)
- Vulkan (experimental)
```

---

## Troubleshooting Settings

### Common Issues

#### **Settings Not Saving**
```
Solutions:
1. Check permissions on %AppData% folder
2. Restart application
3. Check available disk space
4. Run as Administrator (if needed)
5. Reset settings to defaults
```

#### **Performance Degradation**
```
Check:
- GPU acceleration disabled? (Re-enable)
- Rendering quality too high? (Reduce)
- Memory limit too low? (Increase)
- Logs too verbose? (Reduce logging level)
- Cache full? (Clear cache)
```

#### **Restore Default Settings**
```
Full Reset:
1. Settings → Advanced → Reset All Settings
2. Confirm action
3. Restart application
4. All defaults restored

Partial Reset:
- Individual sections can reset
- Located in each section's menu
```

---

## Getting Help

For settings-related support:
- See [User Guide](01-USER-GUIDE.md)
- Check [Troubleshooting Guide](08-TROUBLESHOOTING.md)
- Search settings help in application
- Contact support: support@fabsim.example.com
