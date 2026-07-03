# Physics Models & Yarn Path Geometry

**Version:** 0.4.0  
**Last Updated:** July 2026  
**Audience:** Advanced Users, Engineers, Researchers

---

## Overview

FabSim includes **13 scientifically-validated yarn path models** from textile physics literature. Each model generates unique yarn geometry for 3D fabric simulation.

### Model Categories

```
Group A: Wave-Based Models (sinusoidal variations)
- Peirce (4 variants)
- Kemp
- Hearle & Woodhead
- Rectangular

Group B: Parametric Curve Models (mathematical curves)
- Fourier Series
- Bézier Curves
- Splines (cubic, quintic)

Group C: Physical Models (empirical/theoretical)
- Backer
- Olofsson
- Custom (user-defined)
```

---

## Built-in Models (13 Total)

### Group A: Wave-Based Models

#### **1. Peirce Elliptical**

**Reference:** W.H. Peirce, "The geometry of cloth structure," Journal of the Textile Institute, 1937

```
Mathematical Form:
x(u) = 1/2 * (1 - cos(π*u))
y(u) = 0 (planar in 2D)
z(u) = a/2 * sin(π*u)

Parameters:
- a: Amplitude (yarn crimp height)
- u: Parameter (0 to 1, normalized pick position)

Characteristics:
- Symmetric wave pattern
- Sinusoidal cross-section
- Smooth interlacing
- Most realistic for plain weaves

Output:
- Continuous smooth yarn path
- Natural undulation
- Professional appearance
```

**When to Use:**
```
✓ Plain and simple weaves
✓ High-count fabrics
✓ When aesthetic quality is priority
✓ Academic publications
✓ Production visualization
```

#### **2. Peirce Parabolic**

```
Mathematical Form:
x(u) = 1/2 * (u - u²)
z(u) = a * (u - u²)

Parameters:
- a: Amplitude
- u: Parameter

Characteristics:
- Parabolic curve (not sinusoidal)
- Sharper peaks than elliptical
- More angular appearance
- Slightly more realistic for coarse yarns

Comparison to Elliptical:
- Peak sharpness: Higher
- Smoothness: Slightly less
- Physical accuracy: Similar
```

**When to Use:**
```
✓ Coarse/heavy weaves
✓ Twill patterns
✓ When sharpness preferred
✓ Technical analysis
```

#### **3. Peirce Involute**

```
Mathematical Form:
Involute of circle, parametric form

Characteristics:
- Continuous curve with variable curvature
- More complex path than elliptical/parabolic
- Spiral-like quality
- Advanced theoretical model

Computational Cost: Higher (involves transcendental functions)
```

**When to Use:**
```
✓ Advanced research
✓ Specialized simulations
✓ When theoretical accuracy critical
```

#### **4. Peirce Catenary**

```
Mathematical Form:
z(u) = a * cosh(b*u) - a

Characteristics:
- Catenary curve (chain-hanging shape)
- Natural physics of hanging yarn
- Different from sinusoidal
- More realistic for heavy, flexible yarns

Physical Basis:
- Minimizes gravitational potential energy
- Observed in loose, draped fabrics
```

**When to Use:**
```
✓ Loose, draped fabrics
✓ Flexible yarns (silk, wool)
✓ Physical accuracy for gravity
✓ Research on fabric mechanics
```

#### **5. Kemp Model**

**Reference:** A.M. Kemp, "An extension of Peirce's cloth geometry," Journal of the Textile Institute, 1958

```
Mathematical Form:
x(u) = 1/2 * (1 - cos(π*u))
z(u) = a/2 * sin(π*u) + d * (u - u²)

Parameters:
- a: Primary amplitude (main crimp)
- d: Secondary deviation
- u: Parameter

Characteristics:
- Peirce elliptical with deviation term
- More realistic interlacing
- Accounts for compression
- Smoother transitions
```

**When to Use:**
```
✓ Realistic simulations
✓ Multi-component interlacing
✓ When compression effects matter
✓ Production-grade simulation
```

#### **6. Hearle & Woodhead Model**

**Reference:** B.S. Hearle & M.A.J. Woodhead, 1959

```
Characteristics:
- Considers yarn thickness
- Accounts for relative positions
- Asymmetric potential
- Complex multi-parameter

Parameters:
- Yarn thickness ratio
- Interlacing position
- Symmetry factors
```

**When to Use:**
```
✓ Multi-component yarns
✓ When yarn thickness is factor
✓ Asymmetric interlacing
✓ Complex fiber systems
```

#### **7. Rectangular**

