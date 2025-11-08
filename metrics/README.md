# Signal Transformation Analysis - Metrics

This folder contains Jupyter notebooks for analyzing signal transformations using different temporal alignment metrics. The notebooks evaluate how well various metrics can detect and reverse-engineer signal transformations.

## 📁 Notebooks Overview

### 1. `skew.ipynb` - Cross-Correlation (Skew) Analysis
**Purpose**: Analyzes signal transformations using cross-correlation to measure temporal skew (time shift) between signals.

**Key Features**:
- Measures temporal shift between signals using cross-correlation
- Reverse-engineers transformed signals by applying the detected skew
- Tests 8 different transformations (translations, compression, dilation, amplitude scaling, noise, combinations)
- Evaluates reconstruction quality using correlation and MSE metrics

**When to Use**:
- ✅ Pure temporal translations/synchronization
- ✅ Detecting delays or latencies
- ✅ Fast alignment of rigid signals
- ❌ Not suitable for non-linear temporal deformations (compression/dilation)
- ❌ Ignores amplitude changes
- ❌ Sensitive to noise

**Complexity**: O(N log N) - Fast and efficient

---

### 2. `dtw.ipynb` - Dynamic Time Warping Analysis
**Purpose**: Analyzes signal transformations using Dynamic Time Warping (DTW) to measure temporal alignment, handling non-linear time deformations.

**Key Features**:
- Measures temporal alignment using DTW distance
- Handles non-linear temporal deformations (compression/dilation)
- Computes optimal warping path between signals
- Reverse-engineers transformed signals using the DTW warping path
- Tests the same 8 transformations as the skew notebook
- Evaluates reconstruction quality with improved results for temporal deformations

**When to Use**:
- ✅ Temporal translations
- ✅ Non-linear temporal deformations (compression/dilation)
- ✅ Local adaptive alignment
- ✅ Robust to tempo changes
- ❌ Does not handle amplitude changes
- ❌ More computationally expensive
- ❌ Can overfit on noise

**Complexity**: O(N²) - Slower but more robust

---

## 🔧 Prerequisites

### Required Python Packages
```bash
pip install numpy matplotlib scipy pandas dtaidistance
```

### Key Dependencies
- `numpy`: Numerical computations
- `matplotlib`: Visualization
- `scipy`: Statistical functions (correlation, cross-correlation)
- `pandas`: Results table display
- `dtaidistance`: DTW implementation (for `dtw.ipynb` only)

---

## 📊 Signal Transformations Tested

Both notebooks test the following transformations:

1. **T1: Translation +50ms** - Positive time shift
2. **T2: Translation -30ms** - Negative time shift
3. **T3: Compression 0.8x** - Signal speed-up (temporal compression)
4. **T4: Dilation 1.2x** - Signal slow-down (temporal dilation)
5. **T5: Amplitude 1.5x** - Amplitude scaling up
6. **T6: Amplitude 0.6x** - Amplitude scaling down
7. **T7: Noise 0.2** - Gaussian noise addition
8. **T8: Translation +20ms + Amplitude 1.3x** - Combined transformation

---

## 🎯 Methodology

Both notebooks follow the same analysis workflow:

1. **Signal Generation**: Create a reference signal A (combination of sine waves)
2. **Transformation**: Apply transformation to create signal B
3. **Metric Calculation**: Measure transformation using the respective metric (skew or DTW)
4. **Reverse Engineering**: Generate signal C by applying the detected transformation
5. **Evaluation**: Compare signals B and C using:
   - Pearson correlation coefficient
   - Mean Squared Error (MSE)
6. **Visualization**: Plot signals A, B, and C for visual comparison
7. **Results Summary**: Generate a comprehensive results table

---

## 📈 Key Metrics Explained

### Skew Metrics (`skew.ipynb`)
- **Skew (ms)**: Temporal shift detected via cross-correlation
- **Corr A-B**: Global correlation between original and transformed signals
- **Corr B-C**: Correlation between transformed and reconstructed signals (reconstruction quality)
- **MSE B-C**: Mean squared error between transformed and reconstructed signals

### DTW Metrics (`dtw.ipynb`)
- **DTW Distance**: Normalized DTW distance (smaller = better alignment)
- **Warping Path Length**: Number of points in the optimal alignment path
- **Corr A-B**: Global correlation between original and transformed signals
- **Corr B-C**: Correlation between transformed and reconstructed signals
- **MSE B-C**: Mean squared error between transformed and reconstructed signals

