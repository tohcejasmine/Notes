# Summary of Modules Taken

## AY2016/2017 Semester 1 (Y1S1)
1. CL2281 - Translation and Interpretation
2. FMS1204S - Freshmen Seminar: Fraud, Deception and Data
3. GER1000 - Quantitative Reasoning
4. MA1101R - Linear Algebra
5. MA1102R - Calculus

## AY2016/2017 Semester 2 (Y1S2)
1. CS1010S - Programming Methodology
2. DSA1101 - Introduction to Data Science
3. GEQ1000 - Asking Questions
4. GES1028 - Singapore Society
5. SP1541 - Exploring Science Communication Through Popular Science
6. ST2131 - Probability

## AY2017/2018 Semester 1 (Y2S1)
1. CFG1010 - Roots and Wings - Personal and Interpersonal Effectiveness 1.0
2. CS1020 - Data Structures and Algorithms I
3. DSA2101 - Essential Data Analytics Tools: Data Visualisation
4. MA1100 - Fundamental Concepts of Mathematics
5. MA2311 - Techniques in Advanced Calculus
6. ST2132 - Mathematical Statistics

## AY2017/2018 Semester 2 (Y2S2)
1. ALS1010 - Learning to Learn Better
2. CS2010 - Data Structures and Algorithms II
3. CS2102 - Database Systems
4. DSA2102 - Essential Data Analytics Tools: Numerical Computation
5. GEH1019 - Food & Health
6. GET1004 - Cyber Security

## AY2018/2019 Semester 1 (Y3S1)
1. [CS3244 - Machine Learning](#cs3244---machine-learning)
2. DSA3101 - Data Science in Practice
3. DSA3102 - Essential Data Analytics Tools: Convex Optimisation
4. ST3131 - Regression Analysis
5. [ST3248 - Statistical Learning I](#st3248---statistical-learning-i)

## AY2018/2019 Semester 2 (Y3S2), Student Exchange Programme, Victoria University of Wellington
1. BIOL113 - Biology of Plants (LSM1991 - Exchange Enrichment Module)
2. MAOR123 - Maori Society and Culture (SC1741 - Department Exchange Module)
3. STAT392 - Sample Surveys (ST3239 - Survey Methodology)
4. SWEN303 - User Interface Design (CS3240 - Interaction Design)

## AY2019/2020 Semester 1 (Y4S1)
1. CS2103 - Software Engineering
2. DSA4199 - Honours Project in Data Science and Analytics (tbc)
3. DSA4211 - High-dimensional Statistical Analysis
4. [MA4270 - Data Modelling and Computation](#ma4270---data-modelling-and-computation)


## AY2019/2020 Semester 2 (Y4S2)
1. DSA4199 - Honours Project in Data Science and Analytics
2. DSA4212 - Optimisation for Large-scale Data-driven Inference
3. ST4234 - Bayesian Statistics

# Description of Modules

## CS3244 - Machine Learning

1. Naive Bayes
2. K-Nearest Neighbours (KNN)
3. Perceptron Learning Algorithm (PLA)
4. Linear and Logistic Regression
5. Bias, Variance and Overfitting
6. Regularisation, Cross-validation
7. Neural networks
8. Deep learning
    * RNN, CNN, DNN
9. SVM
10. Decision trees, ensembling
11. K-means clustering
12. PCA, Expectation-Maximisation algorithms

## MA4270 - Data Modelling and Computation

1. Perceptron
    * Linear classifier (only 2 categories)
    * Passes through origin
    * Function: f(x_n) = sign(theta^T_n * x_n) = sign(LC of weights * variables)
    * Decision boundary: where function value = 0
2. Support Vector Machine (SVM)
    * Maximum margin classifier
    * Basic SVM (only linearly separable data)
    * General SVM
        * with Offset (more flexibility, not restricted to passing through origin)
        * with Slack variables (allows for soft margin, misclassified examples)
    * Support vectors are points lying on margin/boundary (and those violating margin, in the case of soft margin SVMs)
3. Bias and Variance
    * Trade-off
        * Big n (more data), smaller variance but higher bias (overfit)
        * Small n, higher variance but lower bias
    * Bias-Variance decomposition
        * MSE = squared bias + variance
    * Balancing bias and variance to reduce overall MSE
4. Linear Regression
    * Fit using maximum likelihood estimation (maximise log-likelihood)
        * Not good when n is small (overfit)
    * Fit using least squares estimation (minimise average squared error/residual sum of squares)
        * Has closed form solution
5. Regularisation
    * To avoid overfitting
6. Kernel Methods
    * Feature map: map each input to a well-defined feature space
    * Kernel function: inner product of two inputs
    * Types of kernels
        
        1. Polynomial kernels
        2. String kernels
        3. Radial Basis function (RBF kernel)
    * Examples:
        * Kernelised linear regression
        * K-nearest Neighbours (KNN)
7. Convex Optimisation
    * General problem: find a set of inputs such that it minimises a convex function subject to inequality and equality constrains
    * Convert general problem to Lagrange Dual Problem (simplify general problem into one single function)
8. Dual SVM
    * Examples:
        * SVM with hard margin and offset (find weights of classifier that can maximise margin subject to constraints)
        * SVM with soft margin and offset (non-separable case)
        * Kernelised Dual SVM
    * Computationally intensive when n large
10. Boosting
    * Combine simple classifiers to produce a complex classifier
    * Example of simple classifiers:
        * Decision stump (function = sign(x_k - threshold))
    * Example of complex classifier:
        * Weighted decision function
    * Boosting finds good thresholds and weights for complex classifier
    * Example:
        * Adaboost
            
            1. Initialise weights
            2. Choose thresholds that minimise weight training error
            3. Points with lower error gets higher vote
            4. Update weights using formula involving current weights, vote value and normalising constant (higher weight if incorrect classfication)
            5. Iterate for a specified number of rounds (Step 2-4)
            6. Final classifier is weighted sum of decision stump
    
11. Statistical Learning Theory
    * True Risk/Expected Loss (test error)
    * Empirical Risk (train error)
    * Risk decomposition
        * Test error = Train error + generalisation error
        * Train error large if underfit
        * Generalisation error large if overfit
12. Unsupervised Learning
    * K-means clustering

        1. Initialise cluster means randomly
        2. Assign data point to closest cluster mean
        3. Update cluster mean
        4. Iterate through Step 2 and 3 (alternating optimisation)
13. Distributed learning
    * Density estimation (mean, variance)

## ST3248 - Statistical Learning I

1. Linear Regression
    * Simple linear regression
    * Multiple linear regression
    * Considerations in regression model
2. Classification
    * Logistic regression
    * LDA, QDA
3. Resampling methods
4. Linear model selection and regularisation
    * Subset selection
    * Shrinkage methods (RR, LASSO)
    * Dimension reduction methods
5. Unsupervised learning
    * Principal component analysis
    * K-means clustering
    * Hierarchical clustering




# Summary

* Classification
    * Output: classified category/group
* Regression
    * Output: real-value number
* Visualisation
    * Relationship between variables