# Deployment & Release

Complete deployment procedures for building, packaging, and releasing WinUI FabSim.

## Table of Contents

1. [Release Planning](#release-planning)
2. [Build Process](#build-process)
3. [MSIX Packaging](#msix-packaging)
4. [Quality Checks](#quality-checks)
5. [Deployment Options](#deployment-options)
6. [Release Management](#release-management)
7. [Version Management](#version-management)

## Release Planning

### Release Cycle

**Semantic Versioning:** `MAJOR.MINOR.PATCH`

- **MAJOR:** Breaking changes (0.x.x → 1.0.0)
- **MINOR:** New features, backward compatible (0.3.0 → 0.4.0)
- **PATCH:** Bug fixes, patches (0.4.0 → 0.4.1)

### Release Schedule

| Type | Frequency | Example |
|------|-----------|---------|
| **Major** | Yearly | 0.x.x → 1.0.0 |
| **Minor** | Monthly | 0.3.x → 0.4.0 |
| **Patch** | As needed | 0.4.0 → 0.4.1 |

### Pre-Release Checklist

- [ ] All tests pass (100% pass rate)
- [ ] Code coverage ≥75%
- [ ] No critical bugs open
- [ ] Documentation updated
- [ ] Performance validated
- [ ] Security review complete
- [ ] Translation/localization done
- [ ] User guide updated

---

## Build Process

### Version Update

**Update in `WinUi FabSim.csproj`:**

```xml
<PropertyGroup>
    <Version>0.4.0</Version>
    <AssemblyVersion>0.4.0.0</AssemblyVersion>
    <FileVersion>0.4.0.0</FileVersion>
    <InformationalVersion>0.4.0</InformationalVersion>
    <ApplicationDisplayVersion>0.4.0</ApplicationDisplayVersion>
    <ApplicationVersion>40000</ApplicationVersion>
</PropertyGroup>
```

**ApplicationVersion calculation:**
```
ApplicationVersion = MAJOR*10000 + MINOR*100 + PATCH
Example: 0.4.0 → 0*10000 + 4*100 + 0 = 400
```

### Release Build

```bash
# Full clean build
dotnet clean
dotnet restore

# Build for x64
dotnet build -c Release -p:Platform=x64

# Build for x86
dotnet build -c Release -p:Platform=x86

# Build for ARM64 (on ARM64 machine)
dotnet build -c Release -p:Platform=ARM64
```

### Build Verification

```bash
# Check build output
ls "WinUi FabSim/bin/x64/Release/net8.0-windows10.0.19041.0/"

# Expected files:
# - WinUi FabSim.exe
# - WinUi FabSim.dll
# - Dependencies...
```

---

## MSIX Packaging

### Prerequisites

1. **Test Certificate:**
   ```bash
   cd scripts
   ./create-test-certificate.ps1 -TrustOnThisMachine
   # Creates: certs/WinUiFabSim-Test.pfx
   ```

2. **Trust Certificate:**
   ```bash
   ./trust-test-certificate.cmd
   ```

### Package Build Process

**Using Script:**
```bash
publish-msix.cmd
```

**Manual Process:**

```powershell
# Set environment
$ProjectPath = "WinUi FabSim/WinUi FabSim.csproj"

# Build MSIX bundle
dotnet publish `
  -c Release `
  -f net8.0-windows10.0.19041.0 `
  -p:GenerateAppxPackageOnBuild=true `
  -p:AppxPackageSigningEnabled=true `
  -p:PackageCertificateKeyFile="$(pwd)/certs/WinUiFabSim-Test.pfx" `
  -p:PackageCertificatePassword="FabSimTest" `
  "$ProjectPath"
```

### Package Output

**Location:**
```
WinUi FabSim/AppPackages/
└── WinUi FabSim_0.4.0_x64_ARM64/
    ├── WinUi FabSim_0.4.0_x64_ARM64.msixbundle
    ├── WinUi FabSim_0.4.0_x64.msix
    ├── WinUi FabSim_0.4.0_arm64.msix
    ├── Install.ps1
    ├── Dependencies/
    │   ├── x64/
    │   ├── x86/
    │   └── arm64/
    └── ...
```

**Distribution File:**
```
dist/WinUi FabSim_0.4.0_x64_ARM64.msixbundle
```

### Package Signing

**View Certificate:**
```powershell
Get-ChildItem Cert:\CurrentUser\My | 
  Where-Object { $_.Subject -like "*himpi*" }
```

**Verify Signature:**
```powershell
Get-AuthenticodeSignature "dist/WinUi FabSim_0.4.0_x64_ARM64.msixbundle"
```

---

## Quality Checks

### Pre-Deployment Testing

```bash
# Run full test suite
dotnet test --configuration Release

# Run specific test category
dotnet test --filter "Category=Integration|Regression"

# Generate coverage report
dotnet test /p:CollectCoverage=true /p:CoverageFormat=opencover

# Performance validation
dotnet test --filter "Category=Stress" -v normal
```

### Manual Testing Checklist

- [ ] **Install:** Successfully install MSIX on clean Windows 10/11
- [ ] **Startup:** App launches without errors
- [ ] **UI:** All controls respond correctly
- [ ] **Core Features:**
  - [ ] Can create new weave
  - [ ] Can run 2D simulation
  - [ ] Can run 3D simulation
  - [ ] Can export data
- [ ] **Performance:** No lag or freezing
- [ ] **Stability:** Run for 30+ minutes without crashes
- [ ] **Settings:** Settings persist across restarts

### Security Checks

- [ ] No hardcoded credentials
- [ ] No sensitive data logged
- [ ] All inputs validated
- [ ] Security review completed

---

## Deployment Options

### Option 1: MSIX Bundle (Recommended)

**File:** `WinUi FabSim_0.4.0_x64_ARM64.msixbundle`

**Distribution Methods:**
- Website download
- GitHub releases
- Email
- Cloud storage

**Installation:**
```powershell
Add-AppxPackage -Path "WinUi FabSim_0.4.0_x64_ARM64.msixbundle"
```

### Option 2: Microsoft Store

**Submission Steps:**

1. **Create Developer Account:**
   - Visit [Partner Center](https://partner.microsoft.com/dashboard)
   - Create account and app entry

2. **Update Package Manifest:**
   ```xml
   <Package>
       <Identity Name="WinUiFabSim" 
                  Publisher="CN=Hamed Rezaei"/>
   </Package>
   ```

3. **Upload Bundle:**
   - Go to Partner Center
   - Products → Your App → Packages
   - Upload `.msixbundle`

4. **Configure Store Listing:**
   - Add screenshots
   - Write description
   - Set pricing
   - Configure availability

5. **Submit for Certification:**
   - Review submission
   - Submit for Microsoft certification
   - Wait 2-3 days for approval

6. **Monitor and Update:**
   - Track downloads/ratings
   - Respond to reviews
   - Plan updates

### Option 3: Portable Executable

**Create Portable Build:**

```bash
dotnet publish \
  -c Release \
  -p:Platform=x64 \
  -p:PublishUnpackagedExe=true \
  -p:SelfContained=true \
  -o ./publish-portable
```

**Output:** Single `.exe` file with runtime included

**Distribution:** Direct `.exe` download (larger file, no installation needed)

### Option 4: GitHub Releases

**Create Release:**

```bash
# Create tag
git tag v0.4.0
git push origin v0.4.0

# Create release on GitHub
# Add release notes
# Upload:
# - WinUi FabSim_0.4.0_x64_ARM64.msixbundle
# - WinUi-FabSim-portable-0.4.0.exe
# - sha256-checksums.txt
```

---

## Release Management

### Release Notes Template

```markdown
# WinUI FabSim 0.4.0

**Release Date:** January 15, 2024

## What's New

### Features
- ✨ Added Olofsson yarn model
- ✨ Improved AI equation suggestions
- ✨ New export formats (FBX, GLTF)

### Improvements
- 🚀 30% faster 3D simulation
- 🎨 Enhanced UI responsiveness
- 📚 Extended documentation

### Bug Fixes
- 🐛 Fixed memory leak in physics engine
- 🐛 Corrected crimp factor calculation
- 🐛 Resolved crashes on large grids

## System Requirements
- Windows 10 Build 1809+ / Windows 11
- 4GB RAM
- 500MB disk space

## Installation
[Download MSIX Bundle](link-to-bundle)

## Known Issues
- Large grids (>256x256) may be slow on older hardware
- GPU rendering requires up-to-date drivers

## Upgrading
Simply install the new version over existing installation.

## Credits
- **Producer:** Hamed Rezaei
- **Contributors:** [List]
```

### Changelog Management

**File:** `CHANGELOG.md`

```markdown
# Changelog

All notable changes to WinUI FabSim are documented here.

## [0.4.0] - 2024-01-15

### Added
- Olofsson yarn path model
- AI-assisted equation builder enhancements
- FBX and GLTF export formats

### Changed
- Performance optimizations for physics engine
- Improved UI responsiveness

### Fixed
- Memory leak in simulation context
- Incorrect crimp factor formula

## [0.3.9] - 2024-01-01

### Added
- Model Builder parameters presets

### Fixed
- Export dialog not remembering last location
```

---

## Version Management

### Version Scheme

**Current:** `0.4.0`

**Next Minor:** `0.5.0` (new features)
**Next Patch:** `0.4.1` (bug fixes)
**Next Major:** `1.0.0` (breaking changes)

### Update Paths

**0.3.x → 0.4.0:**
- Backward compatible
- Auto-migrate projects if needed
- Show migration notice

**0.4.0 → 0.4.1:**
- No breaking changes
- Direct upgrade
- No data migration

**0.4.x → 1.0.0:**
- Breaking changes possible
- Migration guide required
- Separate download recommended

### Version Information

**Display Version:**
```xml
<ApplicationDisplayVersion>0.4.0</ApplicationDisplayVersion>
```

**File Version:**
```xml
<FileVersion>0.4.0.0</FileVersion>
```

**Query at Runtime:**
```csharp
var version = AppVersion.Current;  // "0.4.0"
var buildDate = AppVersion.BuildDate;  // DateTime
```

---

## Rollback Procedure

**If Critical Issue Found:**

1. **Immediate Actions:**
   ```bash
   # Remove latest release
   git tag -d v0.4.0
   git push origin --delete v0.4.0
   ```

2. **Revert to Previous:**
   ```bash
   git revert HEAD
   git push origin main
   git tag v0.4.0-rollback
   ```

3. **Notify Users:**
   - GitHub issue with explanation
   - Recommend downgrade
   - Provide previous version download

4. **Investigation:**
   - Root cause analysis
   - Fix implementation
   - Extensive testing
   - New release (0.4.1)

---

## Deployment Checklist

**Final Pre-Release:**

- [ ] Version number updated everywhere
- [ ] Changelog completed
- [ ] Release notes written
- [ ] All tests pass (100%)
- [ ] Code coverage ≥75%
- [ ] Performance validated
- [ ] Security review complete
- [ ] Documentation updated
- [ ] Build succeeds without warnings
- [ ] Package signed correctly
- [ ] Installation tested on clean system
- [ ] All features verified working
- [ ] No known critical bugs
- [ ] Package ready for distribution

**Post-Release:**

- [ ] Release published
- [ ] Users notified
- [ ] GitHub released created
- [ ] Store listing updated
- [ ] Website updated
- [ ] Documentation deployed
- [ ] Monitoring enabled
- [ ] Support ready for issues

---

**See Also:** [Installation & Setup](05-INSTALLATION-SETUP.md), [Testing & Quality](10-TESTING-QUALITY.md)
