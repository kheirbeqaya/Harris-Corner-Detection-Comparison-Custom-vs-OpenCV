# Harris Corner Detection: Custom vs OpenCV

Comparison between a manually implemented Harris Corner Detector and OpenCV's built-in Harris function using Python and OpenCV.

---

##  Abstract

Corner detection is a fundamental problem in computer vision used for feature extraction, image matching, and scene understanding.  
This project presents a comparative study between a **manually implemented Harris Corner Detector** and the **OpenCV built-in implementation**.

The goal is to analyze:
- Detection consistency  
- Robustness to noise  
- Spatial agreement between detected keypoints  

Additionally, a matching procedure is applied to evaluate the correspondence between both methods.

---

##  Methodology

The pipeline consists of four main stages:

### 1. Image Preprocessing
- Convert input image to grayscale  
- Compute intensity gradients using Sobel filters  

### 2. Custom Harris Detector
- Compute image gradients (Ix, Iy)  
- Construct structure tensor:
  - Ix², Iy², Ix·Iy  
- Apply Gaussian smoothing  
- Compute Harris response function  
- Apply Non-Maximum Suppression (NMS)  

### 3. OpenCV Harris Detector
- Use `cv2.cornerHarris`  
- Apply thresholding  
- Apply local NMS for corner selection  

### 4. Feature Matching
- Scale keypoints to visualization space  
- Match spatially close corners  
- Draw correspondences between methods  

---

##  Mathematical Formulation

### 1. Image Gradients

Ix = dI/dx  
Iy = dI/dy  

---

### 2. Structure Tensor (Second-Moment Matrix)

After computing gradient products:

Ix² → intensity change in x direction  
Iy² → intensity change in y direction  
Ix · Iy → correlation between directions  

Apply Gaussian smoothing:

Sxx = G * (Ix²)  
Syy = G * (Iy²)  
Sxy = G * (Ix · Iy)  

Where G is a Gaussian filter.

Structure tensor:

S = [ Sxx   Sxy  
      Sxy   Syy ]

---

### 3. Harris Response Function

R = det(S) - k * (trace(S))²  
Where:
det(S) = Sxx * Syy - (Sxy)²  
trace(S) = Sxx + Syy  
k ≈ 0.04  
The parameter k in the Harris detector is an empirical constant that controls the sensitivity of the response function. It balances between edge and corner detection, and values around 0.04 provide a good trade-off between detecting strong corners and suppressing edges.



---

### 4. Interpretation

- If Ix ≈ 0 and Iy ≈ 0 → Flat region (no features)  
- If one is large → Edge  
- If both are large → Corner  

Harris detects corners by analyzing intensity variation in both directions.

---

##  Discussion

### 1. Detection Differences
The custom implementation is more sensitive to noise due to manual construction of the structure tensor.  
The OpenCV version is more stable due to optimized Gaussian filtering and internal optimizations.

### 2. Corner Distribution
Both methods detect similar high-intensity corner regions.  
OpenCV produces slightly more consistent and dense detections.

### 3. Matching Analysis
Spatial matching shows a high overlap between both methods.  
Small differences arise due to smoothing and thresholding variations.

### 4. Limitations
- Manual NMS is computationally expensive  
- Matching is based only on spatial proximity  

---

##  Results

### Input Image
![Input](results/Harris_img.png)

---

### Detection Comparison
![Detection](results/detected_points_manual_and_cv_harris.png)

---

### Matching Visualization
![Matching](results/matched_points.png)

---

##  Conclusion

Both implementations demonstrate the effectiveness of the Harris corner detector.

- Custom implementation → educational and interpretable  
- OpenCV implementation → optimized and robust  

---

##  Author

**Aya Kheir Beq**  
MSc in Mechatronics Engineering  
Focus: Computer Vision, Robotics, Intelligent Systems
