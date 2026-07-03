# Troubleshooting Guide

Solutions for common issues and problems.

## Table of Contents

1. [Application Won't Start](#application-wont-start)
2. [Runtime Errors](#runtime-errors)
3. [Physics & Simulation Issues](#physics--simulation-issues)
4. [UI & Rendering Problems](#ui--rendering-problems)
5. [File & Project Issues](#file--project-issues)
6. [Performance Issues](#performance-issues)
7. [Build & Compilation](#build--compilation)
8. [Getting Help](#getting-help)

## Application Won't Start

### App crashes immediately on launch

**Symptoms:** App closes within 1 second of launching

**Solutions:**
1. **Check Windows version:**
   - Run: `winver`
   - Must be Windows 10 Build 1809 or later
   - Windows 11 recommended

2. **Check .NET Runtime:**
   ```powershell
   dotnet --list-runtimes
   # Should show: Microsoft.WindowsDesktop.App 8.x.x
   ```
   If not installed: Download from [dotnet.microsoft.com](https://dotnet.microsoft.com)

3. **Reinstall the app:**
   - Uninstall: Settings → Apps → WinUI FabSim → Uninstall
   - Restart computer
   - Reinstall from latest bundle

### "Unhandled Exception" message appears

**Cause:** Usually corrupted settings or cache

**Solutions:**
1. **Clear app data:**
   ```powershell
   # Remove settings cache
   Remove-Item "$env:AppData\WinUi FabSim" -Recurse -Force
   ```

2. **Restart the app** — It will recreate default settings

### App won't start from Microsoft Store

**Solution:**
1. Go to Settings → Apps → Apps & features
2. Find "WinUI FabSim"
3. Click → Advanced options
4. Click "Repair"
5. Wait for repair to complete

---

## Runtime Errors

### "System.InvalidOperationException: Cannot create instance of type..."

**Cause:** Service dependency injection misconfiguration

**Solution:**
- **For users:** Reinstall the application
- **For developers:** Check that all services are properly registered in DI container

### "System.ArgumentNullException: Value cannot be null"

**Cause:** Required parameter is missing or null

**Solution:**
1. Ensure all fields are filled in dialog/input forms
2. Check file is valid before importing
3. Restart the application

### "OutOfMemoryException"

**Cause:** Application ran out of available memory

**Solutions:**
1. **Close other applications** to free memory
2. **Reduce simulation grid size:**
   - Use smaller fabric dimensions (e.g., 64x64 instead of 256x256)
   - Reduce thread density
3. **Restart the app** to clear memory

### "System.IO.IOException: File is in use"

**Cause:** File cannot be accessed because it's locked

**Solutions:**
1. **Close the file in other applications:**
   - Check if file is open in another instance of FabSim
   - Check if file is in use by Cloud sync (OneDrive, Google Drive)
2. **Move file to local drive:**
   - Avoid network/cloud storage locations
   - Use: `C:\Users\YourName\Documents\My Projects\`
3. **Try with different filename** if issue persists

---

## Physics & Simulation Issues

### Simulation takes too long to complete

**Typical times:**
- 32x32 grid: 2-5 seconds
- 64x64 grid: 10-30 seconds
- 128x128 grid: 1-3 minutes
- 256x256 grid: 5-15 minutes

**If taking longer:**

1. **Reduce grid size:**
   ```
   Fabric Physics → Settings → Grid Size → Select smaller
   ```

2. **Change solver type:**
   - Settings → Physics → Solver → Particle System (faster)
   - Or: FEA (slower but more accurate)

3. **Check system resources:**
   - Close other applications
   - Check disk space (need 1GB free)

4. **Update GPU drivers:**
   - NVIDIA: [Driver downloads](https://www.nvidia.com/Download/driverDetails.aspx)
   - AMD: [Driver downloads](https://www.amd.com/en/support)

### Simulation doesn't complete / hangs forever

**Solutions:**
1. **Cancel simulation:**
   - Click "Cancel" button or press Esc
   - Wait 10-30 seconds for graceful shutdown

2. **Force quit:**
   - Press Alt+F4
   - Or: Task Manager → End task

3. **Reduce problem complexity:**
   - Smaller fabric dimensions
   - Simpler yarn properties
   - Fewer material layers

4. **Check settings:**
   - Convergence tolerance not too strict
   - Max iterations sufficient
   - Time step reasonable

### Simulation converges but results look wrong

**Solutions:**
1. **Verify input parameters:**
   - Check thread density (should be > 0)
   - Check yarn denier (should be > 0)
   - Validate geometry parameters

2. **Try different yarn model:**
   - Fabric Physics → Settings → Yarn Path Model
   - Try: Peirce (most stable) or Kemp (more detailed)

3. **Check visualization settings:**
   - Toggle wireframe mode
   - Enable stress visualization
   - Rotate 3D view

4. **Export raw data:**
   - Export as JSON or CSV
   - Inspect numeric values
   - Check if values are in expected ranges

### 3D geometry looks deformed or incorrect

**Solutions:**
1. **Adjust camera:**
   - Right-click and drag to rotate
   - Scroll to zoom
   - Double-click to reset view

2. **Check scale:**
   - Try different zoom levels
   - Use "Fit to View" button

3. **Verify model selection:**
   - Fabric Physics → Settings → Select different yarn model
   - Some models may work better for specific weave types

---

## UI & Rendering Problems

### Controls not responding to clicks

**Solutions:**
1. **Check if processing:**
   - Wait for any running simulation to complete
   - Look for progress indicator

2. **Restart the application:**
   - Close completely (Alt+F4)
   - Wait 5 seconds
   - Reopen

3. **Clear UI cache:**
   ```powershell
   Remove-Item "$env:LocalAppData\WinUi FabSim" -Recurse -Force
   ```

### 3D viewport is black or not rendering

**Causes:** GPU initialization issue

**Solutions:**
1. **Update GPU drivers:**
   - NVIDIA: [Downloads](https://www.nvidia.com/Download/driverDetails.aspx)
   - AMD: [Downloads](https://www.amd.com/en/support)
   - Intel: [Downloads](https://ark.intel.com/content/www/us/en/ark.html)

2. **Disable hardware acceleration (temporary workaround):**
   - Settings → Appearance → Hardware Acceleration → Off
   - Performance will degrade

3. **Try integrated GPU:**
   - Some discrete GPUs may conflict
   - Try using integrated graphics instead

### Text is blurry or hard to read

**Solutions:**
1. **Adjust display scaling:**
   - Settings → Display → Scale and layout
   - Try 100% or 125%
   - Restart application

2. **Increase font size:**
   - Settings → Appearance → Font Size → Larger

### Colors look washed out or incorrect

**Solutions:**
1. **Check color profile:**
   - Settings → Color Management
   - Ensure correct profile for display

2. **Adjust theme:**
   - Settings → Appearance → Theme → Try Light/Dark/Auto

---

## File & Project Issues

### Cannot save project / "Access Denied"

**Causes:** Folder permissions issue

**Solutions:**
1. **Check folder permissions:**
   - Right-click folder → Properties → Security
   - Ensure your user has "Modify" permission

2. **Use different location:**
   - Save to: `C:\Users\YourName\Documents\`
   - Avoid: Network drives, Cloud storage initially

3. **Run as Administrator (Windows only):**
   - Right-click app → Run as administrator

### File corruption / "Cannot read file"

**Symptoms:** Error when opening saved project

**Solutions:**
1. **Try recovery file:**
   ```
   Check: %AppData%\WinUi FabSim\Recovery\
   ```

2. **Restore from backup:**
   - Check if cloud backup exists (OneDrive, Google Drive)
   - Check system restore point

3. **Export and recreate:**
   - If data exportable, reconstruct project
   - Contact support with error message

### Cannot import weave file

**Causes:** Unsupported format or corrupted file

**Supported formats:**
- `.weave` — WinUI FabSim format
- `.asc` — Jacquard format
- `.dup` — Jacquard format

**Solutions:**
1. **Verify file format:**
   - Check file extension
   - Try opening in text editor to inspect

2. **Convert from other software:**
   - Export from source software as supported format
   - Then import into FabSim

3. **Try sample files:**
   - Use built-in patterns first
   - If those work, issue is with imported file

### Export fails or produces empty files

**Solutions:**
1. **Ensure project is complete:**
   - All required fields filled
   - Simulation completed successfully

2. **Check disk space:**
   - Must have 500MB+ free
   - Cannot export to read-only location

3. **Use different format:**
   - Try exporting as JSON instead of CSV
   - Or try SVG instead of PNG

4. **Choose different location:**
   - Avoid network drives
   - Use local folder: `C:\Users\YourName\Documents\`

---

## Performance Issues

### Application is slow/laggy

**Causes:**
- Large simulation grids
- Complex geometry calculations
- Insufficient system memory
- Outdated GPU drivers

**Solutions:**

1. **Reduce workload:**
   - Use smaller fabric dimensions
   - Reduce grid size from 128x128 to 64x64
   - Use simpler yarn models

2. **Close other applications:**
   - Free up memory
   - Reduce CPU contention

3. **Enable performance mode:**
   - Settings → Advanced → Performance Mode → On

4. **Update drivers:**
   - GPU drivers must be current
   - Check for Windows updates

5. **Increase virtual memory:**
   ```powershell
   # Settings → System → About → Advanced system settings
   # → Virtual memory → Set to 2x RAM
   ```

### High CPU usage even when idle

**Solutions:**
1. **Disable recovery autosave:**
   - Settings → Advanced → Autosave → Off

2. **Close background processes:**
   - Task Manager → End unnecessary tasks

3. **Check if simulation running:**
   - Simulation may be background processing
   - Check progress indicator

---

## Build & Compilation

### Build fails with "Project file not found"

**Solution:**
```bash
# Ensure correct working directory
cd "WinUi FabSim"

# Verify file exists
dir *.csproj

# Try full path
dotnet build "C:\Users\yourname\source\repos\WinUi FabSim\WinUi FabSim\WinUi FabSim.csproj"
```

### "NuGet package not found"

**Solution:**
```bash
# Restore packages
dotnet restore

# Update packages
dotnet nuget update source "nuget.org"

# Clear cache
dotnet nuget locals all --clear
```

### Build succeeds but app won't run

**Solutions:**
1. **Check platform:**
   - Ensure x64 matches your system
   - Use x86 if on 32-bit Windows (rare)

2. **Check dependencies:**
   ```bash
   dotnet build -v diagnostic
   ```

3. **Rebuild from scratch:**
   ```bash
   dotnet clean
   dotnet restore
   dotnet build
   ```

---

## Getting Help

### Gathering Diagnostic Information

**For bug reports, collect:**

1. **Error message:** Full text of error
2. **Steps to reproduce:** Exact sequence to cause issue
3. **System info:**
   ```powershell
   # Windows version
   winver
   
   # .NET version
   dotnet --version
   
   # GPU info
   Get-WmiObject Win32_VideoController | Select-Object Name
   ```

4. **Application logs:**
   ```
   Location: %AppData%\WinUi FabSim\Logs\
   ```

5. **Screenshots:** Visual representation of issue

### Where to Report Issues

1. **GitHub Issues:** [New Issue](https://github.com)
   - Search existing issues first
   - Provide reproducible example
   - Include diagnostic info above

2. **GitHub Discussions:** General questions

3. **Producer Email:** Through app's About page

### Additional Resources

- **User Guide:** [01-USER-GUIDE.md](01-USER-GUIDE.md)
- **Developer Guide:** [03-DEVELOPER-GUIDE.md](03-DEVELOPER-GUIDE.md)
- **Architecture:** [04-ARCHITECTURE.md](04-ARCHITECTURE.md)
- **Installation:** [05-INSTALLATION-SETUP.md](05-INSTALLATION-SETUP.md)

---

## Still Need Help?

**Common issues checklists:**

**Application won't start:**
- [ ] Windows 10 Build 1809+?
- [ ] .NET 8 installed?
- [ ] GPU drivers updated?
- [ ] Sufficient disk space (500MB)?

**Simulation issues:**
- [ ] Grid size ≤ 128x128?
- [ ] Yarn parameters valid?
- [ ] Free memory available?
- [ ] GPU drivers current?

**File issues:**
- [ ] Folder permissions correct?
- [ ] File extension supported?
- [ ] File not corrupted?
- [ ] Sufficient disk space?

**Performance issues:**
- [ ] Other apps closed?
- [ ] GPU drivers updated?
- [ ] Virtual memory configured?
- [ ] Grid size reasonable?

If issues persist after all above checks, please report to GitHub Issues with diagnostic information.
