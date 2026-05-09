## MDS_project9_Edge_over_ignorance_Interpolation is not extrapolation
[FOR DETAILED PROCESS AND OBSERVATIONS PLEASE GO THROUGH THE FILE UPLOADED]
# Objective:
In this project, we investigate a classic but still dangerous machine-learning illusion:

A model can perform beautifully inside the region where it has seen data and fail quietly outside it.

This matters because in materials problems, descriptor space is rarely sampled uniformly. Some regions are dense, others sparse, and some are not represented at all. A model that interpolates well may still have no real knowledge beyond its training support.

# Methodology:
We used the following methods to identify where a model is interpolating and where its extrapolating
1) Gaussian Process regression: We train the GPR model on the data that we have, Along with every prediction, GPR provides a standard deviation to quantify uncertainity.We then measure the GPR uncertainity across the entire descriptor space.
   
2) Convex Hull method: By calculating the Convex Hull of a training data, we can try to determine if the new test point is interpolation or extrapolation. We also calculate RMSE inside the hull and outside the hull based on the GPR prediction that we have.
   
3) Kernel density estimation: While Convex Hull provides a binary classification(inside vs outside), KDE can help idenify holes in the descriptor space where data might be sparse even if technically inside the hull. We also calculate RMSE inside the low KDE regions and high KDE regions based on the GPR prediction that we have.
   
4) First, we train the GPR using all the training data that we have and get the following observations:
   <img width="547" height="470" alt="image" src="https://github.com/user-attachments/assets/be4c17c6-da84-4576-af9a-fe12eeb8c950" /> <img width="536" height="451" alt="image" src="https://github.com/user-attachments/assets/c683f65a-23cd-4343-9f72-ed7532cadc5c" />

OBSERVATION: GPR gives a smooth prediction, even at places where we dont have any training data.
Now,lets look at the uncertainity plot:<img width="524" height="451" alt="image" src="https://github.com/user-attachments/assets/776cbbcd-d2bb-4c40-8ff4-a26d6400302b" />
We notice that as we go away from the training data, the uncertainity increases.
For the next part of the analysis, we split the data into train,test and val.

Train RMSE: 0.12487604657692748

Validation RMSE: 0.17612668698413692

Test RMSE: 0.20668220359954478

Convex hull was next used to draw a boundary for interpolation and extrapolation:
<img width="553" height="470" alt="image" src="https://github.com/user-attachments/assets/a90f1f6d-c226-439b-865d-bde46a1af452" />
Here we notice that the hole was included as an interpolation region.
We used GPR to predict inside the interpolation region created by convex hull and outside the interpolation region. 

RMSE inside hull: 0.08790365454915132

RMSE outside hull: 0.8450222534207015

Mean uncertainty (inside hull): 0.06330362226021581

Mean uncertainty (outside hull): 0.5732574163786737

This shows that GPR is more uncertain outside the hull where we have no training points and the distance from the training points is large.

Next, KDE was used to determine how the uncertainity changes as the data distribution changes
<img width="553" height="470" alt="image" src="https://github.com/user-attachments/assets/adb345ec-6b6c-4702-981e-66b3e7994342" />
here low kde is defined as the area thats falls into bottom 40% of the observed densities

RMSE (low KDE): 1.071780878425074

RMSE (high KDE): 0.1810675964264716

Uncertainty (low KDE): 0.8204512673161607

Uncertainty (high KDE): 0.12580309908316217

# FINAL OBSERVATIONS
1) Analysis of Model Reliability and Data Geometry

The performance of the Gaussian Process Regression (GPR) model was evaluated not only through standard error metrics but also by analyzing the relationship between predictive accuracy and the spatial distribution of the training data. The following conclusions were drawn:

2) Data Density as a Determinant of Model Regime

Local data density serves as a primary indicator of whether the model is operating in an interpolation or extrapolation regime. In high-density regions, the model leverages proximal data points to anchor its predictions, whereas in sparse regions, it must rely on the underlying kernel's prior assumptions.

3) Uncertainty Quantification and Predictive Integrity

A significant advantage of GPR is its ability to quantify epistemic uncertainty. While the predictive surface may remain smooth across the entire domain, the model effectively identifies its own limitations by exhibiting high predictive variance (uncertainty) in areas devoid of training data. This ensures that the model's "honesty" remains intact even when its precision decreases.

4) Correlation between Density and Error Metrics

Analysis using Kernel Density Estimation (KDE) confirms a strong inverse correlation between data density and error. Regions identified with high data density consistently yielded a lower Root Mean Square Error (RMSE)

5) Limitations of Geometric Boundaries (Convex Hull)

While the Convex Hull is often used to define the boundary of the interpolation region, this study finds it insufficient for identifying "internal" extrapolation. If a region within the hull contains no data (a "hole"), the model's reliability there is equivalent to that of an extrapolation zone. Therefore, local density is a more robust metric for assessing model applicability than simple geometric boundaries.

6) Analysis of GPR RMSE
   
If we see the overall test error (RMSE)=0.2066, which seems quite less and we cannot tell just by looking at the test error that the model has extrapolated in some regions.To understand that the model has extrapolates we must look at the uncertainity quantification of GPR,geometry(convex hull) and density(KDE) of the space.







   