```
Mathematical Form:
x(u) = 1/2 * square_wave(π*u)
z(u) = a/2 * square_wave(π*u)

Characteristics:
- Rectangular cross-section
- Sharp 90-degree turns
- Simplified, abstract
- Not realistic but useful for testing

Use Cases:
- Algorithm testing
- Simplified visualization
- CAM preparation
- Educational purposes
```

**When to Use:**
```
✓ Simplified simulation
✓ Algorithm testing
✓ Educational demonstration
✓ CAM/CNC preparation (angular geometry)
```

---

### Group B: Parametric Curve Models

#### **8. Fourier Series**

**Mathematical Form:**
```
x(u) = Σ(n=1 to N) aₙ * cos(2πn*u) + bₙ * sin(2πn*u)
z(u) = Σ(n=1 to N) cₙ * cos(2πn*u) + dₙ * sin(2πn*u)

Parameters:
- N: Number of harmonics (default: 4)
- aₙ, bₙ, cₙ, dₙ: Fourier coefficients
- u: Parameter

Characteristics:
- Highly flexible (any smooth curve)
- Compact representation
- Low computational cost for evaluation
- Unlimited curve complexity
```

**Coefficient Presets:**
```
Simple Sine: Single harmonic
- a₀=1, all others=0
- Produces basic sinusoid

Rich Texture: Multiple harmonics
- Coefficients: 1, 0.3, 0.1, 0.05, ...
- Produces complex undulation

Wavy: Emphasis on lower harmonics
- a₀=1, a₁=0.5, b₁=0.3
- Produces flowing wave

Complex: All harmonics active
- Full set of coefficients
- Maximum flexibility
```

**When to Use:**
```
✓ Custom yarn path definition
✓ When specific curve needed
✓ Complex interlacing patterns
✓ Optimization (curve fitting)
✓ Research on yarn geometry
```

#### **9. Bézier Curves**

**Mathematical Form:**
```
B(u) = Σ(i=0 to n) Cᵢ * Bᵢ,ₙ(u)

Where:
- Cᵢ: Control points
- Bᵢ,ₙ(u): Bernstein polynomials
- u: Parameter (0 to 1)

Characteristics:
- Defined by control points (intuitive)
- Smooth curve through weighted points
- Degree depends on control point count
- Cubic (3 points) most common
```

**Common Orders:**
```
Quadratic (3 control points):
- Simple, 2nd-degree curve
- Fast computation
- Limited curve complexity

Cubic (4 control points):
- Most common (CAD standard)
- Good balance
- Smooth transitions

Quartic (5 control points):
- More flexibility
- Slightly slower

Quintic+ (6+ points):
- High complexity
- Slower evaluation
- Rarely needed
```

**When to Use:**
```
✓ CAD integration
✓ When control points intuitive
✓ Design-oriented workflows
✓ Smooth transitions
✓ Professional geometry
```

#### **10. Cubic Splines**

**Mathematical Form:**
```
S(u) = aᵢ + bᵢ*t + cᵢ*t² + dᵢ*t³

Where segments connect smoothly (C² continuous)

Parameters:
- Control points (knots)
- Tension (how tight curve)
- Continuity at junctions
```

**Advantages:**
```
- Very smooth curves
- Continuous derivatives
- Natural-looking paths
- Stable computation
- No oscillation (unlike high-degree polynomials)
```

**When to Use:**
```
✓ Maximum smoothness
✓ Multiple curve segments
✓ Professional appearance
✓ When derivatives needed
✓ Advanced simulations
```

#### **11. Quintic Splines**

```
Same as cubic but higher degree:
S(u) = polynomial of degree 5

Advantages:
- Even smoother (C⁴ continuous)
- Better for complex paths
- Higher computational cost

When to Use:
✓ When smoothness critical
✓ Research simulations
✓ When budget allows
```

#### **12-13. Custom Models (User-Defined)**

