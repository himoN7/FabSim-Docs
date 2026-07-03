# Contributing Guidelines

Guidelines for contributing to WinUI FabSim.

## Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Contribution Workflow](#contribution-workflow)
4. [Code Standards](#code-standards)
5. [Commit Guidelines](#commit-guidelines)
6. [Pull Request Process](#pull-request-process)
7. [Testing Requirements](#testing-requirements)

## Code of Conduct

### Our Commitment

We are committed to providing a welcoming and inspiring community for all. Please read and adhere to our Code of Conduct:

- Be respectful and inclusive
- Welcome diverse perspectives
- Assume good intent
- Focus on constructive feedback
- Report inappropriate behavior

---

## Getting Started

### Prerequisites

1. **Fork the repository** on GitHub
2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR_USERNAME/WinUi-FabSim.git
   cd WinUi-FabSim
   ```
3. **Add upstream remote:**
   ```bash
   git remote add upstream https://github.com/ORIGINAL_OWNER/WinUi-FabSim.git
   ```
4. **Complete development setup** — See [Installation & Setup](05-INSTALLATION-SETUP.md)

---

## Contribution Workflow

### 1. Create Feature Branch

```bash
# Ensure you're up to date
git fetch upstream
git checkout upstream/main
git checkout -b feature/my-feature

# Branch naming conventions:
# feature/description        — New feature
# bugfix/issue-name         — Bug fix
# docs/document-name        — Documentation
# refactor/component-name   — Refactoring
# test/test-name           — Test additions
```

### 2. Implement Changes

**Guidelines:**
- One feature per branch
- Keep commits focused and logical
- Write tests as you code
- Update documentation

### 3. Test Locally

```bash
# Run all tests
dotnet test

# Run specific test project
dotnet test Fabsim.AutomatedTests

# Manual testing
# F5 in Visual Studio to run and debug
```

### 4. Commit Changes

See [Commit Guidelines](#commit-guidelines) below.

```bash
git add .
git commit -m "feat: Add new simulation feature"
```

### 5. Push and Create PR

```bash
git push origin feature/my-feature
```

Then create Pull Request on GitHub.

---

## Code Standards

### Naming Conventions

```csharp
// Classes: PascalCase
public class FabricPhysicsService { }
public interface ISimulationCache { }

// Methods: PascalCase
public async Task ProcessAsync() { }

// Properties: PascalCase
public string ProjectName { get; set; }

// Private fields: _camelCase
private string _buffer;

// Constants: PascalCase or UPPER_SNAKE_CASE
private const int MaxRetries = 3;
public const double PI = 3.14159;

// Local variables: camelCase
var fabricSpec = project.FabricSpecification;
```

### File Organization

```csharp
public class MyService
{
    // 1. Constants
    private const int MaxSize = 100;
    
    // 2. Static fields
    private static int _instanceCount;
    
    // 3. Instance fields
    private readonly ILogger _logger;
    private string? _buffer;
    
    // 4. Constructors
    public MyService(ILogger logger) { }
    
    // 5. Properties
    public string? Name { get; set; }
    
    // 6. Public methods
    public async Task<Result> ExecuteAsync() { }
    
    // 7. Protected/internal methods
    protected void LogMessage(string msg) { }
    
    // 8. Private methods
    private void ValidateInput() { }
    
    // 9. Nested types
    private record ResultData(string Value);
}
```

### Documentation

**All public members must have XML documentation:**

```csharp
/// <summary>
/// Simulates fabric physics with specified parameters.
/// </summary>
/// <param name="spec">Fabric specification.</param>
/// <param name="options">Simulation configuration.</param>
/// <param name="cancellationToken">Cancellation token.</param>
/// <returns>Simulation results with geometry and properties.</returns>
/// <exception cref="ArgumentNullException">If spec is null.</exception>
public async Task<SimulationResult> RunSimulationAsync(
    FabricSimulationSpec spec,
    SimulationOptions options,
    CancellationToken cancellationToken = default)
{
    ArgumentNullException.ThrowIfNull(spec);
    // Implementation
}
```

### Code Style

**Prefer modern C# features:**

```csharp
// ✓ Good: Target-typed new
var service = new FabricPhysicsService();

// ✓ Good: Nullable reference types
public string? GetName() => _name;

// ✓ Good: Pattern matching
var result = obj switch
{
    IService svc => await svc.ExecuteAsync(),
    null => throw new ArgumentNullException(),
    _ => HandleDefault()
};

// ✓ Good: Records for data
public record FabricSpec(string Name, double Thickness);

// ✓ Good: LINQ
var items = data.Where(x => x.IsValid)
                 .OrderBy(x => x.Name)
                 .ToList();

// ✗ Avoid: Older patterns
var service = new FabricPhysicsService() as IService; // Use cast
#nullable disable  // Use nullable types throughout
```

### Error Handling

**Validate early:**

```csharp
public void ProcessFabric(FabricSpec spec)
{
    ArgumentNullException.ThrowIfNull(spec);
    
    if (spec.Thickness < 0)
        throw new ArgumentException("Thickness must be positive");
    
    // Process...
}
```

**Use specific exceptions:**

```csharp
// ✓ Good
throw new ArgumentOutOfRangeException(nameof(density), "Density must be between 0 and 1");

// ✗ Avoid
throw new Exception("Invalid density");
```

---

## Commit Guidelines

### Commit Message Format

```
<type>: <subject>

<body>

<footer>
```

**Types:**
- `feat` — New feature
- `fix` — Bug fix
- `docs` — Documentation changes
- `style` — Code style (formatting, semicolons, etc.)
- `refactor` — Code refactoring
- `perf` — Performance improvement
- `test` — Test additions
- `chore` — Build, dependencies, tooling

**Subject line:**
- Imperative mood ("add" not "added" or "adds")
- Don't capitalize first letter
- No period at end
- Max 50 characters

**Body:**
- Explain what and why, not how
- Wrap at 72 characters
- Separate from subject with blank line

**Footer:**
- Reference issues: `Fixes #123`
- Breaking changes: `BREAKING CHANGE: description`

### Examples

```bash
# Good commits
git commit -m "feat: Add 3D Olofsson yarn model

Implements the Olofsson yarn path model for advanced fabric geometry
calculation. Includes parameter presets and validation.

Fixes #234"

git commit -m "fix: Correct crimp factor calculation

The crimp factor was using incorrect formula. Updated to use
proper empirical model from literature.

Refs #456"

git commit -m "docs: Update physics pipeline documentation"

git commit -m "refactor: Extract geometry validation to service"

git commit -m "test: Add FabricPhysicsService integration tests"
```

---

## Pull Request Process

### Before Creating PR

1. **Update your branch:**
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Run tests:**
   ```bash
   dotnet test
   ```

3. **Check code style:**
   - Visual Studio: Analyze → Run Code Analysis
   - Command: `dotnet format --verify-no-changes`

4. **Build solution:**
   ```bash
   dotnet build -c Release
   ```

### Creating PR

**Fill in PR template:**

```markdown
## Description
Brief description of changes.

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Fixes #123

## Testing
How was this tested?

## Checklist
- [ ] Tests pass
- [ ] Documentation updated
- [ ] Code follows style guide
- [ ] No new warnings
```

### During Review

- Respond promptly to feedback
- Make requested changes in new commits (don't force push)
- Request re-review after changes
- Be respectful and professional

### Merging

- PR must have approval from maintainer(s)
- All CI checks must pass
- Branch must be up to date with main
- Use "Squash and merge" for clean history

---

## Testing Requirements

### Unit Tests

**Location:** `Fabsim.AutomatedTests/Unit/`

**Requirements:**
- Isolated test of single component
- No external dependencies (use mocks)
- Test one behavior per test
- Clear test names

**Example:**
```csharp
[TestFixture]
public class YarnSpecificationTests
{
    [Test]
    public void Constructor_WithValidParams_CreatesInstance()
    {
        // Arrange
        const double expectedDenier = 150;
        
        // Act
        var yarn = new YarnSpecification { Denier = expectedDenier };
        
        // Assert
        Assert.That(yarn.Denier, Is.EqualTo(expectedDenier));
    }
    
    [Test]
    public void Denier_WhenNegative_ThrowsException()
    {
        // Arrange
        var yarn = new YarnSpecification();
        
        // Act & Assert
        Assert.Throws<ArgumentException>(() => yarn.Denier = -1);
    }
}
```

### Integration Tests

**Location:** `Fabsim.AutomatedTests/Integration/`

**Requirements:**
- Test interaction between components
- Can use real services
- Should still be relatively fast

**Example:**
```csharp
[TestFixture]
public class FabricSimulationIntegrationTests
{
    [Test]
    public async Task RunSimulation_WithValidSpec_ReturnsResults()
    {
        // Arrange
        var service = new FabricPhysicsSimulationService();
        var spec = CreateValidSpec();
        
        // Act
        var result = await service.RunSimulationAsync(spec);
        
        // Assert
        Assert.That(result, Is.Not.Null);
        Assert.That(result.VertexPositions, Is.Not.Empty);
    }
}
```

### Test Coverage Goals

- **Services:** ≥80% coverage
- **Utilities:** ≥70% coverage
- **Critical paths:** 100% coverage
- **Overall:** ≥75% coverage

### Running Tests

```bash
# Run all tests
dotnet test

# Run with coverage
dotnet test /p:CollectCoverage=true

# Run specific category
dotnet test --filter "Category=Integration"

# Verbose output
dotnet test -v normal
```

---

## Documentation Guidelines

### Inline Documentation

- Use XML doc comments for public members
- Keep comments up-to-date with code
- Explain why, not what (code shows what)

### File Documentation

- Add file headers for complex files:
  ```csharp
  // <summary>
  // Handles fabric physics simulation calculations.
  // Supports multiple yarn path models and solver types.
  // </summary>
  namespace WinUi_FabSim.Services;
  ```

### Updating External Docs

- Update [User Guide](01-USER-GUIDE.md) for user-facing changes
- Update [Developer Guide](03-DEVELOPER-GUIDE.md) for code changes
- Update [Architecture](04-ARCHITECTURE.md) for structural changes

---

## Common Contribution Scenarios

### Adding a New Feature

1. Create feature branch: `git checkout -b feature/my-feature`
2. Add models in `Models/`
3. Create service interface and implementation
4. Add ViewModel binding if UI-related
5. Create XAML view or control
6. Write unit tests
7. Write integration tests
8. Update documentation
9. Create PR

### Fixing a Bug

1. Create branch: `git checkout -b bugfix/issue-name`
2. Write failing test that reproduces bug
3. Fix code until test passes
4. Run full test suite to ensure no regressions
5. Document the fix
6. Create PR with issue reference

### Refactoring

1. Ensure existing tests pass
2. Create new tests if coverage needed
3. Refactor in small, logical steps
4. Keep each commit focused
5. Verify tests still pass after each change
6. Document any API changes

---

## Support & Questions

- **GitHub Issues** — Bug reports and feature requests
- **Discussions** — General questions and ideas
- **Documentation** — Check docs first for common questions

---

Thank you for contributing to WinUI FabSim! 🧵
