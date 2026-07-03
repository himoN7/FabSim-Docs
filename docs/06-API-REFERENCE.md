# API Reference

Detailed reference for key services, models, and components.

## Table of Contents

1. [Service APIs](#service-apis)
2. [Model APIs](#model-apis)
3. [ViewModel APIs](#viewmodel-apis)
4. [Control APIs](#control-apis)
5. [Helper APIs](#helper-apis)

## Service APIs

### FabricPhysicsSimulationService

**Namespace:** `WinUi_FabSim.Services`

```csharp
public interface IFabricPhysicsSimulationService
{
    Task<SimulationResult> RunSimulationAsync(
        FabricSimulationSpec spec,
        SimulationOptions options,
        CancellationToken cancellationToken = default);
    
    void CancelRunningSimulation();
    
    bool IsSimulationRunning { get; }
}

public class FabricPhysicsSimulationService : IFabricPhysicsSimulationService
{
    // Implementation
}
```

**Usage Example:**
```csharp
var service = new FabricPhysicsSimulationService();
var spec = new FabricSimulationSpec { /* ... */ };
var result = await service.RunSimulationAsync(spec, new SimulationOptions());
```

**Key Methods:**
- `RunSimulationAsync()` — Execute fabric simulation
- `CancelRunningSimulation()` — Stop active simulation
- `IsSimulationRunning` — Check if simulation in progress

---

### AppProjectCoordinator

**Namespace:** `WinUi_FabSim.Services`

Orchestrates project lifecycle operations.

```csharp
public class AppProjectCoordinator
{
    public Task<AppProjectDocument?> CreateNewProjectAsync(
        AppProjectType type,
        CancellationToken cancellationToken = default);
    
    public Task<AppProjectDocument?> OpenProjectAsync(
        string filePath,
        CancellationToken cancellationToken = default);
    
    public Task<bool> SaveProjectAsync(
        AppProjectDocument project,
        CancellationToken cancellationToken = default);
    
    public Task<bool> DeleteProjectAsync(
        string projectId,
        CancellationToken cancellationToken = default);
    
    public IAsyncEnumerable<AppProjectSummary> GetRecentProjectsAsync(
        int limit = 10);
    
    public Task ExportProjectAsync(
        AppProjectDocument project,
        string outputPath,
        string format = "json");
}
```

**Usage Example:**
```csharp
var coordinator = new AppProjectCoordinator();
var project = await coordinator.CreateNewProjectAsync(AppProjectType.FabricSimulation);
await coordinator.SaveProjectAsync(project);
```

---

### ModelBuilderAiAssistCoordinator

**Namespace:** `WinUi_FabSim.Services`

Manages AI-assisted equation generation.

```csharp
public class ModelBuilderAiAssistCoordinator
{
    public Task<IEnumerable<string>> SuggestEquationsAsync(
        ModelBuilderContext context,
        string userPrompt);
    
    public Task<EquationExplanation> ExplainEquationAsync(
        string equation);
    
    public Task<ValidationResult> ValidateEquationAsync(
        string equation,
        IEnumerable<string> variables);
    
    public Task<ParameterRange> SuggestParameterRangeAsync(
        string paramName);
}
```

**Usage Example:**
```csharp
var coordinator = new ModelBuilderAiAssistCoordinator();
var suggestions = await coordinator.SuggestEquationsAsync(
    context,
    "Generate equation for yarn crimp factor");
```

---

### FabricImagePaletteExtractor

**Namespace:** `WinUi_FabSim.Services`

Extracts color palettes from images.

```csharp
public class FabricImagePaletteExtractor
{
    public async Task<ColorPalette> ExtractPaletteAsync(
        string imagePath,
        int colorCount = 5);
    
    public ColorPalette OptimizePalette(
        ColorPalette palette,
        double saturationBoost = 1.2);
}
```

**Usage Example:**
```csharp
var extractor = new FabricImagePaletteExtractor();
var palette = await extractor.ExtractPaletteAsync("fabric-image.jpg", 8);
```

---

### AppSettingsService

**Namespace:** `WinUi_FabSim.Services`

Application preferences and settings.

```csharp
public class AppSettingsService
{
    public static AppSettingsService Current { get; }
    
    // Properties
    public ApplicationTheme CurrentTheme { get; set; }
    public double UIFontSize { get; set; }
    public bool UseMicaBackdrop { get; set; }
    public TimeSpan AutosaveInterval { get; set; }
    public int PhysicsSolverType { get; set; }
    
    public void Load();
    public void Save();
    
    public event EventHandler? SettingsChanged;
}
```

**Usage Example:**
```csharp
var settings = AppSettingsService.Current;
settings.CurrentTheme = ApplicationTheme.Dark;
settings.AutosaveInterval = TimeSpan.FromMinutes(5);
settings.Save();
```

---

### FabricPhysicsValidationSuite

**Namespace:** `WinUi_FabSim.Services`

Validates fabric specifications and physics parameters.

```csharp
public class FabricPhysicsValidationSuite
{
    public ValidationResult ValidateSpec(FabricSimulationSpec spec);
    public ValidationResult ValidateYarnProperties(YarnSpecification yarn);
    public ValidationResult ValidateGeometryParams(FabricGeometryParams @params);
    public ValidationResult ValidateThreadDensity(double endsPerCm, double picksPerCm);
}
```

**Usage Example:**
```csharp
var validator = new FabricPhysicsValidationSuite();
var result = validator.ValidateSpec(fabricSpec);
if (!result.IsValid)
{
    foreach (var error in result.Errors)
        Console.WriteLine(error);
}
```

---

## Model APIs

### FabricSimulationSpec

**Namespace:** `WinUi_FabSim.Models`

Complete fabric specification for simulation.

```csharp
public class FabricSimulationSpec
{
    public WeavePatternData? WeavePattern { get; set; }
    public FabricGeometryParams? GeometryParams { get; set; }
    public YarnSpecification[]? Yarns { get; set; }
    public FiberMaterialProfile[]? Materials { get; set; }
    public SimulationOptions? Options { get; set; }
}
```

**Properties:**
- `WeavePattern` — 2D interlacing pattern definition
- `GeometryParams` — 3D geometry configuration
- `Yarns` — Yarn specifications (one per yarn system)
- `Materials` — Fiber material properties
- `Options` — Simulation solver options

---

### YarnSpecification

**Namespace:** `WinUi_FabSim.Models`

Defines yarn physical properties.

```csharp
public class YarnSpecification
{
    public string? Name { get; set; }
    public double Denier { get; set; }
    public double LinearDensity { get; set; }
    public YarnCrossSectionKind CrossSection { get; set; }
    public double FiberRoughness { get; set; }
    public Color ColorRgb { get; set; }
}

public enum YarnCrossSectionKind
{
    Round,
    Oval,
    Ribbon,
    Rectangular
}
```

**Example:**
```csharp
var yarn = new YarnSpecification
{
    Name = "Warp Yarn",
    Denier = 150,
    LinearDensity = 0.85,
    CrossSection = YarnCrossSectionKind.Round,
    FiberRoughness = 0.15
};
```

---

### FabricGeometryParams

**Namespace:** `WinUi_FabSim.Models`

Fabric geometry and deformation parameters.

```csharp
public class FabricGeometryParams
{
    public double EndsPerCm { get; set; }
    public double PicksPerCm { get; set; }
    public string? YarnPathModel { get; set; }
    public double CrimpFactor { get; set; }
    public double WarpYarnThickness { get; set; }
    public double WeftYarnThickness { get; set; }
}
```

**Yarn Path Models:**
- `"Peirce"` — Classical geometric
- `"Kemp"` — Semi-empirical
- `"Fourier"` — Harmonic
- `"Bézier"` — Smooth interpolation
- `"Backer"` — Wave-based
- `"Olofsson"` — Parametric

---

### AppProjectDocument

**Namespace:** `WinUi_FabSim.Models`

Project file structure.

```csharp
public class AppProjectDocument
{
    public string? ProjectId { get; set; }
    public AppProjectType Type { get; set; }
    public string? Name { get; set; }
    public DateTime Created { get; set; }
    public DateTime LastModified { get; set; }
    public FabricSimulationSpec? FabricSpec { get; set; }
    public Dictionary<string, object>? Metadata { get; set; }
}

public enum AppProjectType
{
    FabricSimulation,
    GeometryModel,
    ColorPalette,
    WeaveLbrary
}
```

---

### SimulationResult

**Namespace:** `WinUi_FabSim.Models`

Output from physics simulation.

```csharp
public class SimulationResult
{
    public Vector3[] VertexPositions { get; set; }
    public int[] TriangleIndices { get; set; }
    public double[] StressData { get; set; }
    public double[] StrainData { get; set; }
    public FabricProperties Properties { get; set; }
    public bool IsConverged { get; set; }
    public TimeSpan ComputationTime { get; set; }
}

public class FabricProperties
{
    public double Thickness { get; set; }
    public double Porosity { get; set; }
    public double CoverageFactor { get; set; }
    public double AirPermeability { get; set; }
}
```

---

## ViewModel APIs

### MainViewModel

**Namespace:** `WinUi_FabSim.ViewModels`

Application-level view model.

```csharp
public partial class MainViewModel : ObservableObject
{
    [ObservableProperty]
    private object? currentPage;
    
    [ObservableProperty]
    private bool isLoading;
    
    [RelayCommand]
    public async Task NavigateToPageAsync(string pageName);
    
    [RelayCommand]
    public async Task OpenProjectAsync(string filePath);
    
    [RelayCommand]
    public async Task SaveProjectAsync();
    
    [RelayCommand]
    public async Task ExportProjectAsync(string format);
}
```

---

### WeaveDesignViewModel

**Namespace:** `WinUi_FabSim.ViewModels`

Weave pattern editing.

```csharp
public partial class WeaveDesignViewModel : ObservableObject
{
    [ObservableProperty]
    private WeavePatternData? currentWeave;
    
    [ObservableProperty]
    private int gridWidth;
    
    [ObservableProperty]
    private int gridHeight;
    
    [RelayCommand]
    public void ToggleCell(int row, int col);
    
    [RelayCommand]
    public async Task LoadPatternAsync(string patternName);
    
    [RelayCommand]
    public async Task SavePatternAsync(string fileName);
}
```

---

### SimulationViewModel

**Namespace:** `WinUi_FabSim.ViewModels`

3D simulation control and visualization.

```csharp
public partial class SimulationViewModel : ObservableObject
{
    [ObservableProperty]
    private SimulationResult? simulationResult;
    
    [ObservableProperty]
    private bool isSimulationRunning;
    
    [RelayCommand]
    public async Task RunSimulationAsync();
    
    [RelayCommand]
    public void CancelSimulation();
    
    [RelayCommand]
    public async Task ExportGeometryAsync(string format);
    
    // Camera methods
    [RelayCommand]
    public void RotateCamera(double deltaX, double deltaY);
    
    [RelayCommand]
    public void ZoomCamera(double factor);
}
```

---

## Control APIs

### FabricPhysicsCanvas2D

**Namespace:** `WinUi_FabSim.Controls`

2D yarn visualization control.

```csharp
public class FabricPhysicsCanvas2D : Control
{
    public FabricSimulationSpec FabricSpec { get; set; }
    public SimulationResult? SimulationResult { get; set; }
    public double ZoomLevel { get; set; }
    
    public void Pan(double deltaX, double deltaY);
    public void ZoomTo(double level);
    public void ResetView();
    
    public event EventHandler<Point>? CellClicked;
    public event EventHandler? ViewChanged;
}
```

---

### FabricPhysicsCanvas3D

**Namespace:** `WinUi_FabSim.Controls`

3D fabric geometry visualization.

```csharp
public class FabricPhysicsCanvas3D : Control
{
    public SimulationResult? Geometry { get; set; }
    public Camera Camera { get; set; }
    public double[] StressVisualization { get; set; }
    public bool ShowWireframe { get; set; }
    public bool ShowStressMaps { get; set; }
    
    public void ResetCamera();
    public void FitToView();
    
    public event EventHandler<Point>? MousePressed;
    public event EventHandler<Point>? MouseMoved;
}
```

---

### InteractiveWeaveGrid

**Namespace:** `WinUi_FabSim.Controls`

Interactive weave pattern editor.

```csharp
public class InteractiveWeaveGrid : Control
{
    public WeavePatternData? Pattern { get; set; }
    public int CellSize { get; set; }
    public bool AllowEditing { get; set; }
    
    public void SelectCell(int row, int col);
    public void SelectRange(int startRow, int startCol, int endRow, int endCol);
    public void ClearSelection();
    
    public event EventHandler<WeaveGridCell>? CellChanged;
}
```

---

## Helper APIs

### ColorHelper

**Namespace:** `WinUi_FabSim.Helpers`

Color utilities and conversions.

```csharp
public static class ColorHelper
{
    public static Color HexToColor(string hexString);
    public static string ColorToHex(Color color);
    public static Color LerpColor(Color a, Color b, double t);
    public static double GetLuminance(Color color);
    public static Color GetContrastingColor(Color background);
}
```

**Example:**
```csharp
var color = ColorHelper.HexToColor("#FF5733");
var hex = ColorHelper.ColorToHex(color);
var contrast = ColorHelper.GetContrastingColor(color);
```

---

### GeometryHelper

**Namespace:** `WinUi_FabSim.Helpers`

3D geometry utilities.

```csharp
public static class GeometryHelper
{
    public static Vector3[] CalculateNormals(Vector3[] vertices, int[] indices);
    public static Bounds CalculateBounds(Vector3[] vertices);
    public static double CalculateDistance(Vector3 a, Vector3 b);
    public static Vector3 Interpolate(Vector3 a, Vector3 b, double t);
}
```

---

### SerializationHelper

**Namespace:** `WinUi_FabSim.Helpers`

JSON serialization utilities.

```csharp
public static class SerializationHelper
{
    public static string ToJson<T>(T obj, bool indent = false);
    public static T? FromJson<T>(string json);
    public static byte[] ToBytes<T>(T obj);
    public static T? FromBytes<T>(byte[] data);
}
```

---

## Extension Methods

### ObservableObjectExtensions

```csharp
public static class ObservableObjectExtensions
{
    public static void SetIfChanged<T>(
        this ObservableObject obj,
        ref T field,
        T value,
        string propertyName);
}
```

---

## Enums & Contracts

### FabricPhysicsSolverMode

```csharp
public enum FabricPhysicsSolverMode
{
    ParticleSystem,
    FiniteElementAnalysis,
    Hybrid
}
```

### SimulationOptions

```csharp
public class SimulationOptions
{
    public FabricPhysicsSolverMode SolverMode { get; set; }
    public double TimeStep { get; set; }
    public double ConvergenceTolerance { get; set; }
    public int MaxIterations { get; set; }
    public bool UseCuda { get; set; }
}
```

---

## Common Usage Patterns

### Running a Simulation

```csharp
// Create specification
var spec = new FabricSimulationSpec
{
    WeavePattern = CreatePlainWeave(8, 8),
    GeometryParams = new FabricGeometryParams
    {
        EndsPerCm = 20,
        PicksPerCm = 20,
        YarnPathModel = "Peirce"
    },
    Yarns = new[] { CreateCottonYarn() }
};

// Run simulation
var service = new FabricPhysicsSimulationService();
var result = await service.RunSimulationAsync(
    spec,
    new SimulationOptions { SolverMode = FabricPhysicsSolverMode.ParticleSystem }
);

// Use results
if (result.IsConverged)
{
    var geometry = (result.VertexPositions, result.TriangleIndices);
    var properties = result.Properties;
}
```

---

**See Also:** [Developer Guide](03-DEVELOPER-GUIDE.md), [Architecture](04-ARCHITECTURE.md)