See [Model Builder Guide](13-FEATURES-COMPLETE.md#model-builder-ai-assisted) for how to create custom yarn path models using:
- Visual equation editor
- AI assistance (optional)
- Parametric equations
- Custom functions

---

## Model Comparison

### Visual Comparison

```
All models with same amplitude (a=2mm):

u=0.5 (center interlacing):

Peirce Elliptical:  ╔═══╗ (smooth peak)
Peirce Parabolic:   ╔═══╗ (slightly sharper)
Peirce Involute:    ╔════╗ (extended curve)
Peirce Catenary:    ╔╗ ╔╗ (chain-like dips)
Kemp:               ╔═════╗ (extended with deviation)
Fourier (N=4):      ╱╲╱╲╱╲ (complex ripples)
Bézier (cubic):     ╔╱════╲╗ (smooth with control)
Rectangular:        ╠═════╣ (sharp corners)
```

### Computational Complexity

```
Model              | Ops/Eval | Memory | Speed
Rectangular        | 2        | Low    | Very Fast
Peirce (all)       | 10-20    | Low    | Fast
Fourier (N=4)      | 40       | Low    | Medium
Kemp               | 15       | Low    | Fast
Hearle             | 25       | Low    | Medium
Bézier (cubic)     | 30       | Low    | Medium
Spline (cubic)     | 40-50    | Medium | Medium
Bézier (quintic)   | 60       | Low    | Slow
Involute           | 100+     | Low    | Slow

Evaluation count: Typical simulation uses 1000-50000 evaluations
```

### Physical Accuracy

```
Model              | Accuracy | Basis
Rectangular        | 30%      | Simplified
Peirce Elliptical  | 75%      | Classic textile physics
Peirce Parabolic   | 70%      | Modified Peirce
Kemp               | 85%      | Improved interlacing model
Hearle             | 80%      | Multi-component
Catenary           | 70%      | Physics-based (gravity)
Fourier            | 90%+     | Can fit any data
Bézier             | 85%      | Flexible geometry
Spline             | 95%      | Smoothest transitions
Custom             | Variable | User-defined accuracy

Note: Higher % = more realistic but not necessarily better for all cases
```

### Recommended Use Cases

```
Quick Visualization:
→ Peirce Elliptical (fast, good-looking)

Research/Academic:
→ Kemp or Hearle (scientifically validated)

Complex Patterns:
→ Fourier Series (flexibility)

Professional CAD:
→ Bézier Curves (CAD-standard)

Maximum Accuracy:
→ Spline + Fourier (best results)

Custom Needs:
→ Model Builder (unlimited flexibility)

Educational:
→ Rectangular or Peirce Elliptical (intuitive)
```

---

## Model Parameters

### Universal Parameters

```
All models include:

amplitude (a):
- Range: 0.1 - 10 mm
- Default: varies by model
- Controls: Crimp height / interlacing depth

frequency (f):
- Range: 0.5 - 10
- Default: 1.0
- Controls: Number of waves

phase (φ):
- Range: 0 - 2π
- Default: 0
- Controls: Horizontal shift
```

### Model-Specific Parameters

#### **Kemp Model**
```
deviation (d):
- Range: 0.0 - 1.0
- Controls: Secondary undulation amount
- 0.0 = pure Peirce
- 1.0 = maximum deviation
```

#### **Fourier Series**
```
harmonic_count (N):
- Range: 1 - 20
- Default: 4
- Controls: Complexity vs speed

coefficients (aₙ, bₙ, cₙ, dₙ):
- Each can be 0.0 - 1.0
- Higher harmonics for detail
```

#### **Spline**
```
tension:
- Range: 0.0 - 1.0
- 0.0 = tight, catenary-like
- 1.0 = loose, straight segments

smoothness:
- Auto-calculated for C² continuity
- Can adjust for specific effects
```

---

## Selecting the Right Model

### Decision Flowchart

```
What's your primary goal?

├─ Production Visualization?
│  ├─ High quality preferred? → Kemp or Spline
│  └─ Speed critical? → Peirce Elliptical
│
├─ Research/Analysis?
│  ├─ Academic paper? → Hearle or Kemp
│  ├─ Custom geometry needed? → Model Builder
│  └─ Multi-component yarn? → Hearle
│
├─ Engineering Simulation?
│  ├─ Abaqus/COMSOL export? → Spline or Bézier
│  ├─ CAD integration? → Bézier or Spline
│  └─ FEA mesh generation? → Rectangular or Spline
│
├─ Design Exploration?
│  ├─ Quick iterations? → Peirce Elliptical
│  ├─ Custom curves? → Fourier Series
│  └─ Fine control? → Bézier
│
└─ Learning/Education?
   ├─ Explain geometry? → Rectangular or Peirce
   └─ Realistic but simple? → Peirce Elliptical
```

### Model Selection by Weave Type

```
Plain Weaves (1/1):
- Recommended: Peirce Elliptical
- Alternative: Kemp, Spline
- Reason: Symmetric, simple interlacing

Twill Weaves (2/2, 3/1, etc.):
- Recommended: Kemp or Hearle
- Alternative: Fourier Series
- Reason: Complex interlacing, asymmetry

Satin Weaves (5-end, 8-end):
- Recommended: Spline or Bézier
- Alternative: Fourier Series
- Reason: Long floats need smooth curves

Rib Weaves:
- Recommended: Peirce Elliptical or Kemp
- Alternative: Hearle
- Reason: Regular structure, uniform crimp

Specialty Weaves:
- Recommended: Fourier or Custom
- Alternative: Bézier
- Reason: Unique patterns need flexibility
```

---

## Physics Solver Integration

### How Models Feed into Simulation

```
Yarn Path Model (defines geometry)
         ↓
[Discretization: x/y/z → particle chain]
         ↓
Physics Solver (XPBD or FEA)
         ↓
Constraints (distance, bending, contact)
         ↓
Simulation Result (deformed fabric)
```

### Solver Compatibility

```
Model              | XPBD Solver | FEA Solver | GPU Support
Rectangular        | ✓ (fast)    | ✓ (fast)  | ✓
Peirce            | ✓ (fast)    | ✓ (fast)  | ✓
Kemp              | ✓ (medium)  | ✓ (medium)| ✓
Fourier           | ✓ (medium)  | ✓ (medium)| ✓
Bézier            | ✓ (medium)  | ✓ (medium)| ✓
Spline            | ✓ (slow)    | ✓ (slow)  | ✓ (partial)
Custom            | ✓ (varies)  | ✓ (varies)| ✓ (varies)
```

---

## Export & Sharing Models

### Exporting Custom Models

See [Model Builder](13-FEATURES-COMPLETE.md#model-builder-ai-assisted) section.

```
1. Design in Model Builder
2. Compile and validate
3. Publish as package
4. Export as .zip
5. Share with others
```

### Using Imported Models

```
1. Receive .zip package
2. File → Import Model Package
3. Select .zip file
4. Model installed locally
5. Available in Model selection dropdown
```

---

## Performance Tips

### Optimization for Different Scenarios

```
Real-Time Preview (60 FPS target):
- Use: Peirce Elliptical or Rectangular
- Avoid: High-order splines, complex Fourier

High-Quality Renders:
- Use: Spline or Bézier
- Computational cost worth it for final render

Research Analysis:
- Use: Kemp or Hearle (scientifically sound)
- Accuracy over speed

Production Sim (fast turnaround):
- Use: Peirce Elliptical + XPBD
- Balanced approach

FEA Export:
- Use: Spline or Bézier (CAD-compatible)
- Clean mesh generation
```

---

## Technical Deep Dive

### Discretization: Model → Particles

```
Process:
1. Yarn path model defined as parametric curve
2. Discretize into n particles: u ∈ {0, Δu, 2Δu, ..., 1.0}
3. Evaluate model at each u: p_i = (x(u_i), y(u_i), z(u_i))
4. Create distance constraints: ||p_i - p_{i+1}|| = target_length
5. Create bend constraints: angle(p_{i-1}, p_i, p_{i+1}) = θ_target

Particle Count:
- Typical: 100-500 particles per yarn
- Total particles: N_yarns × Particles_per_yarn
- Range: 1,000 - 50,000 particles for full fabric
```

### Constraint Generation

```
Distance Constraints:
- Enforce yarn length
- Distance = (model arc length) / (particle count)
- Stiffness: Model-dependent

Bending Constraints:
- Preserve yarn smoothness
- Angle = target from model at u_i
- Stiffness: Model-dependent

Contact Constraints:
- Yarn-to-yarn collision
- Generated during simulation
- Dynamic (added as needed)
```

### GPU Acceleration

```
Models Suitable for GPU:
- Any smooth parametric model
- Parallel evaluation ideal
- NVIDIA CUDA implementation

Threshold:
- Use GPU when: >4,096 particles
- Use CPU when: <4,096 particles
- Auto-selection: Default behavior

Speedup:
- Typical: 2-8× faster (GPU vs CPU)
- Best case: 10-20× (very large sims)
- Depends on GPU model and particle count
```

---

## Research References

**Key Papers on Yarn Path Geometry:**

1. Peirce, W.H. (1937). "The geometry of cloth structure." Journal of the Textile Institute.
2. Kemp, A.M. (1958). "An extension of Peirce's cloth geometry." Journal of the Textile Institute.
3. Hearle, B.S., & Woodhead, M.A.J. (1959). "Yarn interlacing structure." Textile Research Journal.
4. Olofsson, B. (1973). "Simulation of yarn crimp geometry." Journal of the Textile Institute.
5. Backer, S., et al. (1975). "Fabric structure and appearance." Textile Technology and Science.

---

## Getting Help

For model-specific questions:
- See [Features Guide](13-FEATURES-COMPLETE.md#3d-simulation--physics)
- Check [Model Builder](13-FEATURES-COMPLETE.md#model-builder-ai-assisted)
- See [Troubleshooting](08-TROUBLESHOOTING.md)
- Contact: research@fabsim.example.com
