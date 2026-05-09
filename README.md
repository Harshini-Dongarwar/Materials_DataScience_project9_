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
   
4) First, we train the GPR using all the training data that we have and get the following observations:
   <img width="547" height="470" alt="image" src="https://github.com/user-attachments/assets/be4c17c6-da84-4576-af9a-fe12eeb8c950" /> <img width="536" height="451" alt="image" src="https://github.com/user-attachments/assets/c683f65a-23cd-4343-9f72-ed7532cadc5c" />

OBSERVATION: GPR gives a smooth prediction, even at places where we dont have any training data.
Now,lets look at the uncertainity plot:<img width="524" height="451" alt="image" src="https://github.com/user-attachments/assets/776cbbcd-d2bb-4c40-8ff4-a26d6400302b" />
We notice that as we go away from the training data, the uncertainity increases.

Convex hull was next used to draw a boundary for interpolation and extrapolation:
<img width="553" height="470" alt="image" src="https://github.com/user-attachments/assets/a90f1f6d-c226-439b-865d-bde46a1af452" />
Here we notice that the hole was included as an interpolation region.

Next, KDE was used to


Next we performed:Train,Validation,Test Split for Model Evaluation

The earlier analyses focused primarily on understanding prediction behavior, uncertainty distribution, and interpolation versus extrapolation across the full descriptor domain.

To evaluate the generalization performance of the Gaussian Process model on unseen data, the dataset was next divided into:

training, validation, and test subsets





   
