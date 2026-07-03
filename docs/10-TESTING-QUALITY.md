# Testing & Quality Assurance

Complete guide to testing infrastructure, test organization, and quality processes.

## Table of Contents

1. [Test Overview](#test-overview)
2. [Test Organization](#test-organization)
3. [Unit Testing](#unit-testing)
4. [Integration Testing](#integration-testing)
5. [Automation Testing](#automation-testing)
6. [Test Execution](#test-execution)
7. [Coverage & Metrics](#coverage--metrics)
8. [Best Practices](#best-practices)

## Test Overview

### Testing Pyramid

```
        /\           E2E / UI Tests (5-10%)
       /  \         Slow, Brittle, High value
      /    \
     /------\      Integration Tests (15-20%)
    /        \    Medium speed, Cross-component
   /          \
  /            \   Unit Tests (70-85%)
 /              \  Fast, Isolated, Immediate feedback
/______________\

Lower level = more tests
Higher level = fewer, higher-value tests
```

### Test Statistics

- **Total Tests:** 500+
- **Unit Tests:** ~400
- **Integration Tests:** ~80
- **Automation Tests:** ~20
- **Average Run Time:** ~2 minutes (full suite)

---

## Test Organization

### Directory Structure

```
Fabsim.AutomatedTests/
├── Unit/
│   ├── Services/
│   │   ├── FabricPhysicsServiceTests.cs
│   │   ├── AppProjectCoordinatorTests.cs
│   │   └── ...
│   ├── Models/
│   │   ├── FabricSimulationSpecTests.cs
│   │   ├── YarnSpecificationTests.cs
│   │   └── ...
│   └── Helpers/
│       ├── ColorHelperTests.cs
│       └── ...
├── Integration/
│   ├── SimulationPipelineTests.cs
│   ├── FileIoPersistenceTests.cs
│   └── ...
├── Regression/
│   ├── KnownBugTests.cs
│   └── PreviousIssueTests.cs
├── Stress/
│   ├── LargeSimulationTests.cs
│   └── PerformanceTests.cs
├── UiAutomation/
│   ├── UserWorkflowTests.cs
│   └── ...
├── Mocks/
│   ├── FakeGeometryCalculator.cs
│   └── ...
├── Builders/
│   ├── FabricSpecBuilder.cs
│   └── ...
├── Utilities/
│   ├── TestDataGenerator.cs
│   └── ...
└── Snapshots/
    ├── expected_*.json
    └── ...
```

---

## Unit Testing

### Purpose & Characteristics

- **Goal:** Test single component in isolation
- **Speed:** < 100ms per test
- **Dependencies:** Mocked (use FakeItEasy)
- **Scope:** Public methods only

### Unit Test Structure

```csharp
[TestFixture]
public class FabricPhysicsServiceTests
{
    private FabricPhysicsService _service;
    private IGeometryCalculator _calculatorFake;
    
    [SetUp]
    public void Setup()
    {
        _calculatorFake = A.Fake<IGeometryCalculator>();
        _service = new FabricPhysicsService(_calculatorFake);
    }
    
    [TearDown]
    public void Cleanup()
    {
        // Cleanup resources if needed
    }
    
    [Test]
    public async Task RunSimulationAsync_WithValidSpec_ReturnsConvergedResult()
    {
        // Arrange
        var spec = CreateValidFabricSpec();
        A.CallTo(() => _calculatorFake.Calculate(A<double>.Ignored))
            .Returns(1.0);
        
        // Act
        var result = await _service.RunSimulationAsync(spec);
        
        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.IsConverged, Is.True);
        A.CallTo(() => _calculatorFake.Calculate(A<double>.Ignored))
            .MustHaveHappenedOnceExactly();
    }
    
    [Test]
    [TestCase(null)]
    [TestCase("")]
    public void ValidateSpec_WithInvalidInput_ThrowsArgumentException(string? invalid)
    {
        // Act & Assert
        Assert.Throws<ArgumentException>(() => _service.ValidateSpec(invalid));
    }
    
    [Test]
    [TestCaseSource(nameof(GetInvalidSpecCases))]
    public void ValidateSpec_WithInvalidCases_ReturnsFalse(FabricSimulationSpec? spec)
    {
        // Act
        var result = _service.ValidateSpec(spec);
        
        // Assert
        Assert.That(result.IsValid, Is.False);
    }
    
    private static IEnumerable<TestCaseData> GetInvalidSpecCases()
    {
        yield return new TestCaseData(null).SetName("NullSpec");
        yield return new TestCaseData(new FabricSimulationSpec()).SetName("EmptySpec");
    }
    
    private FabricSimulationSpec CreateValidFabricSpec()
    {
        return new FabricSimulationSpec
        {
            WeavePattern = new WeavePatternData { Width = 8, Height = 8 },
            Yarns = new[] { CreateTestYarn() }
        };
    }
    
    private YarnSpecification CreateTestYarn()
    {
        return new YarnSpecification
        {
            Denier = 150,
            LinearDensity = 0.85
        };
    }
}
```

### Mocking with FakeItEasy

```csharp
// Create fake
var calculatorFake = A.Fake<IGeometryCalculator>();

// Configure behavior
A.CallTo(() => calculatorFake.Calculate(A<double>.Ignored))
    .Returns(1.0);

// Verify calls
A.CallTo(() => calculatorFake.Calculate(0.5))
    .MustHaveHappenedOnceExactly();

// Test exceptions
A.CallTo(() => calculatorFake.Calculate(A<double>.That.IsLessThan(0)))
    .Throws<ArgumentException>();
```

### Common Unit Test Patterns

```csharp
// Test success path
[Test]
public void Method_WithValidInput_ReturnsExpectedResult()
{
    var result = _service.Process(validInput);
    Assert.That(result, Is.EqualTo(expected));
}

// Test error handling
[Test]
public void Method_WithInvalidInput_ThrowsException()
{
    Assert.Throws<ArgumentException>(() => _service.Process(invalidInput));
}

// Test state changes
[Test]
public void Method_ModifiesState_AsExpected()
{
    _service.Initialize();
    _service.Execute();
    Assert.That(_service.IsExecuted, Is.True);
}

// Test async methods
[Test]
public async Task AsyncMethod_WithValidInput_CompletesSuccessfully()
{
    var result = await _service.ProcessAsync(input);
    Assert.That(result, Is.Not.Null);
}

// Test parameter validation
[Test]
[TestCase(null)]
[TestCase("")]
public void Method_WithNullOrEmpty_ThrowsException(string? param)
{
    Assert.Throws<ArgumentException>(() => _service.Process(param));
}
```

---

## Integration Testing

### Purpose & Characteristics

- **Goal:** Test interaction between multiple components
- **Speed:** 100ms - 5 seconds per test
- **Dependencies:** Real services (or realistic fakes)
- **Scope:** End-to-end component workflows

### Integration Test Example

```csharp
[TestFixture]
public class FabricSimulationIntegrationTests
{
    private FabricPhysicsService _physicsService;
    private AppProjectCoordinator _coordinator;
    private FabricSimulationSpecSerializer _serializer;
    
    [SetUp]
    public void Setup()
    {
        _physicsService = new FabricPhysicsService();
        _coordinator = new AppProjectCoordinator();
        _serializer = new FabricSimulationSpecSerializer();
    }
    
    [Test]
    public async Task CompleteSimulationWorkflow_Succeeds()
    {
        // Arrange
        var spec = CreateValidSpec();
        var project = new AppProjectDocument { FabricSpec = spec };
        
        // Act - Save project
        await _coordinator.SaveProjectAsync(project);
        
        // Act - Load project
        var loadedProject = await _coordinator.OpenProjectAsync(project.ProjectId);
        
        // Act - Run simulation
        var result = await _physicsService.RunSimulationAsync(loadedProject.FabricSpec);
        
        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.VertexPositions, Is.Not.Empty);
        Assert.That(result.IsConverged, Is.True);
    }
    
    [Test]
    public async Task SerializeDeserialize_PreservesData()
    {
        // Arrange
        var spec = CreateValidSpec();
        
        // Act
        var json = _serializer.Serialize(spec);
        var deserializedSpec = _serializer.Deserialize(json);
        
        // Assert
        Assert.That(deserializedSpec, Is.EqualTo(spec));
    }
}
```

---

## Automation Testing

### UI Automation Tests

**Location:** `UiAutomation/`

```csharp
[TestFixture]
public class UserWorkflowTests
{
    private AppWindow _mainWindow;
    
    [SetUp]
    public void Setup()
    {
        // Launch application
        _mainWindow = LaunchApplication();
    }
    
    [Test]
    public void CreateNewProject_UserCanCompleteWorkflow()
    {
        // Navigate to Weave Design
        _mainWindow.ClickButton("WeaveDesignNav");
        Assert.That(_mainWindow.CurrentPage, Is.EqualTo("WeaveDesign"));
        
        // Create new weave
        _mainWindow.ClickButton("NewWeaveBtnNon");
        _mainWindow.SelectComboboxItem("PatternDropdown", "Plain Weave");
        
        // Set parameters
        _mainWindow.SetTextBoxValue("ThreadCountInput", "20");
        
        // Run simulation
        _mainWindow.ClickButton("RunSimulationBtn");
        
        // Verify results appear
        var viewportReady = _mainWindow.WaitForElement("SimulationViewport", 30);
        Assert.That(viewportReady, Is.True);
    }
}
```

---

## Test Execution

### Running Tests

```bash
# Run all tests
dotnet test

# Run specific test class
dotnet test --filter "ClassName=FabricPhysicsServiceTests"

# Run specific test method
dotnet test --filter "FullyQualifiedName~FabricPhysicsServiceTests.RunSimulationAsync"

# Run by category
dotnet test --filter "Category=Integration"

# Verbose output
dotnet test -v normal

# With diagnostics
dotnet test -v diagnostic
```

### CI/CD Test Execution

**GitHub Actions Workflow:**

```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: windows-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Setup .NET 8
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '8.0.x'
    
    - name: Restore
      run: dotnet restore
    
    - name: Build
      run: dotnet build -c Release
    
    - name: Unit Tests
      run: dotnet test --filter "Category=Unit" -c Release
    
    - name: Integration Tests
      run: dotnet test --filter "Category=Integration" -c Release
    
    - name: Coverage Report
      run: dotnet test /p:CollectCoverage=true
```

---

## Coverage & Metrics

### Code Coverage

**Target Coverage:**
- Overall: ≥75%
- Services: ≥80%
- Critical paths: 100%
- UI code: ≥50%

**Collect Coverage:**

```bash
dotnet test \
  /p:CollectCoverage=true \
  /p:CoverageFormat=opencover \
  /p:Exclude="[*.Tests]*"
```

**Generate Report:**

```bash
reportgenerator \
  -reports:"coverage.opencover.xml" \
  -targetdir:"coveragereport" \
  -reporttypes:"HtmlSummary"
```

### Metrics Tracked

| Metric | Target | Current |
|--------|--------|---------|
| **Code Coverage** | ≥75% | 82% |
| **Test Pass Rate** | 100% | 99.2% |
| **Avg Test Time** | <2s | 1.8s |
| **Regression Tests** | N/A | 45 |

---

## Best Practices

### DO ✓

- ✓ Name tests descriptively: `Method_Scenario_ExpectedResult`
- ✓ One assertion per test (or related assertions)
- ✓ Test behavior, not implementation
- ✓ Use builders for complex test data
- ✓ Mock external dependencies
- ✓ Test edge cases and error conditions
- ✓ Keep tests fast and independent
- ✓ Use TestCase/TestCaseSource for parametric tests

### DON'T ✗

- ✗ Test private methods directly
- ✗ Have tests depend on each other
- ✗ Use Thread.Sleep() for timing
- ✗ Create files/database records unnecessarily
- ✗ Test multiple behaviors in one test
- ✗ Leave tests in "pending" state
- ✗ Mix unit and integration tests

### Test Data Builders

```csharp
public class FabricSpecBuilder
{
    private FabricSimulationSpec _spec = new();
    
    public FabricSpecBuilder WithPlainWeave()
    {
        _spec.WeavePattern = new WeavePatternData { /* */ };
        return this;
    }
    
    public FabricSpecBuilder WithCottonYarn()
    {
        _spec.Yarns = new[] { CreateCottonYarn() };
        return this;
    }
    
    public FabricSimulationSpec Build() => _spec;
}

// Usage
var spec = new FabricSpecBuilder()
    .WithPlainWeave()
    .WithCottonYarn()
    .Build();
```

---

## Test Categories

### By Type

- **Unit:** Isolated component tests
- **Integration:** Multi-component workflows
- **Regression:** Known bug prevention
- **Stress:** Performance under load
- **UiAutomation:** End-to-end user workflows

### By Attribute

```csharp
[TestFixture]
[Category("Unit")]
public class QuickTests { }

[TestFixture]
[Category("Integration")]
public class ComplexTests { }

[TestFixture]
[Category("Regression")]
public class BugFixTests { }

[TestFixture]
[Category("Stress")]
[Timeout(60000)]  // 60 seconds
public class PerformanceTests { }
```

---

## Quality Gates

**Before PR merge:**
- [ ] All tests pass
- [ ] ≥75% code coverage
- [ ] No new warnings
- [ ] Builds successfully

**Before release:**
- [ ] All automation tests pass
- [ ] ≥80% code coverage
- [ ] Zero critical bugs
- [ ] Performance validated

---

**See Also:** [Developer Guide](03-DEVELOPER-GUIDE.md), [Contributing](07-CONTRIBUTING.md)