---

## 🎓 Key Findings

### Skew (Cross-Correlation)
✅ **Strengths**:
- Perfect detection and reconstruction for pure translations
- Fast computation (O(N log N))
- Excellent for synchronization tasks

❌ **Limitations**:
- Fails on compression/dilation (non-linear deformations)
- Completely ignores amplitude changes
- Sensitive to noise
- Only detects global temporal shifts

### DTW (Dynamic Time Warping)
✅ **Strengths**:
- Handles both translations AND non-linear temporal deformations
- Successful reconstruction of compressed/dilated signals
- Local adaptive alignment
- Robust to tempo changes

❌ **Limitations**:
- More computationally expensive (O(N²))
- Does not handle amplitude changes
- Can overfit on noise
- Requires more memory

---

## 📋 Usage Instructions

### Running the Notebooks

1. **Install Dependencies**:
   ```bash
   pip install numpy matplotlib scipy pandas dtaidistance
   ```

2. **Open Jupyter Notebook**:
   ```bash
   jupyter notebook
   ```

3. **Run the Notebook**:
   - Open either `skew.ipynb` or `dtw.ipynb`
   - Execute cells sequentially (Cell → Run All)
   - Review results, visualizations, and interpretations

### Customizing the Analysis

Both notebooks can be customized by:

- **Modifying the reference signal** (Cell 1): Change frequency, duration, or signal composition
- **Adding new transformations** (Cell 5): Define custom transformation functions
- **Adjusting sampling rate** (Cell 1): Change `fs` to modify temporal resolution
- **Modifying noise levels**: Adjust noise parameters in transformation definitions

---

## 🔍 Comparison: Skew vs DTW

| Aspect | Skew (Cross-Correlation) | DTW (Dynamic Time Warping) |
|--------|-------------------------|----------------------------|
| **Temporal Translations** | ✅ Perfect | ✅ Perfect |
| **Compression/Dilation** | ❌ Fails | ✅ Handles well |
| **Amplitude Scaling** | ❌ Ignores | ❌ Ignores |
| **Noise** | ⚠️ Sensitive | ⚠️ Can overfit |
| **Computational Speed** | ✅ Fast (O(N log N)) | ❌ Slower (O(N²)) |
| **Best Use Case** | Simple synchronization | Complex temporal alignments |

---

## 💡 Recommendations

### When to Use Skew:
- Simple temporal synchronization tasks
- Real-time applications requiring fast computation
- Rigid signal alignment (no deformations)
- Detecting delays/latencies

### When to Use DTW:
- Non-linear temporal deformations (compression/dilation)
- Complex temporal alignments
- Offline analysis where computation time is not critical
- Signals with varying tempo

### For Complete Analysis:
- Combine DTW for temporal alignment
- Add separate amplitude analysis (RMS ratio, normalization)
- Apply noise filtering if needed
- Use spectral analysis for frequency-domain transformations

---

## 📝 Notes

- Both methods are **blind to amplitude changes** - they only handle temporal transformations
- For amplitude analysis, consider: RMS ratio, peak detection, normalization factors
- The notebooks use a fixed random seed (42) for reproducibility
- Signal A is a combination of two sine waves: `sin(2π·5t) + 0.5·sin(2π·10t)`
- Sampling frequency is set to 1000 Hz with 1-second duration

---

## 🤝 Contributing

When adding new metrics or transformations:
1. Follow the existing notebook structure
2. Include visualization for each transformation
3. Add results to the summary table
4. Update interpretations section with new findings
5. Document any new dependencies

---

## 📚 References

- **Cross-Correlation**: Standard signal processing technique for temporal alignment
- **DTW (Dynamic Time Warping)**: Algorithm for measuring similarity between time series with temporal deformations
- **dtaidistance**: Python library for DTW computation (https://dtaidistance.readthedocs.io/)

---

## 🐛 Troubleshooting

### Common Issues

1. **ImportError for dtaidistance**:
   ```bash
   pip install dtaidistance
   ```

2. **Memory issues with DTW**:
   - Reduce signal duration or sampling rate
   - Consider using fast DTW approximations

3. **Visualization not showing**:
   - Ensure matplotlib backend is configured correctly
   - Try `%matplotlib inline` in Jupyter

---

## 📧 Contact

For questions or issues related to these metrics, please refer to the main project documentation or contact the project maintainers.

