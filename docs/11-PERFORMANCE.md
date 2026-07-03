# Performance & Optimization

Guidelines for performance optimization, profiling, and best practices.

## Table of Contents

1. [Performance Targets](#performance-targets)
2. [Profiling Tools](#profiling-tools)
3. [Optimization Strategies](#optimization-strategies)
4. [Physics Optimization](#physics-optimization)
5. [UI Performance](#ui-performance)
6. [Memory Management](#memory-management)
7. [Benchmarking](#benchmarking)

## Performance Targets

### Application Startup

| Metric | Target | Current |
|--------|--------|---------|
| **Cold Start** | < 3s | 2.2s |
| **Warm Start** | < 1s | 0.8s |
| **First Paint** | < 500ms | 380ms |
| **Fully Interactive** | < 2s | 1.5s |

### Simulation Performance

| Task | Grid Size | Target Time |
|------|-----------|------------|
| **2D Simulation** | 32x32 | 2-5s |
| **2D Simulation** | 64x64 | 10-30s |
| **3D Simulation** | 32x32 | 5-15s |
| **3D Simulation** | 64x64 | 30s-2m |
| **Model Compilation** | Medium | < 5s |

### UI Responsiveness

| Operation | Target | Current |
|-----------|--------|---------|
| **Button Click Response** | < 100ms | 50ms |
| **Scroll/Pan** | 60 FPS | 58 FPS |
| **Viewport Rotation** | 60 FPS | 55 FPS |
| **Dialog Open** | < 200ms | 150ms |

---

## Profiling Tools

### Visual Studio Profiler

```
Debug → Performance Profiler or Alt+F2

1. Select "CPU Usage"
2. Run application
3. Perform operation
4. Stop recording
5. Analyze results
```

**Key Metrics:**
- CPU usage (%) — Should be <80% idle time
- Call tree — Functions consuming most CPU
- Hot paths — Most frequently called functions

### Memory Profiler

```
Debug → Performance Profiler

1. Select "Memory Usage"
2. Take baseline snapshot
3. Perform operation
4. Take another snapshot
5. Compare snapshots
```

**Checklist:**
- No unexpected allocations
- Objects properly disposed
- No memory leaks

### Custom Performance Tracking

```csharp
// Simple timing
var sw = Stopwatch.StartNew();
var result = await ExpensiveOperationAsync();
sw.Stop();
Debug.WriteLine($"Operation took {sw.ElapsedMilliseconds}ms");

// Detailed metrics
var metrics = new OperationMetrics
{
    StartTime = DateTime.UtcNow,
    MemoryBefore = GC.GetTotalMemory(true)
};

// Do work...

metrics.MemoryAfter = GC.GetTotalMemory(true);
metrics.Duration = DateTime.UtcNow - metrics.StartTime;

LogMetrics(metrics);
```

---

## Optimization Strategies

### 1. Async/Await Pattern

**Always use async for I/O operations:**

```csharp
// ✓ Good
public async Task<SimulationResult> RunSimulationAsync(FabricSimulationSpec spec)
{
    return await Task.Run(() => PerformSimulation(spec));
}

// ✗ Bad - Blocks UI thread
public SimulationResult RunSimulation(FabricSimulationSpec spec)
{
    return PerformSimulation(spec);  // Blocks if expensive
}
```

### 2. Caching

**Cache expensive computations:**

```csharp
public class ModelCache
{
    private readonly Dictionary<string, CachedModel> _cache = new();
    
    public IGeometryModel GetModel(string modelId)
    {
        if (_cache.TryGetValue(modelId, out var cached))
        {
            return cached.Model;
        }
        
        var model = LoadModel(modelId);
        _cache[modelId] = new CachedModel { Model = model };
        return model;
    }
    
    public void InvalidateCache(string? modelId = null)
    {
        if (modelId != null)
            _cache.Remove(modelId);
        else
            _cache.Clear();
    }
}
```

### 3. Lazy Initialization

**Defer expensive operations:**

```csharp
private Lazy<ExpensiveResource> _resource = new(() => 
    new ExpensiveResource());

public ExpensiveResource Resource => _resource.Value;
```

### 4. Object Pooling

**Reuse frequently allocated objects:**

```csharp
public class Vector3Pool
{
    private readonly Stack<Vector3> _pool = new();
    private const int InitialSize = 1000;
    
    public Vector3Pool()
    {
        for (int i = 0; i < InitialSize; i++)
            _pool.Push(new Vector3());
    }
    
    public Vector3 Get()
    {
        return _pool.Count > 0 ? _pool.Pop() : new Vector3();
    }
    
    public void Return(Vector3 vector)
    {
        vector.X = vector.Y = vector.Z = 0;
        _pool.Push(vector);
    }
}
```

### 5. LINQ Optimization

```csharp
// ✓ Good - Single pass
var largeValues = data.Where(x => x > 100).ToList();

// ✗ Bad - Multiple passes
var large = data.Where(x => x > 100);
var count = large.Count();  // Second pass
var first = large.FirstOrDefault();  // Third pass

// ✓ Better - Use iterator when possible
foreach (var item in data.Where(x => x > 100))
{
    // Process without materialization
}
```

---

## Physics Optimization

### GPU Acceleration

**Enable GPU-accelerated simulation:**

```csharp
var options = new SimulationOptions
{
    UseGpuAcceleration = true,
    ThreadCount = Environment.ProcessorCount
};

var result = await _service.RunSimulationAsync(spec, options);
```

**Requirements:**
- DirectX 11+ compatible GPU
- Current GPU drivers
- At least 2GB VRAM

### Solver Selection

| Solver | Speed | Accuracy | Use Case |
|--------|-------|----------|----------|
| **Particle System** | Fastest | Good | Most fabric types |
| **FEA** | Slower | Best | Precision needed |
| **Hybrid** | Balanced | Very Good | Complex geometries |

### Grid Resolution Impact

```
32x32:   Fastest, lowest accuracy
64x64:   Balanced
128x128: Slower, better accuracy
256x256: Slowest, highest detail

Recommendation: Use 64x64 for interactive design
```

### Parallel Processing

```csharp
// Use all cores
var options = new SimulationOptions
{
    ThreadCount = Environment.ProcessorCount,  // Auto-detect cores
    UseGpuAcceleration = true
};
```

---

## UI Performance

### Virtual Scrolling

For large lists, implement virtual scrolling:

```xaml
<ItemsRepeater ItemsSource="{x:Bind Items}">
    <ItemsRepeater.Layout>
        <UniformGridLayout ItemWidth="100" ItemHeight="100"/>
    </ItemsRepeater.Layout>
</ItemsRepeater>
```

### Image Optimization

```csharp
// Load images asynchronously
public async Task<BitmapImage> LoadImageAsync(string path)
{
    var bitmap = new BitmapImage();
    bitmap.SetSource(await File.OpenStreamForReadAsync(path));
    return bitmap;
}

// Limit image size
public void OptimizeImage(BitmapImage image)
{
    image.DecodePixelWidth = 800;  // Max width
    image.DecodePixelHeight = 600; // Max height
}
```

### Viewport Optimization

**3D Rendering:**
```csharp
// Reduce vertex count
var simplified = SimplifyMesh(fullGeometry, 0.5);  // 50% reduction

// Use LOD (Level of Detail)
if (zoomLevel < 0.5)
    RenderLodMesh(simplifiedGeometry);
else
    RenderFullMesh(fullGeometry);

// Frustum culling
if (!IsInViewFrustum(object3D))
    return;  // Skip rendering
```

### Rendering Optimization

```csharp
// Batch similar objects
var batch = new RenderBatch();
foreach (var yarn in yarns)
{
    batch.Add(yarn.Geometry);  // Group rendering
}
batch.Render();

// Use dirty flags
public class RenderTarget
{
    private bool _isDirty = true;
    
    public void Render()
    {
        if (!_isDirty) return;
        // Render logic
        _isDirty = false;
    }
    
    public void Invalidate() => _isDirty = true;
}
```

---

## Memory Management

### Allocation Patterns

**Minimize allocations:**

```csharp
// ✓ Good - Reuse arrays
var buffer = new double[1000];
for (int iteration = 0; iteration < 100; iteration++)
{
    ProcessData(buffer);  // Reuse
}

// ✗ Bad - Allocate each iteration
for (int iteration = 0; iteration < 100; iteration++)
{
    var buffer = new double[1000];  // Allocate each time
    ProcessData(buffer);
}
```

### Disposal Pattern

```csharp
// Implement proper disposal
public class SimulationContext : IDisposable
{
    private IntPtr _nativeHandle;
    private bool _disposed = false;
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                // Dispose managed resources
            }
            
            // Free unmanaged resources
            if (_nativeHandle != IntPtr.Zero)
            {
                Marshal.FreeHGlobal(_nativeHandle);
            }
            
            _disposed = true;
        }
    }
    
    ~SimulationContext()
    {
        Dispose(false);
    }
}
```

### Memory Profiling

```csharp
// Measure allocations
long before = GC.GetTotalMemory(true);

// Do work...

long after = GC.GetTotalMemory(true);
Debug.WriteLine($"Allocated: {(after - before) / 1024} KB");
```

---

## Benchmarking

### Setting Up Benchmarks

```csharp
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

[MemoryDiagnoser]
public class SimulationBenchmarks
{
    private FabricSimulationService _service;
    private FabricSimulationSpec _spec;
    
    [GlobalSetup]
    public void Setup()
    {
        _service = new FabricSimulationService();
        _spec = CreateTestSpec();
    }
    
    [Benchmark]
    public async Task<SimulationResult> Simulate32x32()
    {
        return await _service.RunSimulationAsync(_spec);
    }
    
    [Benchmark]
    public async Task<SimulationResult> Simulate64x64()
    {
        var largeSpec = CreateTestSpec(64);
        return await _service.RunSimulationAsync(largeSpec);
    }
}

// Run benchmarks
static void Main(string[] args)
{
    var summary = BenchmarkRunner.Run<SimulationBenchmarks>();
}
```

### Expected Results

```
|           Method |     Mean |   StdDev | Allocated |
|----------------- |---------:|---------:|----------:|
| Simulate32x32    | 2.345 s  | 0.087 s  |  45.2 MB  |
| Simulate64x64    | 12.543 s | 0.342 s  | 185.6 MB  |
```

---

## Performance Checklist

Before release:

- [ ] Startup time < 3 seconds (cold)
- [ ] UI responsive during heavy operations
- [ ] No memory leaks detected
- [ ] Peak memory usage < 1 GB
- [ ] GPU properly utilized (if available)
- [ ] 60 FPS maintained in viewport
- [ ] No unnecessary allocations in hot paths
- [ ] Async patterns used for blocking operations
- [ ] Caching implemented for expensive operations
- [ ] Profiler run shows no bottlenecks

---

**See Also:** [Testing & Quality](10-TESTING-QUALITY.md), [Architecture](04-ARCHITECTURE.md)
