# Installation & Setup Guide

Complete guide for building, developing, and deploying WinUI FabSim.

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Developer Setup](#developer-setup)
3. [Building](#building)
4. [Running Locally](#running-locally)
5. [Publishing & Deployment](#publishing--deployment)
6. [Troubleshooting Setup Issues](#troubleshooting-setup-issues)

## System Requirements

### For Users (Running App)

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **OS** | Windows 10 Build 1809 | Windows 11 |
| **Architecture** | x86 / x64 / ARM64 | x64 |
| **RAM** | 2GB | 4GB+ |
| **GPU** | Any DirectX 9+ | Modern GPU for 3D |
| **Disk** | 500MB | 1GB+ (for projects) |

### For Developers (Development)

| Component | Version | Notes |
|-----------|---------|-------|
| **OS** | Windows 10 Build 19041+ | Windows 11 recommended |
| **Visual Studio** | 2022+ | Community Edition acceptable |
| **.NET SDK** | 8.0+ | Required for CLI builds |
| **Windows App SDK** | Latest | Installed via VS workload |
| **Git** | Latest | For version control |

## Developer Setup

### Step 1: Clone Repository

```bash
git clone https://github.com/yourusername/WinUi-FabSim.git
cd WinUi-FabSim
```

### Step 2: Install Visual Studio Workload

**If Visual Studio 2022 is not installed:**

1. Download [Visual Studio Community 2022](https://visualstudio.microsoft.com/downloads/)
2. Run installer
3. Select **Windows App SDK** workload
4. Under "Individual components," ensure:
   - .NET 8 SDK installed
   - C# language support enabled
5. Complete installation

**If Visual Studio is already installed:**

1. Open **Visual Studio Installer**
2. Click **Modify** on Visual Studio 2022
3. Add **Windows App SDK** workload (if not present)
4. Click **Modify**

### Step 3: Restore Dependencies

```bash
# Using dotnet CLI
dotnet restore

# Or let Visual Studio handle it automatically on first open
```

### Step 4: Verify Installation

```bash
# Check .NET 8 SDK
dotnet --version

# List installed SDKs
dotnet --list-sdks

# Should show 8.x.xxx in the output
```

## Building

### Command Line Build

**Build for x64 Debug:**
```bash
cd "WinUi FabSim"
dotnet build -p:Platform=x64 -c Debug
```

**Build for x64 Release:**
```bash
dotnet build -p:Platform=x64 -c Release
```

**Build for x86:**
```bash
dotnet build -p:Platform=x86 -c Debug
```

**Build all platforms:**
```bash
dotnet build -p:Platform=x64 -c Debug
dotnet build -p:Platform=x86 -c Debug
dotnet build -p:Platform=ARM64 -c Debug  # ARM64 on native machine only
```

### Visual Studio Build

1. Open `WinUi FabSim.slnx` in Visual Studio
2. Select platform: **x64** (preferred for development)
3. Select configuration: **Debug**
4. **Build** → **Build Solution** (Ctrl+Shift+B)

**Build output location:**
```
WinUi FabSim/bin/x64/Debug/
```

### Build Issues & Solutions

| Issue | Solution |
|-------|----------|
| `.NET 8 SDK not found` | Install .NET 8 SDK from dotnet.microsoft.com |
| `Windows App SDK missing` | Install via Visual Studio Installer workload |
| `Cannot find WinUI` | Run `dotnet restore` to restore packages |
| `Namespace X not found` | Clean solution, rebuild (Visual Studio may need restart) |
| `"rimraf" command not found` | Install Node.js (required for some build tasks) |

## Running Locally

### From Visual Studio (Recommended for Development)

1. Open `WinUi FabSim.slnx`
2. Ensure **x64** is selected as active platform
3. Press **F5** or **Debug** → **Start Debugging**

**Debugging Features:**
- Set breakpoints by clicking line numbers
- Step through code (F10, F11)
- Inspect variables in debugger
- Watch window (Ctrl+Alt+W)
- Immediate window (Ctrl+Alt+I)

### From Command Line

```bash
# Build and run
cd "WinUi FabSim"
dotnet build -c Debug
dotnet run

# Or directly run published build
dotnet publish -c Release -p:Platform=x64
# Find .exe in publish folder and run
```

### Testing Locally

**Run all tests:**
```bash
dotnet test
```

**Run specific test project:**
```bash
dotnet test Fabsim.AutomatedTests/Fabsim.AutomatedTests.csproj
```

**Run with coverage (if configured):**
```bash
dotnet test /p:CollectCoverage=true
```

## Publishing & Deployment

### MSIX Bundle (Recommended)

MSIX is the modern Windows package format. One file contains x64 + ARM64.

#### Prerequisites

1. **Test Certificate** (for signing):
   ```bash
   cd scripts
   ./create-test-certificate.ps1 -TrustOnThisMachine
   ```
   
   Creates: `certs/WinUiFabSim-Test.pfx`
   Password: `FabSimTest` (or configured value)

2. **Trust Certificate** (first time only):
   ```bash
   ./trust-test-certificate.cmd
   ```

#### Build MSIX Bundle

**Via Publish Script:**
```bash
publish-msix.cmd
```

**Or PowerShell (if scripts blocked):**
```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\publish-msix.ps1
```

**Or Visual Studio:**
1. Right-click project
2. **Publish** → **msix** profile
3. Follow prompts

**Output Location:**
```
dist/WinUi FabSim_0.4.0_x64_ARM64.msixbundle
```

### Publishing on Microsoft Store

1. **Reserve App** on [Partner Center](https://partner.microsoft.com/dashboard)
   - Get Product Identity Name
   - Get Publisher ID

2. **Update Package Manifest:**
   - Edit `Package.appxmanifest`
   - Set `Identity Name` to match Partner Center
   - Set `Publisher` to match

3. **Rebuild & Publish:**
   ```bash
   publish-msix.cmd
   ```

4. **Upload** `.msixbundle` to Partner Center → Packages

5. **Submit** for certification

### Portable Executable (Alternative)

**Unpackaged Distribution:**
```bash
dotnet publish \
  -c Release \
  -p:Platform=x64 \
  -p:PublishUnpackagedExe=true \
  -o ./publish-win-x64-unpackaged
```

**Output:** Single `.exe` file with all dependencies

**Note:** Requires .NET 8 runtime on target machine

### Manual Distribution

1. **Create Package:**
   ```bash
   publish-msix.cmd
   ```

2. **Sign & Verify:**
   ```bash
   Get-AuthenticodeSignature dist/WinUi*.msixbundle
   ```

3. **Test Install** (before distribution):
   ```powershell
   Add-AppxPackage -Path "dist/WinUi FabSim_0.4.0_x64_ARM64.msixbundle"
   ```

4. **Distribute:**
   - Host on website
   - Email to users
   - GitHub releases
   - Microsoft Store

## Configuration Files

### Build Configuration

**WinUi FabSim.csproj:**
```xml
<PropertyGroup>
    <Version>0.4.0</Version>
    <TargetFramework>net8.0-windows10.0.19041.0</TargetFramework>
    <Platforms>x86;x64</Platforms>
    <!-- More settings... -->
</PropertyGroup>
```

**Update Version:**
1. Edit `WinUi FabSim.csproj`
2. Change `<Version>` tag
3. Rebuild

### Package Manifest

**Package.appxmanifest:**
```xml
<Package>
    <Identity Name="WinUiFabSim" Publisher="CN=himpi"/>
    <Properties>
        <DisplayName>WinUI FabSim</DisplayName>
        <PublisherDisplayName>Hamed Rezaei</PublisherDisplayName>
    </Properties>
</Package>
```

**To Update Publisher:**
1. Edit `Package.appxmanifest`
2. Change `Publisher` attribute
3. Regenerate certificate to match
4. Rebuild package

### App Settings

**Default Locations:**
```
%AppData%\WinUi FabSim\       (User data)
%AppData%\Local\WinUi FabSim\ (Cache)
```

**Modify Default Settings:**
1. Edit `AppSettings.cs` in Services/
2. Change default property values
3. Rebuild

## Environment Variables

**Development:**
```powershell
$env:FABSIM_DEBUG = "true"
$env:FABSIM_PHYSICS_VERBOSE = "true"
$env:DOTNET_TieredCompilation = "1"
```

**CI/CD:**
```bash
export CI=true
export BUILD_NUMBER=123
export VERSION=0.4.0
```

## Advanced Build Scenarios

### Clean Build

```bash
# Clean all artifacts
dotnet clean

# Or manually
Remove-Item -Recurse bin/
Remove-Item -Recurse obj/

# Then rebuild
dotnet build
```

### Build for Specific Architecture

```bash
# ARM64 (only works on ARM64 machine)
dotnet build -p:Platform=ARM64 -c Release

# x86
dotnet build -p:Platform=x86 -c Release

# x64
dotnet build -p:Platform=x64 -c Release
```

### Parallel Build

```bash
# Faster on multi-core systems
dotnet build -m
```

### Verbose Output

```bash
# For debugging build issues
dotnet build -v diagnostic
```

## Troubleshooting Setup Issues

### "Windows App SDK not installed"

**Solution:**
```powershell
# Check installed versions
Get-AppxPackage -Name "WindowsAppRuntimeMain"

# If not found:
# 1. Open Visual Studio Installer
# 2. Modify Visual Studio 2022
# 3. Install "Windows App SDK" workload
# 4. Restart computer
```

### ".NET 8 SDK not found"

**Solution:**
1. Download [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
2. Run installer
3. Verify: `dotnet --version` shows 8.x.xxx
4. Restart Visual Studio

### "Project file not found"

**Solution:**
```bash
# Ensure you're in correct directory
cd "WinUi FabSim"
ls *.csproj  # Should list the .csproj file

# Try explicit path
dotnet build "WinUi FabSim.csproj"
```

### "Cannot open package"

**Solution:**
```bash
# Run Visual Studio as Administrator
# Right-click: Run as Administrator
# Then open solution

# Or rebuild from command line:
dotnet clean
dotnet restore
dotnet build
```

### Build succeeds but app won't launch

**Solution:**
1. Check platform matches (x86 vs x64)
2. Ensure correct startup project selected
3. Clean and rebuild:
   ```bash
   dotnet clean
   dotnet build
   ```
4. Try launching from: `bin/x64/Debug/WinUi FabSim.exe`

### "Namespace not found" errors after restore

**Solution:**
```bash
# Clean and restore
dotnet clean
dotnet restore

# Restart Visual Studio
# Then rebuild
```

### Certificate issues with MSIX

**Solution:**
```bash
# Remove old certificate
Remove-Item certs/WinUiFabSim-Test.pfx

# Create new certificate
./scripts/create-test-certificate.ps1 -TrustOnThisMachine

# Trust on current machine
./scripts/trust-test-certificate.cmd

# Rebuild package
publish-msix.cmd
```

---

## Next Steps

1. **Complete setup** following steps 1-4 above
2. **Build** the application
3. **Run** locally to verify
4. **Run tests** to ensure environment
5. **Read** [Developer Guide](03-DEVELOPER-GUIDE.md)

For issues not covered here, see [Troubleshooting Guide](08-TROUBLESHOOTING.md).
