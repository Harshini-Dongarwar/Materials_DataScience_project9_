## Materials_Data_Science_project
# Objective:
In this project, we investigate a classic but still dangerous machine-learning illusion:

A model can perform beautifully inside the region where it has seen data and fail quietly outside it.

This matters because in materials problems, descriptor space is rarely sampled uniformly. Some regions are dense, others sparse, and some are not represented at all. A model that interpolates well may still have no real knowledge beyond its training support.

# Methodology:
We used the following methods to identify where a model is interpolating and where its extrapolating
1) Gaussian Process regression: We train the GPR model on the data that we have, Along with every prediction, GPR provides a standard deviation to quantify uncertainity.We then measure the GPR uncertainity across the entire descriptor space. 
2) Convex Hull method: By calculating the Convex Hull of a training data, we can try to determine if the new test point is interpolation or extrapolation. We also calculate RMSE inside the hull and outside the hull based on the GPR prediction that we have.
3) Kernel density estimation: While Convex Hull provides a binary classification(inside vs outside), KDE can help idenify holes in the descriptor space where data might be sparse even if technically inside the hull. We also calculate RMSE inside the low KDE regions and high KDE regions based on the GPR prediction that we have.
   
