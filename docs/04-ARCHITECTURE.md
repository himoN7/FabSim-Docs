# WinUI FabSim Architecture Overview

Comprehensive architecture design document covering system structure, component relationships, and data flow.

## Table of Contents

1. [System Overview](#system-overview)
2. [Architectural Patterns](#architectural-patterns)
3. [Component Architecture](#component-architecture)
4. [Data Flow](#data-flow)
5. [Physics Pipeline](#physics-pipeline)
6. [Project File Structure](#project-file-structure)
7. [Dependency Injection](#dependency-injection)
8. [Threading & Concurrency](#threading--concurrency)

## System Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────┐
│              WinUI Application Layer                 │
│  (Views, Controls, Converters, Selectors)           │
└──────────────┬──────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────┐
│         MVVM ViewModel Layer                         │
│  (MainViewModel, WeaveDesignViewModel, etc.)        │
└──────────────┬──────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────┐
│           Service Coordination Layer                 │
│  (Coordinators, Managers, Service Facades)          │
└──────────────┬──────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────┐
│         Business Logic Services Layer                │
│  (Simulation, FileIO, Import/Export, etc.)         │
└──────────────┬──────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────┐
│      Domain Models & Data Structures                 │
│  (FabricSimulationSpec, YarnSpec, etc.)            │
└──────────────┬──────────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────────┐
│     Physics Engine & Core Algorithms                 │
│  (FabSim.Physics, Particle Systems, FEA)           │
└─────────────────────────────────────────────────────┘
```

### Key Principles

1. **Separation of Concerns:** Clear boundaries between UI, logic, and data
2. **Dependency Injection:** Services injected, not created directly
3. **Reactive UI:** UI automatically updates when data changes
4. **Async/Await:** Non-blocking operations, responsive UI
5. **Testability:** Interfaces and dependency injection enable mocking
6. **Composability:** Services combine to create complex workflows

## Architectural Patterns

### 1. MVVM (Model-View-ViewModel)

**Purpose:** Separate UI from business logic

**Structure:**
```
View (XAML)
    ↓ (Binds to)
ViewModel (ObservableObject)
    ↓ (Uses)
Services & Models
```

**Benefits:**
- UI can be redesigned without code changes
- Logic can be tested without UI
- Multiple views can share same ViewModel
- Reactive updates via property change notification

### 2. Service-Oriented Architecture

**Purpose:** Encapsulate reusable business logic

**Characteristics:**
- Services implement specific interfaces
- Loosely coupled through interfaces
- Can be tested in isolation
- Composed into coordinators for complex workflows

**Example Services:**
```csharp
// File I/O
public interface IAppProjectFileIo { }

// Simulation
public interface IFabricPhysicsSimulationService { }

// Import/Export
public interface IFabricImagePaletteExtractor { }

// Project Management
public interface IAppProjectCoordinator { }
```

### 3. Coordinator Pattern

**Purpose:** Orchestrate multiple services for complex workflows

**Structure:**
```csharp
public class MyCoordinator
{
    private readonly IServiceA _serviceA;
    private readonly IServiceB _serviceB;
    private readonly IServiceC _serviceC;
    
    public async Task ComplexWorkflowAsync()
    {
        var step1 = await _serviceA.DoWorkAsync();
        var step2 = await _serviceB.ProcessAsync(step1);
        var step3 = await _serviceC.FinalizeAsync(step2);
        return step3;
    }
}
```

### 4. Repository Pattern

**Purpose:** Abstract data access

**Current Implementation:**
- File-based persistence (JSON)
- Abstracted through `IAppProjectStore`
- Could be replaced with database

### 5. Factory Pattern

**Purpose:** Create complex objects

**Examples:**
```csharp
public class GeometryModelEvaluatorFactory
{
    public IGeometryModelEvaluator Create(GeometryModelSource source)
    {
        return source.Kind switch
        {
            ModelKind.BuiltIn => new BuiltInGeometryModelEvaluator(),
            ModelKind.Package => new PackageGeometryModelEvaluator(),
            _ => throw new ArgumentException()
        };
    }
}
```

### 6. Strategy Pattern

**Purpose:** Encapsulate interchangeable algorithms

**Example: Yarn Path Models**
```csharp
public interface IYarnPathModel
{
    Vector3[] CalculatePath(YarnSpecification spec);
}

// Multiple implementations
public class PeirceYarnPathModel : IYarnPathModel { }
public class KempYarnPathModel : IYarnPathModel { }
public class FourierYarnPathModel : IYarnPathModel { }
// ... etc
```

## Component Architecture

### Core Components

#### 1. Views & Controls Layer

**Responsibility:** Present UI and handle user input

**Key Components:**
- `HomePage` — Project overview and quick start
- `WeaveDesignPage` — 2D design interface
- `FabricPhysicsPage` — Physics simulation UI
- `ModelBuilderPage` — Visual equation editor
- `SimulationPage` — Advanced 3D simulation
- Custom Controls: Canvas, Viewport, GridEditor

**Communication:**
- Binds to ViewModel properties
- Invokes ViewModel commands
- Handles navigation

#### 2. ViewModel Layer

**Responsibility:** Application state and user interaction logic

**Key ViewModels:**
```
MainViewModel
├── HomeViewModel
├── WeaveDesignViewModel
│   ├── WeaveDesignViewModel.Advanced
│   └── WeaveDesignViewModel.Project
├── FabricPhysicsViewModel
│   └── FabricPhysicsViewModel.Patterns
├── SimulationViewModel
│   ├── SimulationViewModel.Camera
│   ├── SimulationViewModel.Design
│   ├── SimulationViewModel.Export
│   ├── SimulationViewModel.Lifecycle
│   ├── SimulationViewModel.OutputBuild
│   ├── SimulationViewModel.Projects
│   ├── SimulationViewModel.PublishedPackages
│   ├── SimulationViewModel.RenderBindings
│   ├── SimulationViewModel.SpecBindings
│   ├── SimulationViewModel.ViewState
│   └── SimulationViewModel.WeaveImport
├── ModelBuilderPreviewViewModel
├── FabricAnalysisViewModel
└── SettingsViewModel
```

**Pattern:** Partial classes for organization
```csharp
// SimulationViewModel.cs - Main class
public partial class SimulationViewModel : ObservableObject { }

// SimulationViewModel.Camera.cs - Separate concern
public partial class SimulationViewModel
{
    // Camera-specific methods
}
```

#### 3. Service Coordination Layer

**Services** provide business logic and handle complex operations.

**Key Services:**

**Project Management:**
- `AppProjectCoordinator` — Project lifecycle
- `AppProjectFileIo` — File I/O
- `AppProjectStore` — Project storage/retrieval
- `AppProjectSession` — Session management
- `AppProjectRecoveryService` — Auto-save & recovery

**Simulation & Physics:**
- `FabricPhysicsSimulationService` — Physics calculations
- `FabricPhysicsSimulationWorker` — Background worker
- `FabricPhysicsContext` — Simulation context
- `FabricPhysicsValidationSuite` — Physics validation

**Geometry & Models:**
- `BuiltInGeometryModelEvaluator` — Built-in models
- `PackageGeometryModelEvaluator` — Custom models
- `GeometryModelEvaluatorFactory` — Model factory

**Model Builder (AI):**
- `ModelBuilderAiAssistCoordinator` — AI orchestration
- `ModelBuilderPackageCompiler` — Compilation
- `ModelBuilderParameterBinder` — Parameter binding
- `ModelBuilderExpressionEvaluator` — Expression evaluation

**Import/Export:**
- `FabricSimulationSpecSerializer` — JSON serialization
- `FabricImagePaletteExtractor` — Image color extraction
- Export services for various formats

**UI Infrastructure:**
- `AppSettingsService` — User preferences
- `HelixViewportCoordinator` — 3D viewport management
- `SystemBackdropCoordinator` — Mica effects

#### 4. Models & Data Structures

**Purpose:** Data contracts and domain entities

**Key Models:**

**Fabric Specification:**
```csharp
public class FabricSimulationSpec
{
    public WeavePatternData? WeavePattern { get; set; }
    public FabricGeometryParams? GeometryParams { get; set; }
    public YarnSpecification[]? Yarns { get; set; }
    public FiberMaterialProfile[]? Materials { get; set; }
}
```

**Yarn Definition:**
```csharp
public class YarnSpecification
{
    public string? Name { get; set; }
    public double Denier { get; set; }
    public double LinearDensity { get; set; }
    public YarnCrossSectionKind CrossSection { get; set; }
    public double FiberRoughness { get; set; }
}
```

**Geometry Model:**
```csharp
public class FabricGeometryModel
{
    public string? Id { get; set; }
    public GeometryModelSource Source { get; set; }
    public FabricGeometryParams? Params { get; set; }
}
```

**Project Document:**
```csharp
public class AppProjectDocument
{
    public string? ProjectId { get; set; }
    public AppProjectType Type { get; set; }
    public FabricSimulationSpec? FabricSpec { get; set; }
    public Dictionary<string, object>? Metadata { get; set; }
}
```

#### 5. Physics Engine

**Separate Project:** `FabSim.Physics`

**Components:**
- **Simulation Engines:**
  - `FabricSimulation2D` — 2D yarn simulation
  - `FabricSimulation3D` — 3D fabric simulation
  
- **Particle System:** Yarn modeled as particle sequences
- **Constraint Solvers:** Position and contact constraints
- **Material Models:** Fiber properties and behavior
- **Topology:** Yarn interlacing patterns
- **Metrics:** Property calculations

## Data Flow

### Typical User Workflow Data Flow

```
User Input (UI)
    ↓
ViewModel Command Handler
    ↓
Service Call(s)
    ↓
Model/Data Processing
    ↓
Physics Engine (if applicable)
    ↓
Results returned to Service
    ↓
ViewModel updates Properties
    ↓
UI automatically refreshes (data binding)
    ↓
User sees results
```

### Example: Running a Simulation

```
User clicks "Run Simulation" button
    ↓
SimulationViewModel.RunSimulationCommand executed
    ↓
Coordinator validates FabricSimulationSpec
    ↓
FabricPhysicsSimulationService.RunSimulationAsync(spec)
    ↓
Selects appropriate yarn path model (Peirce, Kemp, etc.)
    ↓
Physics engine calculates geometry (possibly GPU-accelerated)
    ↓
Results packaged into SimulationResult object
    ↓
ViewModel.SimulationResult = result
    ↓
UI bound to SimulationResult.Geometry, etc.
    ↓
Controls render updated visualization
    ↓
User sees 3D fabric in viewport
```

## Physics Pipeline

### Simulation Architecture

```
Input Specification
├── Fabric Geometry
│   ├── Weave pattern
│   ├── Thread density
│   └── Yarn properties
├── Material Properties
│   ├── Fiber type
│   ├── Denier
│   └── Density
└── Simulation Parameters
    ├── Solver type
    ├── Time step
    └── Convergence criteria
    ↓
Geometry Model Selection
├── Peirce geometric
├── Kemp semi-empirical
├── Fourier harmonic
├── Bézier interpolation
├── Backer wave-based
└── Olofsson parametric
    ↓
Yarn Path Calculation
├── Centerline geometry
├── Cross-section
└── Interlacing zones
    ↓
Constraint Setup
├── Position constraints
├── Contact constraints
└── Material constraints
    ↓
Solver Execution
├── Particle system
├── FEA (optional)
└── Iterative refinement
    ↓
Results Collection
├── Vertex positions
├── Stress/strain data
├── Property calculations
└── Visualization data
    ↓
Output Result
```

### Yarn Path Models

**Strategy Pattern Implementation:**

```csharp
public interface IYarnPathModel
{
    Vector3[] CalculatePath(
        WeaveTileData tile,
        YarnCrossSectionKind section,
        FabricGeometryParams @params);
}

// Peirce: Classical geometric approximation
public class PeirceYarnPathModel : IYarnPathModel { }

// Kemp: Empirical enhancement
public class KempYarnPathModel : IYarnPathModel { }

// Fourier: Harmonic decomposition
public class FourierYarnPathModel : IYarnPathModel { }

// ... etc
```

## Project File Structure

### Project Persistence

**Format:** JSON-based with metadata

```json
{
  "projectId": "proj-123",
  "projectType": "FabricSimulation",
  "version": "0.4.0",
  "metadata": {
    "name": "My Weave Design",
    "created": "2024-01-15T10:30:00Z",
    "lastModified": "2024-01-15T14:45:00Z",
    "thumbnail": "base64..."
  },
  "fabricSimulationSpec": {
    "weavePattern": { ... },
    "geometryParams": { ... },
    "yarns": [ ... ],
    "materials": [ ... ]
  }
}
```

**Storage:**
- **Default Location:** `%AppData%\WinUi FabSim\Projects\`
- **File Extension:** `.weave`
- **Backup Location:** `%AppData%\WinUi FabSim\Recovery\`

## Dependency Injection

### DI Container Setup

**Currently:** Manual service creation in coordinators

**Future:** Microsoft.Extensions.DependencyInjection

```csharp
// Current pattern (manual)
public class MyCoordinator
{
    private readonly IService1 _service1;
    private readonly IService2 _service2;
    
    public MyCoordinator()
    {
        _service1 = new Service1();
        _service2 = new Service2();
    }
}

// Future pattern (DI)
var services = new ServiceCollection();
services.AddSingleton<IService1, Service1>();
services.AddTransient<IService2, Service2>();
var provider = services.BuildServiceProvider();

var coordinator = provider.GetRequiredService<MyCoordinator>();
```

### Service Lifetimes

- **Singleton:** `AppSettingsService`, `AppProjectStore`
- **Transient:** Service instances for temporary work
- **Scoped:** (Not currently used, but available for DI)

## Threading & Concurrency

### Main Thread (UI Thread)

**Responsibilities:**
- XAML rendering
- Event handling
- UI updates via DispatcherQueue

**Access:** `DispatcherQueue.GetForCurrentThread()`

### Background Threads

**Physics Simulation:**
```csharp
// Run off UI thread
await Task.Run(async () => {
    var result = await _physicsEngine.SimulateAsync(spec);
    return result;
});

// Update UI on main thread
this.DispatcherQueue.TryEnqueue(() => {
    this.SimulationResult = result;
});
```

**Pattern:** Async/await with proper thread marshalling

### Synchronization

- **No explicit locks** (async/await manages coordination)
- **UI updates** marshalled to UI thread
- **Data** immutable or thread-safe by design

## Advanced Patterns

### Model Builder Pipeline

```
User Creates Equation
    ↓
ExpressionTokenizer → Tokens
    ↓
ExpressionEvaluator → AST
    ↓
Compiler → IL
    ↓
Runtime Execution
    ↓
Results → ViewModel
```

### AI Assistance Flow

```
User Request
    ↓
ModelBuilderAiPromptBuilder (construct prompt)
    ↓
IModelBuilderAiService (call AI)
    ↓
ModelBuilderAiResponseParser (parse response)
    ↓
ModelBuilderAiResponseValidator (validate)
    ↓
ModelBuilderAiSuggestionApplier (apply to UI)
```

### Recovery & Auto-save

```
Periodic Timer (configurable interval)
    ↓
AppProjectRecoveryService.AutoSaveAsync()
    ↓
Serialize current project state
    ↓
Write to recovery location
    ↓
Application crash
    ↓
Next launch: Detect recovery file
    ↓
Prompt user to recover
    ↓
Restore from recovery
```

---

## Design Patterns Summary

| Pattern | Usage | Example |
|---------|-------|---------|
| MVVM | UI architecture | ViewModel + DataBinding |
| Service-Oriented | Business logic | FabricPhysicsService |
| Coordinator | Complex workflows | AppProjectCoordinator |
| Repository | Data access | AppProjectStore |
| Factory | Object creation | GeometryModelEvaluatorFactory |
| Strategy | Algorithms | IYarnPathModel implementations |
| Observer | State changes | ObservableProperty |
| Command | User actions | RelayCommand |

---

**Next:** See [API Reference](06-API-REFERENCE.md) for service details.
