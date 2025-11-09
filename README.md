### Assignment: Estimating Unknown Parameters of a Parametric Curve

#### Problem Statement
Given the parametric equations:

x = t*cos(theta) - exp(M*|t|) * sin(0.3*t) * sin(theta) + X

y = 42 + t*sin(theta) + exp(M*|t|) * sin(0.3*t) * cos(theta)

Unknowns: theta, M, X
Ranges:
- 0 < theta < 50 degrees
- -0.05 < M < 0.05
- 0 < X < 100

Parameter 't' range: 6 < t < 60
Data provided: xy_data.csv

#### Steps Followed

1. **Import Libraries:**
   - `pandas` for reading CSV
   - `numpy` for numerical calculations
   - `scipy.optimize.minimize` for parameter optimization
   - `matplotlib.pyplot` for plotting

2. **Load Data:**
   - Read `x` and `y` values from CSV into arrays.

3. **Define Parameter `t`:**
   - Uniformly sample `t` in the range 6 to 60 with same number of points as data.

4. **Define Model Function:**
   ```python
   def model(params, t):
       theta, M, X = params
       x = t*np.cos(theta) - np.exp(M*np.abs(t))*np.sin(0.3*t)*np.sin(theta) + X
       y = 42 + t*np.sin(theta) + np.exp(M*np.abs(t))*np.sin(0.3*t)*np.cos(theta)
       return x, y
   ```

5. **Define Loss Function (L1 Distance):**
   ```python
   def loss(params, t, x_data, y_data):
       x_model, y_model = model(params, t)
       return np.sum(np.abs(x_data - x_model) + np.abs(y_data - y_model))
   ```

6. **Optimize Parameters:**
   ```python
   initial_guess = [np.deg2rad(25), 0.0, 50]
   bounds = [(0, np.deg2rad(50)), (-0.05, 0.05), (0, 100)]
   res = minimize(loss, initial_guess, args=(t, x_data, y_data), bounds=bounds)
   theta_opt, M_opt, X_opt = res.x
   theta_deg = np.rad2deg(theta_opt)
   ```

7. **Prediction & Plotting:**
   - Predicted curve is generated using optimized parameters.
   - Scatter plot for original points and line plot for predicted curve.

8. **Compute Average L1 Distance:**
   ```python
   l1_distance = np.sum(np.abs(x_data - x_fit) + np.abs(y_data - y_fit))
   avg_L1 = l1_distance / N
   print("Average L1 per point:", avg_L1)
   ```

9. **Output Optimized Parameters:**
   ```python
   print(f"Theta (deg): {theta_deg:.4f}")
   print(f"M: {M_opt:.5f}")
   print(f"X: {X_opt:.4f}")
   ```

#### Example Submission

Optimized values (example):
- Theta = 0.4367 rad (25 deg)
- M = 0.0123
- X = 11.5793

Parametric curve submission in LaTeX format:

\[
\left(t \cdot \cos(0.4367) - e^{0.0123 |t|} \cdot \sin(0.3 t) \sin(0.4367) + 11.5793, 
42 + t \cdot \sin(0.4367) + e^{0.0123 |t|} \cdot \sin(0.3 t) \cos(0.4367)\right)
\]

#### Notes
- This approach uses L1 distance minimization for parameter estimation.
- Visual inspection via plot ensures a good fit.
- Smaller L1 distance indicates higher accuracy.
- Method can be directly applied to other parametric curves with unknown parameters.

---

**End of README**
