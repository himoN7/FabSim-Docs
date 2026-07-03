# WinUI FabSim Developer Guide

Complete reference for developers working on WinUI FabSim codebase.

## Table of Contents

1. [Overview](#overview)
2. [Tech Stack](#tech-stack)
3. [Project Structure](#project-structure)
4. [Getting Started](#getting-started-for-developers)
5. [Architecture Patterns](#architecture-patterns)
6. [Development Workflows](#development-workflows)
7. [Code Standards](#code-standards)
8. [Testing Strategy](#testing-strategy)

## Overview

**WinUI FabSim** is a modern Windows desktop application for fabric simulation and design. The codebase emphasizes:

- **MVVM Architecture:** Clear separation of UI, logic, and data
- **Reactive Programming:** Uses MVVM Toolkit for property notification
- **Service-Oriented:** Dependency injection and loose coupling
- **Performance:** Optimized physics calculations with GPU support
- **Testability:** Comprehensive unit and integration test coverage

**Key Metrics:**
- **Language:** C# (.NET 8)
- **UI Framework:** WinUI 3
- **Code Style:** Modern C# with nullable reference types
- **Test Coverage:** Unit, Integration, Automation, Regression tests
- **Build Targets:** x86, x64, ARM64

## Tech Stack

### Core Framework
- **.NET 8** — Latest LTS framework
- **WinUI 3** — Modern native Windows UI
- **Windows App SDK** — Latest Windows platform APIs

### Libraries & Packages
- **Community Toolkit MVVM** (8.4.2) — MVVM infrastructure
- **Community Toolkit Animations** (8.2) — Smooth transitions
- **HelixToolkit** — 3D rendering (SharpDX)
- **System.Text.Json** — JSON serialization
- **CommunityToolkit.Diagnostics** — Diagnostics utilities

### Development Tools
- **Visual Studio 2022** — Primary IDE
- **Git** — Version control
- **NUnit** — Testing framework
- **FakeItEasy** — Mocking library

### Physics & Simulation
- **Particle Systems** — Custom yarn simulation
- **Constraint Solvers** — FEA integration
- **Geometry Engines** — Multiple yarn path models

## Project Structure

```
WinUi FabSim/
├── Views/                          # XAML UI pages
│   ├── HomePage.xaml
│   ├── WeaveDesignPage.xaml
│   ├── FabricPhysicsPage.xaml
│   ├── ModelBuilderPage.xaml
│   ├── SimulationPage.xaml
│   └── ...
├── ViewModels/                     # MVVM ViewModel layer
│   ├── MainViewModel.cs
│   ├── WeaveDesignViewModel.cs
│   ├── FabricPhysicsViewModel.cs
│   ├── SimulationViewModel*.cs     # Partial classes
│   └── ...
├── Models/                         # Data models & contracts
│   ├── FabricSimulationSpec.cs
│   ├── YarnSpecification.cs
│   ├── ModelBuilderPackage.cs
│   └── ...
├── Services/                       # Business logic & coordination
│   ├── FabricPhysicsSimulationService.cs
│   ├── ModelBuilderAiAssistCoordinator.cs
│   ├── AppProjectFileIo.cs
│   ├── AppSettingsService.cs
│   └── ...
├── Controls/                       # Custom XAML controls
│   ├── FabricPhysicsCanvas2D.cs
│   ├── FabricPhysicsCanvas3D.cs
│   ├── InfiniteWeaveCanvas.cs
│   └── ...
├── Converters/                     # XAML value converters
├── Helpers/                        # Utility functions
├── Core/                           # Core domain logic
├── Selectors/                      # DataTemplate selectors
└── Assets/                         # Images, icons, styles
```

### Related Projects

- **FabSim.Physics** — Physics simulation engine (separate project)
- **FabSim.Physics.Fea** — FEA solver components
- **Fabsim.AutomatedTests** — Test automation framework

## Getting Started for Developers

### Prerequisites

1. **Windows 10 (Build 17763+) or Windows 11**
2. **Visual Studio 2022** with:
   - .NET 8 SDK
   - Windows App SDK workload
   - C# development tools
3. **Git** — For version control

### Development Setup

```bash
# Clone repository
git clone https://github.com/yourusername/WinUi-FabSim.git
cd WinUi-FabSim

# Restore dependencies
dotnet restore

# Build solution (x64)
dotnet build -p:Platform=x64

# Run from Visual Studio
# Press F5 to debug with breakpoints
```

### First Build

```powershell
# PowerShell
cd "WinUi FabSim"
dotnet build -p:Platform=x64 -c Debug

# Visual Studio
# 1. Open WinUi FabSim.slnx
# 2. Set x64 as active platform
# 3. Press Ctrl+Shift+B to build
# 4. Press F5 to run
```

### Debugging

**Visual Studio Debugger:**
```
F5              — Start debugging
Shift+F5        — Stop debugging
Ctrl+Alt+B      — Breakpoints window
Ctrl+Alt+W      — Watch window
Ctrl+Alt+I      — Immediate window
```

**Console Output:**
- Debug.WriteLine() output appears in Output window
- Structured logging in Services/Diagnostics

## Architecture Patterns

### MVVM Architecture

```
View (XAML)
    ↓
ViewModel (ObservableObject)
    ↓
Services & Models
    ↓
Physics Engine / Persistence
```

**Key Components:**

1. **Views** — XAML UI pages and controls
   - Data bind to ViewModel
   - Handle user input
   - Minimal code-behind (prefer behaviors)

2. **ViewModels** — Observable business logic
   - Inherit from `ObservableObject` (MVVM Toolkit)
   - Expose properties with `OnPropertyChanged()`
   - Commands for user actions
   - Coordinate services

3. **Services** — Reusable business logic
   - Stateful or singleton
   - Dependency injection via DI container
   - Clear interfaces for testability

4. **Models** — Data contracts
   - Plain C# classes (POCOs)
   - JSON serializable
   - Immutable where possible

### Example: Adding a Feature

**1. Define Model:**
```csharp
public class CustomParam
{
    public string Name { get; set; }
    public double Value { get; set; }
}
```

**2. Create Service:**
```csharp
public interface ICustomService
{
    Task<CustomResult> ProcessAsync(CustomParam param);
}

public class CustomService : ICustomService
{
    public async Task<CustomResult> ProcessAsync(CustomParam param)
    {
        // Business logic here
    }
}
```

**3. Use in ViewModel:**
```csharp
public partial class MyViewModel : ObservableObject
{
    private readonly ICustomService _service;
    
    [ObservableProperty]
    private CustomParam? currentParam;
    
    public MyViewModel(ICustomService service)
    {
        _service = service;
    }
    
    [RelayCommand]
    private async Task ProcessAsync()
    {
        if (CurrentParam != null)
        {
            var result = await _service.ProcessAsync(CurrentParam);
            // Handle result
        }
    }
}
```

**4. Bind in View:**
```xaml
<Button Command="{x:Bind ViewModel.ProcessCommand}" 
        Content="Process"/>
```

### Service Coordination Pattern

**Multi-service workflows** use coordinator services:

```csharp
public class MyCoordinator
{
    private readonly IServiceA _serviceA;
    private readonly IServiceB _serviceB;
    
    public async Task ExecuteWorkflowAsync()
    {
        var stepOne = await _serviceA.GetDataAsync();
        var stepTwo = await _serviceB.ProcessAsync(stepOne);
        return stepTwo;
    }
}
```

### Physics Pipeline

```
User Input (Parameters)
    ↓
Simulation Service validates & prepares
    ↓
Physics Engine calculates (GPU-accelerated)
    ↓
Results cached
    ↓
ViewModel updates UI bindings
    ↓
Controls render visualization
```

## Development Workflows

### Feature Development

1. **Create branch:**
   ```bash
   git checkout -b feature/my-feature
   ```

2. **Implement feature:**
   - Create/modify models
   - Write service(s)
   - Add ViewModel binding
   - Design XAML view
   - Add test coverage

3. **Test locally:**
   ```bash
   # Run unit tests
   dotnet test Fabsim.AutomatedTests/Fabsim.AutomatedTests.csproj
   
   # Manual testing
   # F5 in Visual Studio
   ```

4. **Commit and push:**
   ```bash
   git add .
   git commit -m "feat: Add my new feature"
   git push origin feature/my-feature
   ```

5. **Create Pull Request**
   - Link related issues
   - Add description
   - Request reviewers

### Bug Fixing

1. **Reproduce and isolate:** Identify minimal reproduction case
2. **Create test:** Write failing unit test that reproduces bug
3. **Fix code:** Implement solution
4. **Verify test passes:** Confirm fix resolves test
5. **Check no regressions:** Run full test suite
6. **Create PR:** Link to bug report issue

### Refactoring

1. **Identify target:** Which component/service to refactor
2. **Ensure test coverage:** Write tests before refactoring
3. **Refactor incrementally:** One small change at a time
4. **Verify tests still pass:** After each change
5. **Commit frequently:** Create a new commit for each logical step
6. **Document changes:** Update comments/docs as needed

## Code Standards

### Naming Conventions

```csharp
// Classes
public class FabricPhysicsService { }
public interface ISimulationCache { }

// Properties
public string ProjectName { get; set; }

// Private fields
private readonly ILogger _logger;
private string _tempBuffer;

// Methods
public async Task<bool> ProcessAsync() { }
public void ExecuteOperation() { }

// Constants
private const int MaxRetries = 3;
public static readonly Color DefaultColor = Colors.Blue;

// Local variables
var fabricSpec = project.FabricSpecification;
```

### Code Organization

```csharp
public class MyService
{
    // Constants
    private const int MaxSize = 100;
    
    // Fields
    private readonly ILogger _logger;
    private string? _buffer;
    
    // Constructors
    public MyService(ILogger logger) { _logger = logger; }
    
    // Properties
    public string? Name { get; set; }
    
    // Public methods
    public async Task<Result> ExecuteAsync() { }
    
    // Private methods
    private void ValidateInput() { }
    
    // Nested types
    private record ResultData(string Value);
}
```

### XAML Conventions

```xaml
<!-- Namespace declarations at top -->
<Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WinUi_FabSim.Views">
    
    <!-- Resources -->
    <StackPanel>
        <!-- Nested elements with proper indentation -->
        <TextBlock Text="Label"/>
        <Button Command="{x:Bind ViewModel.ActionCommand}"/>
    </StackPanel>
</Window>
```

### Documentation

```csharp
/// <summary>
/// Simulates fabric physics with specified parameters.
/// </summary>
/// <param name="spec">Fabric specification defining geometry.</param>
/// <param name="options">Simulation options (solver type, tolerance).</param>
/// <param name="cancellationToken">Token for cancellation.</param>
/// <returns>Simulation results including stress/strain data.</returns>
/// <exception cref="ArgumentNullException">Thrown if spec is null.</exception>
public async Task<SimulationResult> RunSimulationAsync(
    FabricSimulationSpec spec,
    SimulationOptions options,
    CancellationToken cancellationToken = default)
{
    ArgumentNullException.ThrowIfNull(spec);
    // Implementation
}
```

## Testing Strategy

### Test Organization

```
Fabsim.AutomatedTests/
├── Unit/                    # Fast, isolated tests
│   ├── Services/
│   ├── Models/
│   └── Helpers/
├── Integration/             # Service interaction tests
├── Regression/              # Known bug prevention
├── Stress/                  # Performance & scale tests
├── UiAutomation/           # End-to-end UI tests
└── Snapshots/              # Visual regression tests
```

### Writing Tests

**Unit Test Example:**
```csharp
[TestFixture]
public class FabricPhysicsServiceTests
{
    private FabricPhysicsService _service;
    private IGeometryCalculator _calculator;
    
    [SetUp]
    public void Setup()
    {
        _calculator = A.Fake<IGeometryCalculator>();
        _service = new FabricPhysicsService(_calculator);
    }
    
    [Test]
    public async Task RunSimulationAsync_WithValidSpec_ReturnsResults()
    {
        // Arrange
        var spec = CreateValidFabricSpec();
        
        // Act
        var result = await _service.RunSimulationAsync(spec);
        
        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.IsConverged, Is.True);
    }
    
    private FabricSimulationSpec CreateValidFabricSpec()
    {
        return new FabricSimulationSpec { /* ... */ };
    }
}
```

### Running Tests

```bash
# Run all tests
dotnet test

# Run specific test project
dotnet test Fabsim.AutomatedTests/Fabsim.AutomatedTests.csproj

# Run with verbose output
dotnet test -v normal

# Run specific test class
dotnet test --filter "TestClass=MyServiceTests"
```

### Test Coverage Goals

- **Services:** ≥80% coverage
- **Models:** ≥70% coverage (simple data containers)
- **Controllers:** ≥75% coverage
- **Critical paths:** 100% coverage

---

## Performance Considerations

### Physics Simulation
- GPU acceleration via DirectX/SharpDX
- Multithreading for large calculations
- Caching intermediate results
- Streaming results to UI

### UI Rendering
- Virtual scrolling for large grids
- Cached control templates
- Async image loading
- DirectX viewport for 3D

### Memory Management
- Object pooling for frequently allocated types
- Disposing resources properly
- Monitoring large allocations
- Streaming file I/O for large projects

---

## Debugging Physics

**Enable physics debug visualization:**
```csharp
#if DEBUG
FabricPhysicsContext.DebugVisualization = true;
#endif
```

**Profile simulation performance:**
```csharp
var sw = Stopwatch.StartNew();
var result = await _service.RunSimulationAsync(spec);
sw.Stop();
Debug.WriteLine($"Simulation time: {sw.ElapsedMilliseconds}ms");
```

---

## Next Steps

1. **Clone repo** and complete development setup
2. **Review existing services** to understand patterns
3. **Write a small feature** to get familiar with workflow
4. **Run test suite** to verify environment
5. **Read Architecture Guide** for deep dive

See [Architecture Overview](04-ARCHITECTURE.md) for detailed system design.
